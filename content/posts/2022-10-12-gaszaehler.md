---
title: "Auf Irrwegen: Gaszähler automatisch auszulesen"
date: 2022-10-18T20:46:00+02:00
author: 'Daniel'
language: de
draft: false
keywords: 'Aqara, Kontakt, QMC5883, HMC5883, Gaszähler, Arduino, ESP32, Reed, Hall, Sensor'
description: "Gaszähler mit ESP32 und einem QMC5883 Sensor automatisch auslesen"

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
Nachdem klar war, dass der Sensor des Aqara-Kontaktes nicht fein genug ist, um den Magneten zu registrieren, habe ich nach Alternativen geschaut. Die Optionen:

- Reed-Kontakt
- Hall-Sensor
- Kompass/Magnetometer

Nachdem ich mit dem Reed-Kontakt im Aqara-Sensor schon auf die Nase gefallen war, habe ich zunächst in Richtung Hall-Sensor überlegt. Auch hier finden sich aber unterschiedliche Berichte, welcher Sensor für diesen Zweck jetzt empflindlich genug ist. Zu guter Letzt habe ich aber ein Projekt gefunden, wo ein [Magnetometer HMC5883 mit Raspberry Pi](https://www.kompf.de/tech/gascountmag.html) die Aufgabe übernimmt. Die Überlegung, dass ein Sensor, der das Erdmagnetfelt detektieren kann, auch den Magneten im Gaszähler detektieren können sollte, fand ich plausibel.  

<figure style="font-size:small;width:500px;margin:auto">
  <img src="/images/gaszaehler/soldering.jpeg" style="">
  <figcaption>Der Sensor QMC5883 wird mit Stifleisten versehen</figcaption>
</figure>

Nachdem ich den ersten Sensor beim Anlöten der Stiftleisten frittiert hatte und die Ersatzlieferung eingetroffen war, konnte das Projekt umgesetzt werden.

Material:

- ESP32 Microprozessor
- QMC5833 Kompass
- Breadboard
- Jumperkabel

Werkzeug:
- Lötkolben

Die Idee: Der ESP32 wählt sich ins Wifi ein und übermittelt die Impulse via MQTT an meine Iobroker-Instanz, wo bei jedem Impuls der Zählerstand um 0,1m³ erhöht wird.

<figure style="font-size:small;width:640px;margin:auto">
<video width="640" height="360" controls>
  <source src="/images/gaszaehler/GasMeter.mp4" type="video/mp4">
</video>  <figcaption>Springt die zweite Nachkommastelle von 6 auf 7 (rechts im Video), erkennt der Kompass einen starken Ausschlag (links im Video). So lassen sich wiederum Inkremente der ersten Nachkommastelle detektieren</figcaption>
</figure>

Das Sketch dafür ist relativ simpel: Im Wesentlichen werden 10 Sekunden lang Messwerte des Magnetfeldes gesammelt. Da der Sensor relativ empfindlich ist und die Werte bei Erkennung des Magneten überlaufen können, wird die Signalstärke auf 4000 beschränkt. Ist der Durchschnittswert der letzten 10 Sekunden > 2000, wird das `high` Flag gesetzt. Ist der Durchschnittswert der letzten 10 Sekunden kleiner 300 und das High-Flag ist gesetzt, wurde eine Rotation erkannt, das `high` Flag wird entfernt und das Ganze via MQTT publiziert. Die Erkennung basiert also auf Anstieg und anschließendem Fall der gemessenen Feldstärke. Dieses Vorgehen erscheint mir relativ robust, weil es wenig Fehlauslösungen gibt - selbst wenn der Magnet längere Zeit direkt unter dem Sensor stehen bleibt. Das wäre anders, wenn der Trigger für `high` und der Trigger für `low` identisch wären - bei ungünstiger Positionierung des Magneten könnte der Sensor zwischen den Auslösewerten hin- und herspringen.


```c


void loop()
{
  loopMqtt();

  sVector_t mag = compass.readRaw();

  // calculate field strength & store for average calculation
  fieldStrength = sqrt(mag.XAxis ^ 2 + mag.YAxis ^ 2 + mag.ZAxis ^ 2);
  avg = avgFieldStrength.reading(fieldStrength > 4000 ? 4000 : fieldStrength);
  
  Serial.print("Arrow: "); Serial.println(fieldStrength);  

  // every 10 seconds: evaluate if signall falls from high (magnet detected) to low (no magnet)
  if ((unsigned long)(millis() - timer) > 10000) {
    if (avg < 300 and isHigh) {
      isHigh = false;
      client.publish("mqtt.0.gas_meter.tick", itoa(millis(), charBuf, 10));
    }
    
    if (avg > 2000) {
      isHigh = true;
    }

    timer = millis();
  }

  delay(500);
}
```
Das [komplette Skript findet sich auf Github](https://github.com/dnoegel/gasmeter-sensor). 

## Fazit
Insgesamt bin ich ziemlich zufrieden mit dem Ergebnis: Zumindest gibt es nach einem Tag noch keine Abweichung, der erfasste Zählerstand entspricht (bis auf die erste Nachkommastelle) genau dem tatsächlichen Zählerstand. Und via Iobroker/Jarvis kann ich den Tagesverbauch auch sehr leicht graphisch visualisieren lassen.

<figure style="font-size:small;width:100%;margin:auto">
  <img src="/images/gaszaehler/graph.png" style="">
  <figcaption>Nach 24 Stunden hat der Sensor alle 16 Ticks korrekt erfasst</figcaption>
</figure>
