---
layout: post
title: "Philips Hue: Alternative Netzteile ohne Fiepen und Garantieverlust"
date: 2019-01-30 17:33
author: Hypfer
category: posts
---

<p>Nicht-Retrofit Hue Leuchten haben leider alle eine gemeinsame Krankheit: Sie fiepen.<br><br>Warum? Weil das Netzteil völliger Müll ist. Philips tauscht zwar aus, aber die Austauschnetzteile sind ebenfalls Müll. Alle Originalnetzteile von Philips fiepen. Manche mehr, manche weniger. Jedoch niemals nie.<br><br>Da die Zielgruppe das zwar doof findet, sich aber dennoch damit abspeisen lässt, war es betriebswirtschaftlich auf jeden Fall die richtige Entscheidung bei einem 170€ Produkt die paar Cent beim Netzteil einzusparen.<br><br>Ich weigere mich jedoch sowas einfach zu akzeptieren und habe daher eine andere Lösung gefunden, die sowohl das Problem löst, als auch die Garantie behält.<br><br><strong>Update:</strong><br>Mittlerweile existiert eine weitaus schönere Lösung mittels eines custom Adapter PCBs.<br>Zu finden ist sie hier: <a rel="noreferrer noopener" href="https://github.com/Hypfer/huesful-power-adaptor" target="_blank">https://github.com/Hypfer/huesful-power-adaptor</a></p>


<p>Der Hersteller war besonders clever. Er hat nicht nur (statt der sonst üblichen 5.5x2.5mm) eine 6.5x3mm Buchse verbaut, nein, er hat sogar das Gender der Anschlüsse getauscht.<br>Sprich an der Lampe ist ein Kabel mit einem Stecker, welches in das Netzteil mit Buchse führt.<br><br>Damit haben sie natürlich alle Käufer ausgetrickst, denn selbst wenn er einen Adapter für dieses exotische Format finden sollte ist dieser nur von Male 5.5x2.1mm auf 6.5x3mm und nicht umgekehrt.<br><br>Ich hätte wirklich kein Problem mit dieser Kundenbindung, wenn sie denn immerhin anständige Netzteile verkaufen würden. Tun sie aber nicht. Fiepen ist - egal wie viel Geld man auf das Problem wirft - alternativlos.</p>

<p>Einzige Bezugsquelle für passende Buchsen mit diesem exotischen Format sind Mouser oder DigiKey. Beide mit $22 shipping für Bestellungen unter $50 Warenwert. Das ist natürlich keine Option.</p>

<p>Einige Nutzer haben dann einfach die Schere in die Hand genommen und den Stecker abgeschnitten. Die freien Kabelenden kann man dann in ein normales 24v Netzteil einklemmen.<br>Funktioniert zwar, die Garantie ist jedoch weg. Also ebenfalls keine Option für ein brandneues 170€ Produkt.</p>

<p>Sehr wohl eine Option ist hingegen ein <a href="https://www.amazon.de/gp/product/B07G29BJJ8/">5.5x2.5mm Female to Female 50cm Kabel für 3.99€ von Amazon</a>.</p>



<figure><img src="/assets/img/2019/kabel1.png"/><figcaption>Dieses hier z.B.</figcaption></figure>



<p>Wie bekommt man jetzt 5.5mm auf 6mm?<br><br>Richtig.<br>Gewaltsam!</p>



<img src="/assets/img/2019/gewalt.png"/>



<p>Der ganze Eingriff ist eigentlich recht simpel:</p>



<ul><li>Isolierung entfernen </li><li>Viermal gegenüberliegend 3mm tief in den Metallmantel einschneiden </li><li>Das Originalkabel hineindrücken </li></ul>



<figure><img src="/assets/img/2019/kabel2.png"/><figcaption>Die Isolierung</figcaption></figure>



<figure><img src="/assets/img/2019/kabel3.png"/><figcaption>Keine Isolierung + erste Schnitte</figcaption></figure>



<figure><img src="/assets/img/2019/kabel4.png"/><figcaption>Der präparierte Stecker nach Kontakt mit dem Originalkabel</figcaption></figure>



<figure><img src="/assets/img/2019/kabel5.png"/><figcaption>Das Endergebnis<br></figcaption></figure>



<p>Dazu nun noch Schrumpfschlauch über die Kontaktstelle und schon kann man ein nicht fiependes 24V 1A Netzteil anschließen. <a href="https://www.amazon.de/gp/product/B01GRYFCKK/">Dieses z.B.</a></p>



<figure><img src="/assets/img/2019/kabel6.png"/><figcaption>Und so ist es VDE-konform. Ganz bestimmt sogar.</figcaption></figure>



<p>Sofern es Probleme mit der Lampe geben sollte, löst man einfach den Schrumpfschlauch, entfernt das Adapterkabel und ab damit zum Hersteller. </p>

