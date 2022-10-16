---
title: "Auf Irrwegen: Vom Versuch meinen Gaszähler automatisch auszulesen"
date: 2022-10-16T19:21:00+02:00
author: 'Daniel'
language: de
draft: true
---
Smart Home und der mitleidige Blick meiner Ehefrau, wenn ich ihr mal wieder eine Automatisierung oder eine neue Sensorüberwachung präsentiere: Diese beiden Sachen gehen Hand in Hand. Das war auch wieder bei diesem Projekt der Fall: Der automatischen Überwachung des Gaszählers.

## Warum das Ganze
Ich erfasse immer schon monatlich unseren Strom- und Gasverbrauch. Bisher händisch: Einmal im Monat ein Foto von beiden Zählern - und bei Zeiten dann in Excel eingepflegt. Fertig. Im Rahmen der Dämmung unseres Hauses sowie der Heizlastberechnung wollte ich aber genauer verstehen, wie (schnell) sich der Erdgasverbauch entwickelt. 

Wie so oft: Das Ganze war doch komplizierter als gedacht.

## Das Grundprinzip
In einigen Gaszählern - so auch in meinem G4 RF1 - finden sich Magneten in einer Walze der Segmentanzeige. Positioniert man einen Reed-Kontakt an die richtige Position, kann dieser recht zuverlässig detektieren, wenn sich die Walze beispielsweise von 9 nach 0 weiter dreht, weil dann eben auch der Magnet am Kontakt vorbeigeführt wird.  Passende Kontakte / Sensoren gibt es direkt vom Hersteller - der lässt sich das mit 50-60€ aber auch gut vergüten. In solchen Fällen ist "selber machen" immer eine gute Option - und auch mindestens eine ebensogute Ausrede, den gleichen Betrag in Zubehör und Werkzeug zur Umsetzung des Projektes zu investieren ;-).   

In meinem Fall ist auf dem Gaszähler übrigens vermerkt, dass 1 Impuls 0,1m³ entspricht - demzufolge findet sich der Magnet auf der zweiten Walze nach dem Komma (eine volle Umdrehung der zweiten Nachkommastelle entspricht einem 1er-Inkrement der ersten Nachkommastelle). 

## Versuch 1: Der Aqara Tür- und Fensterkontakt
Im Netz finden sich verschiedene Berichte, dass ein Aqara Tür- und Fensterkontakt genutzt werden kann, um die Impulse zu messen. Diese Lösung wäre natürlich besonders charmant, da ich (a) solche Kontakte ohnehin zu Hause habe, (b) die Sensoren sehr lange batteriebetrieben laufen und sie (c) sehr einfach über Zigbee in das bestehende Hausautomatisierungssystem eingebunden werden können.

Die Idee ist einfach: Sensor aus der Plastikhülle befreien, an die richtige Stelle auf den Gaszähler platzieren - fertig…

…oder auch nicht.

<div class="gallery-grid">

<figure style="font-size:small;">
  <img src="/images/gaszaehler/closed.jpeg" style="">
  <figcaption>Aquara Tür- und Fenstersensor im Auslieferungszustand</figcaption>
</figure>

<figure style="font-size:small;">
  <img src="/images/gaszaehler/open1.jpeg" style="">
  <figcaption>Die untere Kappe des Kontaktes lässt sich leicht öffnen</figcaption>
</figure>

<figure style="font-size:small;">
  <img src="/images/gaszaehler/open2.jpeg" style="">
  <figcaption>Mit einem Messer lässt sich auch leicht der Rest zerstörungsfrei öffnen (anders als im Bild gezeigt ;-))</figcaption>
</figure>

<figure style="font-size:small;">
  <img src="/images/gaszaehler/implemented.jpeg" style="">
  <figcaption>Der Sensor wird in die entsprechende Aufnahme platziert. Das Glasröhrchen mit dem Reed-Kontakt sollte grob oberhalb der zweiten Nachkommastelle liegen, da sich in dieser Walze der Magnet befindet</figcaption>
</figure>

<figure style="font-size:small;">
  <img src="/images/gaszaehler/implemented2.png" style="">
  <figcaption>Kein Signal: Die nächsten Tage habe ich den Sensor in alle möglichen und unmöglichen Positionen und Ausrichtungen an den Gaszähler geklebt. Ohne Erfolg</figcaption>
</figure>
</div>

Trotz aller Bemühungen - und gut 10 abenteuerlichen Klebepositionen: Der Sensor erkennt den Magneten nicht, egal wie oft dieser im Betrieb am Sensor vorüber zieht. Das Problem ist bekannt - nicht jede Kombination aus Gaszählerfabrikat und Aqara-Sensor scheint zu funktionieren.

## Versuch 2: IP Webcam
Im nächsten Versuch sollte ein altes Handy als IP Webcam herhalten - ehrlicherweise nur um die Zeit zu überbrücken, bis das Zubehör für Versuch 3 eingetroffen ist. Mit einer [entsprechenden App](https://play.google.com/store/apps/details?id=com.pas.webcam&hl=de&gl=US) lassen sich alte Android Geräte relativ leicht als IP Webcam betreiben. Die App bietet umfangreiche Konfigurationsoptionen, letztlich benötige ich aber nur folgende Gegebenheiten:

- Bild lässt sich via URL auslesen
- LED-Licht lässt sich via HTTP-API ein- und abschalten

So bekomme ich nun stündlich einen aktuellen Zählerstand via Telegram-Nachricht mitgeteilt - dank LED Licht auch in der Nacht gut lesbar. Nur aus Interesse habe ich versucht, die Bilder auch automatisiert auszuwerten - zumindest via [SSOCR](https://www.unix-ag.uni-kl.de/~auerswal/ssocr/) ist es mir aber nicht gelungen. Würde man so etwas zum Laufen bringen *wollen* - ein Blick auf _[AI on the edge](https://github.com/jomjol/AI-on-the-edge-device)_ wäre sicher lohnenswert: Dort werden Wasserzähler mittels ESP32 und einem neuronalem Netzwerk ausgelesen. Entsprechende Trainingsdaten inklusive…

<div class="gallery-grid">


<figure style="font-size:small;">
  <img src="/images/gaszaehler/handy.png" style="">
  <figcaption>Ein ausgemustertes Handy überwacht den Gaszähler</figcaption>
</figure>


<figure style="font-size:small;">

  <img src="/images/gaszaehler/in.png" style="margin-top:50px;">
  <figcaption>Der Versuch die Bilder automatisch zu verarbeiten…</figcaption>
  <img src="/images/gaszaehler/out.png" style=" margin-bottom: 50px">
  <figcaption>scheitert aber: SSOCR erkennt maximal 3 Ziffern</figcaption>

</figure>


<figure style="font-size:small;">
  <img src="/images/gaszaehler/blockly.png" style="">
  <figcaption>Wenn es schon mit der automatischen Auswertung nicht klappt: Dieses Blockly schickt stündlich…</figcaption>
</figure>


<figure style="font-size:small;">
  <img src="/images/gaszaehler/telegram.png" style="">
  <figcaption>…Telegram-Nachrichten mit dem aktuellen Zählerstand 🥴</figcaption>
</figure>

</div>  

Eine Erkenntnis ist aber: Alte Handys lassen sich doch sehr gut als IP Webcam umfunktionieren: Bewegungserkennung, Zwei-Wege-Kommunikation, LED-Licht, Video, Automatische Aufnahmen… alles möglich.

## Versuch 3: Kompass