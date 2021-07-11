---
layout: post
title: Der legale Weg zum Forza Horizon 4 Soundtrack
date: 2018-12-04 16:27
author: Hypfer
category: posts
---

<p><del>Ich spiele gerne Rennspiele und fahre daher folgerichtig auch gerne Auto.</del><br>Ich fahre gerne Auto und spiele daher folgerichtig auch gerne Rennspiele. <br>Insbesondere die mit offener Spielwelt in der man endlos untermalt vom Soundtrack umherfahren kann. Forza Horizon 4 ist da so ein Kandidat<br><br>Diese meditative Erfahrung möchte ich natürlich auch in die reale Welt übertragen. Leider gibt es den FH4 Soundtrack nicht online fürs Auto zu kaufen.<br>Ich besitze aber die Demo :^)<br></p>




<p>Im Spiel läuft ja die Musik, also muss sie auch irgendwo auf meinem Computer sein. Ist sie auch. Tief versteckt und geschützt in D:\WindowsApps<br><br>Microsoft versucht mit UWP alles in seiner Macht stehende um die Assets der Spiele zu schützen. Weder als TrustedInstaller noch über ein Live Linux war es mir möglich Dateien aus diesem Ordner zu kopieren.<br>Wie es das tut ist mir nicht ganz klar. Irgendwas mit Filesystem level encryption.<br><br>Ich muss es aber auch gar nicht wissen. Jemand anders hat sich damit bereits befasst und eine Lösung gefunden:&nbsp;<a href="https://github.com/Wunkolo/UWPDumper">https://github.com/Wunkolo/UWPDumper</a><br><br>Anstatt die ACLs auszuhebeln injected man einfach eigenen Code in die Anwendung, die die Assets sowieso lesen darf: Die FH4 Demo selbst.<br></p>



<p>Der Dumper tut genau das was der Name vermuten lässt: Er dumpt alle Assets an einen Ort der für normale Nutzer lesbar ist.<br><br>Für den Soundtrack relevant sind hierbei nur eine Handvoll Files:<br><em>R1_Tracks.bank</em> bis <em>R6_Tracks.bank</em> und <em>RadioInfo_DE.xml</em> für die Metainformationen.</p>



<p>In diesen Dateien mit <em>.bank</em> Dateiendung befinden sich sich Soundbanks im&nbsp;<em>FMOD Sample Bank</em> Format. Scheint wohl so eine Middleware/Library von einem Drittanbieter zu sein damit nicht jeder Spielentwickler das Rad neu erfinden muss wenn es um Audio Assets geht.<br>Durch diese große Verbreitung wurde das Format natürlich schon lange reversed und Tools zum entpacken sind frei verfügbar.</p>



<p>Um die <em>fsb Soundbank</em> entpacken zu können muss man zunächst aber mittels des folgenden QuickBMS Skripts aus der .bank-Datei bekommen:</p>


<!-- wp:quote -->
<blockquote class="wp-block-quote"><p>for OFFSET = 0<br>goto OFFSET<br>findloc OFFSET string "FSB5"<br>goto OFFSET<br>getdstring FSB_SIGN 4 # FSOUND_FSB_HEADER_FSB5 (fsb.h)<br>get version long<br>get numsamples long<br>get shdrsize long<br>get namesize long<br>get datasize long<br>xmath SIZE "0x3c + shdrsize + namesize + datasize"<br>log "" OFFSET SIZE<br>next OFFSET + SIZE</p><cite>Woher das Skript kommt ist für mich leider nicht so recht nachvollziehbar.<br>Vermutlich ein Werk des Autors von QuickBMS.</cite></blockquote>
<!-- /wp:quote -->


<p>Bei <a href="https://aluigi.altervista.org/quickbms.htm">QuickBMS</a> scheint es sich um eine Art Baukasten für diverse RE Aufgaben mit unbekannten Dateitypen zu handeln.<br>Kann man mal im Hinterkopf behalten und beizeiten näher betrachten. Scheint sehr nützlich und vor allem mächtig zu sein.</p>



<p>Mit den <em>fsb Soundbanks</em> braucht es dann nur noch eine&nbsp;<em>fsb_aud_extr.exe</em>, welche unter diesem Namen durch das Internet geistert, um am Ende einen Ordner voller .wav Dateien zu haben. Die Musik selbst liegt innerhalb der fsb als Vorbis mit knapp über 100kbps. 320k ist für MP3s aus diesen WAV-Dateien also nicht sonderlich zielführend.</p>



<p>Nun folgt viel Fleißarbeit in der die nicht weiter mit Metainformationen versehenen WAV-Dateien den richtigen Titelnamen aus der&nbsp;<br><em>RadioInfo_DE.xml</em> zugeordnet bekommen müssen.<br><br>~3h später hat man dann sein Ziel erreicht und kann den FH4 Soundtrack auch im echten Auto hören. Das ganze funktioniert übrigens genau so auch mit FH3.</p>

