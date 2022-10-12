---
title: "Heizlastberechnung - Teil 1: Heizlast eines Raumes ermitteln"
date: 2022-10-09T16:36:27+02:00
author: 'Daniel'
language: de
draft: true
--next: "2022-10-12-gaszaehler.md"
---
Energiekrise, Heizkostenentwicklung, Nachhaltigkeit: (Nicht nur) wer einen Altbau hat wird sich dieser Tage intensiv mit der Frage beschäftigen, wo man den Heizenergiebedarf des Hauses weiter optimieren kann und ob beispielsweise eine Wärmepumpe in Frage kommt. Mit einer sogenannten Heizlastberechnung ist es möglich, ein Modell des Heizenergiebedarfes eines Hauses zu entwickeln und zu verstehen, ob sich die vorhandenen Heizkörper beispielsweise für eine Wärmepumpe eignen. In meinem konkreten Fall lautet die Frage daher:

> Wieviel Energie werde ich nach der Dämmung der obersten Geschossdecke noch benötigen und eignet sich mein Altbau damit für eine Wärmepumpe?

## Ein Wort vorab
Ich bin nicht vom Fach - alle Angaben sind falsch ;-). Mein Ziel ist es nicht, eine 100% akurate Ermittlung der Heizlast meines Hauses vorzunehmen (die Heizlastberechnung an sich ist ja schon nur ein Modell): Sondern die grundlegenden Konzepte soweit verinnertlicht zu haben, dass ich näherungsweise verstehen kann, wie sich bspw. die Dämmung der ob ersten Geschossdecke auf meinen Heizwärmebedarf auswirkt.

Was wir hier machen ist nach meinem Verständnis eine Interpretation der Norm _DIN EN 12831-1: 
> Energetische Bewertung von Gebäuden - Verfahren zur Berechnung der Norm-Heizlast - Teil 1: Raumheizlast.
> 
Die Norm liegt mir nicht vor, und das Netz ist voller veralteter, falsch abgeschriebener und unvollständiger Informationen dazu, die ich nach besten Wissen und Gewissen neu arrangiere.

Zu dieser Norm gibt es auch einen Teil 2: 
> Heizungsanlagen in Gebäuden - Verfahren zur Berechnung der Norm-Heizlast - Beiblatt 2: Vereinfachtes Verfahren zur Ermittlung der Gebäude-Heizlast und der Wärmeerzeugerleistung
 
Der nach meinem Verständnis nicht mehr aktuell ist und sich auf Verbrauchsmessungen und Hüllflächenberechnungen stützt. Beim _IKZ: Magazin für Gebäude und Energietechnik_ wird die [vereinfachte Berechnung am Beispiel durchgeführt](https://www.ikz.de/uploads/media/022.pdf).

Weiterhin gibt es einen Teil 3:
> Energetische Bewertung von Gebäuden - Verfahren zur Berechnung der Norm-Heizlast - Teil 3: Trinkwassererwärmungsanlagen, Heizlast und Bedarfsbestimmung

Dieser befasst sich mit der Trinkwassererwärmung, die damit nicht Teil der hier geschilderten Heizlastberechnug ist. Hier kommt der Gesetzgeber mit einer Formel zur Hilfe: `Quadratmeterzahl Haus * 32 * 1,1`, die so [in §9 der Heizkostenverordnung vorgeschlagen wird](https://www.gesetze-im-internet.de/heizkostenv/__9.html). In meinem Fall komme ich auf `110 * 32 * 1,1 = 3.872kWh/a` was wiederum den `4.626 kWh/a` aus einem Energiegutachten recht nahe kommen. Alternativ könnte man auch überschlägig den Gasverbrauch eines Sommermonates (da ohne Heizwärme) mit 12 multiplizieren um auf den Jahresverbrauch zur Trinkwassererwärmung zu kommen; auch damit komme ich überschlägig auf den Wert aus dem Energiegutachten. 

## Was machen wir hier
- Heizlastberechnung
- Heizkörper
- Modell
- WP Auslegung




## Was ist eine Heizlastberechnung?
Wir berechnen die Heizlast für einen 23m2 großen Beispielraum.

## Wie setzt sich die Heizlast zusammen?

Die Heizlast eines Raumes ist die Summe folgender Wärmeverluste:

- **Transmissionswärmeverluste**: Wieviel Wärme verliert ein Raum durch Fenster, Decken und Wände nach außen oder in benachbarte Räume?
- **Lüftungswärmeverluste**: Wieviel Wärme verliert ein Raum durch Lüftung?
- **Aufheizleistung**: Wieviel Wärme wird benötigt um den Raum nach Absenkung/Nichtbenutzung wieder auf Temperatur zu bringen

### Transmissionswärmeverluste

Die Transmissionswärmeverluste sind die Wärmeverluste, die ein Raum durch Wände, Decken, Böden und Fenster zu anderen Räumen hat. Aus Gründen der Einfachheit werden Räume mit gleichem Temperaturnivau in der Rechnung nicht berücksichtigt (die Wand vom Wohnzimmer zum Esszimmer muss also nicht separat berücksichtigt werden, da man davon ausgeht, dass die die Wärmeverluste gegenseitig ausgleichen).

Die Heizlast für alle genannten Bauteille wird dabei wie folgt berechnet:

`Fläche des Bauchteils * Wärmedurchgangskoeffizient * Temperaturdifferenz` 

Der **Wärmedurchgangskoeffizient** ist der oft zitierte U-Wert. Er gibt an, wieviel Watt Wärmeleistung je Quadratmeter und Grad Kelvin bspw. draußen verloren geht. Einige Möglichkeiten an diesen U-Wert zu gelangen sind in [Heizlastberechnung - Teil 2: U-Wert ermitteln](/posts/2022-10-12-uwert-ermitteln) aufgeführt.


Die **Temperaturdifferenz** ist hierbei die Temperaturdifferenz zwischen dem betrachteten Raum (bspw. 21°C im Wohnzimmer) und dem angrenzenden Bereich (etwa 17°C im Schlafzimmer). Achtung: Für den Außenbereich wird die sogenannte [Normauslegungstemperatur](https://www.haustechnikverstehen.de/glossary/normaussentemperatur/) betrachtet. Diese lässt sich auf verschiedenen Webseiten nachschlagen, bspw. auf der [Klimakarte des Bundesverbandes Wärmepumpe e.V](https://www.waermepumpe.de/normen-technik/klimakarte/). Bei mir liegt die Normauslegungstemperatur -9°C. Im Wohnzimmer heize ich auf 21°C - die Temperaturdiffernz liegt damit bei 30°K. 

Ein Beispiel: 6,58m2 Fensterfläche mit einem U-Wert von 1,9 W/(m2*K) bei einer Temperaturdifferenz von 30°K:

`6,58m2 * 1,9 W/(m2*K) * 30K = 375,06 W`

Der Temperaturverlust der Fensterfläche beträgt also 375,06 Watt. 

Wollen wir die Transmissionswärmeverluste für unseren 23m2 großen Beispielraum berechnen, müssen natürlich noch Decke, Boden, Tür zum Flur etc. betrachtet werden. Dies könnte bspw. wie folgt aussehen:

Bauteil        | Fläche (m2) | U-Wert (W/m2*K) |  Temperatur-Differenz (°K) | Verlust (W) |
-------------- |------------:|----------------:|---------------------------:|------------:|
Fenster        |        6,58 |             1,9 |                         30 |      375,06 | 
Außenwand      |         1,5 |            0,24 |                         30 |        10,8 | 
Rolladenkasten |        1,57 |               1 |                         30 |        47,1 | 
Decke          |          23 |            0,14 |                         11 |       35,42 | 
Keller         |          23 |               1 |                         11 |         253 | 
Tür Flur       |        2,39 |               3 |                          4 |       28,68 |
**Summe**      |             |                 |                            |  **750,06** |

Die Transmissionswärmeverluste des Beispielraumes betragen in Summe also rund **750 W**.

### Lüftungswärmeverluste
Für ein gesundes Raumklima muss gelüftet werden, entweder händisch oder durch automatische Lüftungsanlagen. Durch den Austausch der aufgeheizten, nassen Luft gegen kalte Luft sinkt die Luftfeuchtigkeit. Es wird aber auch Wärmenergie abgegeben.

Zur Berechnung stellt sich zunächst die Frage, wie oft die Raumluft je Stunde ausgetauscht wird (die sogenannte Luftwechselrate): Eine Luftwechselrate von 1 h^-1 bedeutet, dass das gesamte Raumvolumen einmal je Stunde ausgetauscht wird. Im Falle eines 23m2 großen Raumes mit 2,55m Deckenhöhe also `23 * 2,55 * 1h^-1 = 58,65m3/h` Luft. Es gibt für unterschiedliche Räume unterschiedliche Angaben zu angestrebten Luftwechselramen - ein Badezimmer oder eine Küche wird man wegen der hohen Luftfeuchtigkeit sicher öfter/länger lüften als ein Wohnzimmer. Zumindest für Standard-Räume scheint aber häufig eine Luftwechselrate von 0,5 h^-1 angenommen zu werden: Im Schnitt wird dann das Raumvolumen an Luft alle zwei Stunden ausgetauscht. Für unsere Rechnung ergibt sich damit ein Volumenstrom von **29,325m3/h**: `23m2 * 2,55m * 0,5h^-1 = 29,325m3/h`.

Um nun die Lüftungswärmeverluste zu berechnen, multiplizieren wir den berechneten Volumenstrom mit der spezifischen Wärmekapazität von Luft (0,34 Wh/(m3*K)) und der Temperaturdifferenz:

`29,32m3/h * 30K * 0,34 Wh/(m3*K) = 299,12 W`

Oder alles in einer Formel:

`Raumfläche * Raumhöhe * Luftwechselrate * Temperaturdifferenz * Wärmespeicherkapazität_Luft`

Oder eben:

`23m2 * 2,55m * 0,5h^-1 * 30K * 0,34Wh/(m3*K) = 299,12 W`

Der Lüftungswärmeverlust unseres Beispielraumes beträgt damit **299,12 W**.


### Wiederaufheizleistung
Die Wiederaufheizleistung gibt an, wie viel Leistung benötigt wird, wenn er nach Unterbrechung des Heizbetriebes (bspw. wg. Nachtabsenkung oder Nichtbenutzung) wieder aufgeheizt werden soll. Mein persönlicher Eindruck ist, dass dieser Wert häufig eher unterschlagen wird, dazu finden sich vereinzelt auch Quellen:

> Meistens wird bei der Heizlastberechnung diese Aufheizleistung nicht berücksichtigt, weil dadurch die Leistung des Wärmererzeugers und die Raumheizlasten und entsprechend die Heizflächen für den "normalen Betrieb" zu groß ausgelegt werden und sich die Wärmeabgabe der Heizflächen evtl. als Fremdwärme bemerkbar machen. Da aber zunehmend die Heizung für das gesamte Gebäude oder einige Räume zeitweise abgesenkt oder sogar total abgeschaltet werden, wird die Zusatzaufheizleistung wieder aktuell.
> 
> <div style="font-size:small;text-align:right">Quelle: <a href="http://www.bosy-online.de/heizlastberechnung_nach_din_en_12831.htm">http://www.bosy-online.de/heizlastberechnung_nach_din_en_12831.htm</a></div>

Bei _Gebäude Energieberater_ findet sich gar die Information, dass der Wiederaufheizfaktor mit der Überarbeitung der Norm DIN EN 12831 "national auf 0 gesetzt" wurde - und damit die Wiederaufheizleistung pauschal immer mit 0W veranschlagt wird: 

> Meistens wird die Heizlast zu hoch ermittelt. Jetzt wurde die Notbremse gezogen. Wegen der teilweise harschen Kritik, vor allem am Wiederaufheizfaktor, der bei Wärmepumpen nicht sinnvoll anzuwenden ist und auch allgemein die Effizienz einer Anlage reduziert, wurde eine nationale Neuherausgabe des Beiblatts zur DIN EN 12831 eingeleitet. 
> 
>[...]
> 
> Der Wiederaufheizfaktor wird national auf 0 gesetzt und im Vorwort der Begriff „Norm-Heizlast“ definiert.
> 
> <div style="font-size:small;text-align:right">Quelle: <a href="https://www.geb-info.de/neue-normen/neue-normen-heizlastberechnung-wird-ueberarbeitet">https://www.geb-info.de/neue-normen/neue-normen-heizlastberechnung-wird-ueberarbeitet</a></div>

Für alle Interessierten, die den Wert dennoch berechnen möchten:

Die Aufheizleistung ist das Produkt aus der Grundfläche des Raumes in Quadratmetern und dem Wiederaufheizfaktor. Letzterer berücksichtigt Faktoren wie _Gebäudemasse_, _Luftwechselrate_, _gewünschter Wiederaufheizzeit_ und _Absenkzeit bzw. dem Temperaturabfall_ in der Absenkzeit. Der korrekte Wert lässt sich [entsprechenden Tabellen entnehmen](http://www.bosy-online.de/DIN%20EN%2012831/DIN_EN_128312008-07-Tabellen-12-13-14-15.pdf). 

Ein Beispiel: Während der Nachtabsenkung sinkt die Temperatur im 23m2 großen Raum um 3°C, Fenster und Türen sind über Nacht geschlossen. Die Gebäudemasse ist schwer/mittelschwer, innerhalb von einer Stunde soll der Raum wieder warm sein: Gemäß Tabelle beträgt der Wiederaufheizfaktor für diese Rahmenbedingungen `34 W/m2`. Damit wird die Aufheizleistung für unseren 23m2 großen Beispielraum mit `34 W/m2 * 24m2 = 787W`berechnet. 

Aus meiner Laiensicht kann die Wiederaufheizleistung zunächst auf 0W gesetzt werden; zum Abgleich mit den realen Verbräuchen kann die Wiederaufheizleistung dann feinjustiert werden, über die jeweiligen Wiederaufheizfaktoren lassen sich dann Szenarien abbilden wie "_Büro, von 17-8 Uhr unbeheizt_" (höherer Faktor) oder "_Wohnzimmer mit nur kleiner Außenwand und entsprechend geringem Temperaturabfall in der Nacht_" (kleinerer Faktor).

Für unseren 23m2 großen Beispielraum notieren wir für die Wiederaufheizleistung damit **0 Watt**.

### Die Summe der Wärmeverluste unseres Beispielraumes

Unser 23m2 großer Raum hat nun:

- Transmissionswärmeverluste von **750 W**
- Lüftungswärmeverluste **299,12 W**
- Wiederaufheizleistung von **0 W**

In Summe also eine Heizlast von **1049,12 W**. Die Heizlast wird in der Regel in Watt je m2 angegeben - also `1049,12W / 23m2 = 45,61 W/m2` für unseren Beispielraum.

