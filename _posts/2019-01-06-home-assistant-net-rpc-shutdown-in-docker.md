---
layout: post
title: "Home Assistant: net rpc shutdown in Docker"
date: 2019-01-06 15:44
author: Hypfer
category: posts
---

<p>Die WoL Component von Home Assistant wäre nicht vollständig, wenn sie nicht auch dazu taugen würde den Rechner wieder auszuschalten.<br><br>Für einen Windows host geht dies über das Netzwerk mittels <code>net rpc shutdown</code>. Diese Binary ist Teil von samba-common-bin.<br>samba-common-bin ist jedoch nicht Teil des offiziellen Docker-Images von Home assistant :-/<br><br>Wie also das Problem lösen?</p>




<p>Was ist die einfachste Möglichkeit eine Binary ohne weitere Abhängigkeiten zu bekommen? Richtig. Statisch kompilieren.<br><br>Mehr als 2h später hatte ich dann auch eine pseudo-statische <code>net</code> binary, welche ich innerhalb des Docker Images mittels mount verfügbar machen konnte.<br>Shutdown funktioniert. Alles super. Schlafen.</p>



<p>Am nächsten Morgen fiel mir dann auf, dass ich ja Docker nutze und das ganze doch eigentlich viel einfacher gehen müsste.<br><br>Und in der Tat:<br>Jo, geht viel einfacher.</p>


<!-- wp:quote -->
<blockquote class="wp-block-quote"><p>FROM homeassistant/home-assistant:latest<br>RUN apt-get update &amp;&amp; apt-get install -y samba-common-bin</p><cite>custom_homeassistant.dockerfile</cite></blockquote>
<!-- /wp:quote -->


<p>Die <code>CMD</code> Direktive (welche Home assistant startet), sowie auch alle weiteren <code>RUN</code> Kommandos werden natürlich vom offiziellen Image vererbt.<br><br>Hätte mir doch nur jemand früher gesagt, dass ich Docker nutze.</p>


<!-- wp:quote -->
<blockquote class="wp-block-quote"><p>version: '2'<br> services:<br>     homeassistant:<br>         build:<br>           context: .<br>           dockerfile: custom_homeassistant.dockerfile</p><cite>Bonus: docker_compose.yaml</cite></blockquote>
<!-- /wp:quote -->


<p><br></p>

