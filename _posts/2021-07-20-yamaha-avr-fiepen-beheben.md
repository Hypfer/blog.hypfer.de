---
layout: post
title: Yamaha AVR Standby Fiepen beheben am Beispiel des Aventage RX-A1050
date: 2021-07-20
author: Hypfer
category: posts
---

<figure>
    <img src="/assets/img/2021/avr.jpg"/>
    <figcaption>
        RX-A1050 mit Squeezebox und Canton CT 800
    </figcaption>
</figure>


Ich bin großer Fan meiner Yamaha AV-Receiver. Insbesondere aber nicht ausschließlich auch aufgrund der tollen Netzwerkintegration,
welche die vollständige Steuerung über IP ermöglicht.

Damit dies vernünftig funktioniert, muss das Gerät natürlich permanent am Strom- und IP-Netz hängen.
Sollte gerade bei dem Preis kein Problem sein würde man meinen.

## Problemstellung

Leider gibt es da jedoch ein Problem: der AV-Receiver fiept im Standby. Dies auch nicht nur ein bisschen, sondern deutlich selbst
aus mehreren Metern Distanz wahrnehmbar.

Dieses Problem scheint mich nicht alleine zu betreffen. Es gibt diverse weitere Google-results dazu.
Hier nur eine Auswahl:

- <a href="http://www.hifi-forum.de/viewthread-276-6228-48.html" target="_blank">http://www.hifi-forum.de/viewthread-276-6228-48.html</a>
- <a href="http://www.hifi-forum.de/viewthread-276-7854.html" target="_blank">http://www.hifi-forum.de/viewthread-276-7854.html</a>
- <a href="http://www.hifi-forum.de/viewthread-220-9068.html" target="_blank">http://www.hifi-forum.de/viewthread-220-9068.html</a>
- <a href="http://www.hifi-forum.de/viewthread-276-7720.html" target="_blank">http://www.hifi-forum.de/viewthread-276-7720.html</a>

Immer wieder wurde vorgeschlagen, doch irgendwelche Standby-features wie beispielsweise das Durchleiten des HDMI-Signals
ein- bzw. auszuschalten, um das Geräusch zu beheben, was teilweise auch zu verbesserungen führte.
In meinem Fall jedoch leider nicht.

Nur zufällig fiel mir auf, dass das Standby-Fiepen bei meinem RX-A1050 dann verschwand, wenn ich einen kleinen Monitor
an dessen USB-Port anschloss.
Weitere Tests haben dann ergeben, dass das Fiepen lastabhängig ist.
In meinem Fall verändert sich bis 0.3A der Klang. Ab ~0.45A ist dann Stille.

Damit ergeben die Vorschläge aus den Forenbeiträgen auch Sinn, denn diese Features zu verstellen verändert auch den Stromverbrauch.

Ebenfalls fiel mir auf, dass mein RX-A2010 im Standby niemals fiept, mein RX-A1080 jedoch schon.

## Erklärung

Was geschieht hier also?
Die Antwort lautet <a href="https://www.powersystemsdesign.com/articles/burst-mode-operation-a-double-edged-sword/31/14595" target="_blank">"Burst-Mode Operation"</a>.
Nun fehlt mir doch sehr der Hintergrund im electrical engineering, weshalb ich den gelinkten Artikel hier nicht noch weiter
inhaltlich detailliert zusammenfassen kann.

Ganz grundsätzlich braucht man aber nur zu wissen, dass zwecks Senkung des Standby-Stromverbrauchs sich das Netzteil intervallbasiert
abschaltet. Die Frequenz davon kann lastabhängig sein und insbesondere hörbare Artefakte erzeugen.

Hörbare Artefakte wie eben ein Fiepen im Standby.

Dies betrifft vermutlich keine älteren Geräte, da damals seitens des Gesetzgebers noch nicht so hohe Anforderungen an
den Standby-Stromverbrauch von Geräten existierten und dementsprechend kein Burst-Mode notwendig war.

## Lösung

Um dies zu lösen braucht man also nur die Last so weit erhöhen, dass das Netzteil vom Burst-Mode in den Continuous-Mode wechselt.
Schon hat man angenehme und insbesondere schlafzimmertaugliche Stille.

Meine persönliche Lösung für das Problem war es, drei 10W 3.6Ohm Widerstände mit Wärmeleitkleber an einen alten CPU Kühler aus einem
Server zu kleben, welcher noch so in der Parts-Kiste herumlag.

<figure>
    <img src="/assets/img/2021/dummyload.jpg"/>
    <figcaption>
        Die fertige Dummy-Load mit Anschlusskabel und wunderschöner Zugentlastung
    </figcaption>
</figure>

Bei den 5 - 5.6V (laut Board Beschriftung) die der AVR zur Verfügung steht, entsteht eine Last zwischen 0.46 und 0.51A und damit ein stilles Netzteil. 
Leider auch ein um 2-3W höherer Standby-Verbrauch, aber es ist eben alternativlos.

Ein größeres Problem war dann noch der Anschluss dieser Dummy-Load, da bis auf den Front-USB-Port des Receivers meines Wissens nach
keinerlei Anschlüsse mit +5V nach draußen gelegt sind.

Nachdem ich einen ungenutzten Header auf der Digitalplatine des RX-A1050 gefunden hatte, entschied ich mich dazu, den SMA
connector der W-Lan Antenne auszubauen und das dort entstandene Loch für die Kabeldurchführung zu nutzen.
Auf diese Weise waren keine permanenten Modifikationen sowohl am Gehäuse als auch an den PCBs vonnöten.

<figure>
    <img src="/assets/img/2021/digitalplatine1.jpg"/>
    <figcaption>
        Der vormals ungenutzte Header nun mit custom 3-Pin JST-XH Kabel für die Dummy-Load
    </figcaption>
</figure>

Um das ganze etwas schöner zu gestalten habe ich mich dazu entschieden, eine 3.5mm Audio Klinke in diesem Loch zu verbauen.
Dieses Format ist günstig, leicht zu bekommen und passte einwandfrei in die vorhandene Bohrung.

<figure>
    <img src="/assets/img/2021/avr_mit_anschluss.jpg"/>
    <figcaption>
        Die fertig verbaute 3.5mm Klinke
    </figcaption>
</figure>

An dieser Stell sei noch warnend erwähnt, dass insbesondere bei so einem teuren Gerät lieber 10-mal mehr als eigentlich nötig
gemessen werden sollte anstatt es nicht zu tun und deshalb entweder das Gerät, das Haus oder gar sich selbst zu zerstören.
Auch sollte auf die ausreichende Dimensionierung der genutzten Komponenten geachtet werden.

Alles an diesem Build ist Overkill.<br/>
Sowohl der Querschnitt der Zuleitung zur Load als auch die Load selbst mit 3x 10W Widerstand und einem 65W+ CPU Kühler.
Gerade deshalb kann ich mir jedoch sehr sicher sein, dass es sich nicht spontan dazu entscheiden wird, in Flammen aufzugehen,
und dadurch meine Wohnung und ggf. auch meine Existenz zu löschen.

## Fazit

Die gezeigte Lösung ist selbstverständlich nicht die einzige Möglichkeit um dieses Problem zu umgehen.
Sofern die Front des AVRs nicht sichtbar ist, könnte man sicherlich auch den USB-Port dafür nutzen und sich das Aufschrauben sparen.

In meinem Setup musste es jedoch von vorne schön aussehen.<br/>
Ich bin der Ansicht, dass die gewählte Lösung tatsächlich sehr sauber geworden ist.

Noch schöner wäre es natürlich, wenn man den Burst-Mode einfach in Software deaktivieren könnte.
Es ist jedoch nicht davon auszugehen, dass das jemals geschehen wird.
