---
layout: post
title: Wie man einen Nello One nicht von der Cloud befreit
date: 2018-12-04 17:07
author: Hypfer
category: posts
---

<p>Beim Nello One handelt es sich um ein Ding, welches es ermöglichen soll die Haustür in einem Mehrfamilienhaus "smart" zu machen.<br>Mittels einer Handyapp gibt es Informationen darüber wann geklingelt wird, sowie die Möglichkeit die Tür remote zu öffnen.<br><br>Dazu bietet Nello noch einige Kooperationen mit Lieferdiensten und Paketdienstleistern um Lieferungen zu ermöglichen, obwohl niemand zuhause ist.</p>



<p>Nette Sache. Wo ist der Haken?<br>Die Cloud.&nbsp;☁<br>Das Ganze gibt es natürlich nur mit einem Account beim Hersteller. Eine Offlinenutzung ist nicht vorgesehen.</p>



<p><strong>Disclaimer:<br></strong>Dieser Artikel liegt schon etwas länger hier rum. Es kann also sein, dass die Informationen nicht mehr korrekt sind.</p>




<p>Okay, es handelt sich um ein kleines Start-up. Die Zielgruppe des Produkts ist ohnehin beschränkt. Da jetzt auch noch die Möglichkeit der Offline-Nutzung einzubauen würde Resourcen benötigen die schlicht und ergreifend nicht vorhanden sind.<br>Dazu sind die Nutzerdaten dieser Zielgruppe natürlich noch verdammt geil denn Zeug auf dem "Smart" steht verkauft sich wie geschnitten Brot. Da kann man gar nicht genug Datensätze zu haben.</p>


<!-- wp:heading -->
<h2>Die Analyse</h2>
<!-- /wp:heading -->


<p>Gut, dacht ich mir, bevor du das Ding orderst schaust du mal wie weit du kommst. Zunächst ein Blick in die AGB. Reverse Engineering wird dort nicht ausgeschlossen. 👍<br><br><a href="https://www.charlesproxy.com/">Charles Proxy</a> gestartet, die App des Herstellers auf einem Android Handy mit Android 5 installiert und dann einfach mal mit der Konfiguration angefangen und das ganze mitgesnifft.<br><br>Warum Android 5? Seit Android 6 oder 7 muss man die APK von Hand bearbeiten (oder <a href="https://github.com/levyitay/AddSecurityExceptionAndroid">dieses Skript</a> nutzen) wenn man möchte, dass sie ein falsches CA Zertifikat akzeptiert.<br></p>



<p>Aus dem mitgesnifften Traffic dann eben mit Node/Express einen Mock Server mit HTTPS erstellt, fake CA Cert auf dem Handy installiert und schon war die echte App nutzbar um dem Nello One auch ohne Anlage eines Cloud-Accounts die W-Lan Zugangsdaten mitzuteilen.<br><br>An diesem Punkt habe ich dann, in Erwartung, dass es so leicht bleiben würde, eines dieser Geräte geordert.</p>



<p><strong>Was habe ich bisher herausgefunden?</strong></p>



<ul><li>Das Backend hört auf <em>api.nello.io</em> und&nbsp;<em>live-mqtt.nello.io</em>. Dazu gibts noch eine mqtt Testumgebung unter <em>mqtt.nello.io</em></li><li>Die Api ist ein REST irgendwas auf Basis von Python/Flask hinter einem NGINX auf Ubuntu VMs bei AWS EC2.&nbsp;&nbsp;<em>AWS hat eigentlich auch selbst Loadbalancer. Warum der NGINX?</em></li><li>Die Kommunikation des Nello One mit dem Server des Herstellers erfolgt mittels MQTT ohne SSL auf dem default port 1883</li></ul>


<!-- wp:heading -->
<h2>Das Backend</h2>
<!-- /wp:heading -->


<p>Generell geben die Endpunkte von <em>api.nello.io</em> anscheinend immer JSON zurück. Ich habe nur HTTP Statuscode 200 beobachten können.<br><br>Innerhalb dieses JSON gibt es dann ein result object mit einer property namens "status". Diese enthält bunt gemischt numerische HTTP Statuscodes wie z.B. 404 als <em><strong>String</strong></em> oder auch mal ganz andere Strings wie "OK".<br></p>



<p>Für den Login gibt es den Endpunkt <em>/login</em>. Gibt man dort inkorrekte Daten ein antwortet der Server wie folgt:</p>



<p><code>{<br>&nbsp; &nbsp; "authentication": false,<br>&nbsp; &nbsp; "result": {<br>&nbsp; &nbsp; &nbsp; &nbsp; "message": "Username/Password problem.",<br>&nbsp; &nbsp; &nbsp; &nbsp; "status": "OK"<br>&nbsp; &nbsp; }<br>}</code></p>



<p>Passt. Verrät einem Angreifer nicht was falsch war. So soll es sein...<br></p>


<!-- wp:heading {"level":4} -->
<h4><strong>Sicherheitsbedenken</strong></h4>
<!-- /wp:heading -->


<p>... wäre da nicht der <em>/check-email </em>Endpunkt.<br><br>Dieser existiert, um bei der Neuanlage eines Accounts zu prüfen, ob die Email bereits in der Datenbank existiert.<br>Hier ein POST ohne Authentifizierung o.ä. aber dafür mit einer Email-Adresse an <em>/check-email</em>:</p>



<p><code>{<br>&nbsp; &nbsp; "has_password": true,<br>&nbsp; &nbsp; "password_reset": false,<br>&nbsp; &nbsp; "result": {<br>&nbsp; &nbsp; &nbsp; &nbsp; "message": "User already exists",<br>&nbsp; &nbsp; &nbsp; &nbsp; "status": "200"<br>&nbsp; &nbsp; },<br>&nbsp; &nbsp; "user_exists": true,<br>&nbsp; &nbsp; "user_id": "64750b31-9f15-4f6f-b712-5ce4598abe01"<br>}<br></code></p>



<p>Oh ein Datenleck. Sogar noch mit user_id frei Haus dazu.<br></p>


<!-- wp:heading {"level":4} -->
<h4>Weitere Erkenntnisse</h4>
<!-- /wp:heading -->


<p>Grundsätzlich existiert keine Validierung der Emailadresse innerhalb der App und wohl auch nicht Serverseitig. <br>Die Anlage eines Accounts mit der Emailadresse "foobar" führte zu einem Fehler bei der Anlage und gleichzeitig zu einem halb angelegten Account für den ein Login möglich war.<br><br>Emailadressen, die auf @example.com enden lassen einen immerhin im Dialog in der App weiter vorankommen.<br>Dazu scheint es generell derzeit an einigen Stellen in der App kein Handling von Timeouts zu geben. Die stürzt dann gerne mal ab.&nbsp;<br><br>Soweit zu der ersten Analyse ohne Gerät.</p>


<!-- wp:heading -->
<h2>Das Gerät</h2>
<!-- /wp:heading -->


<p>Irgendwann kam der Nello One dann auch mit der Post. Ausgepackt, eingebaut, konfiguriert.<br><strong>Hier die Erkenntnisse in Kurzfassung:</strong><br></p>



<ul><li>Jedes Nello One scheint eine eindeutige ID zu haben. Diese steht nicht auf der Verpackung und dient als infix der zugehörigen mqtt topics.</li><li>Bei der ersten Verbindung zum W-Lan subscribed das Gerät zu allerhand Topics:&nbsp;/nello_one/FFFFFF/{test,BE_ACK,tw,geo,door,BEn}/</li><li>Danach publisht es eine Nachricht mit irgendwelchem Base64 und QoS 1 auf das Topic /nello_one/FFFFFF/map.</li><li>Daraufhin publisht das nello Backend eine Nachricht mit irgendwelchem anderen Base64, trailing newline und QoS 1 auf das Topic /nello_one/FFFFFF/test</li><li>Wenn dies korrekt war beginnt die LED des Nello One zu blinken.</li></ul>



<p>Nun folgt der weitere Einrichtungsdialog, welchen ich jedoch nicht ohne Cloud replizieren konnte, da für das gleiche Kommando bei jeder Wiederholung eine andere Kontrollnachricht auf das topic /nello_one/FFFFFF/BEn gepublished wurde.</p>



<p>Ich nehme an, dass bei der ersten Registrierung irgendetwas ausgetauscht wird um Replay-Angriffe zu verhindern. Der mqtt Server ist schließlich öffentlich und anscheinend kann dort jeder ohne Authentifizierung Nachrichten publishen.</p>


<!-- wp:heading -->
<h2>Die Kapitulation</h2>
<!-- /wp:heading -->


<p>An dieser Stelle verlor ich dann aber auch das Interesse. Der Aufwand stand für mich leider in keinem Verhältnis zum Nutzen 🙁<br><br>Vielleicht hat ja jemand anderes Lust sich das näher anzusehen.</p>


<!-- wp:heading {"level":4} -->
<h4>Weitere Gedanken</h4>
<!-- /wp:heading -->


<p>Während meiner Recherche habe ich noch <a href="https://diliulyssis.blogspot.de/2016/11/porting-nordic-mqtt-client-to-ipv4.html">diesen Blogartikel</a> gefunden in dem ein ehemaliger Mitarbeiter des Unternehmens irgendwelche Details aus der Entwicklung festgehalten hat. Der Artikel lässt vermuten, dass es sich bei der Hardware um einen&nbsp;<em>Nordic nrf52 MCU</em> mit einem<em>&nbsp;Atmel winc1500 W-Lan uC</em> handelt.</p>

