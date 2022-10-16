---
title: "Heizlastberechnung - Teil 2: U-Wert von Bauteilen ermittelnn"
date: 2022-10-12T19:55:27+02:00
author: 'Daniel'
language: de
draft: true
prev: "2022-10-09-heizlastberechnung.md"
next: "2022-10-13-heizung.md"
---

Geht es ums Haus stolpert man immer wieder über den U-Wert: Sätze wie _nach EnEV brauchste 'nen U-Wert von 0,24 aber für KfW müssen es schon 0,2 sein_ gehören zu den Standardsätzen des stolzen Altbaubesitzers. Hier betrachte ich kurz, was es mit dem U-Wert auf sich hat, wo man relevante Informationen für sein Haus findet - und wie man die Auswirkung einer Dämmung auf den U-Wert seines Hauses berechnen kann.

<!--more-->

## Mehr zu dieser Reihe
In diesem zweiten Teil meiner Reihe "_Heizlastberechnung_" geht es um einige Infos rund um den U-Wert. Wer sich generell für das Thema _Heizlastberechnung_ interessiert, kann einen Blick auf die anderen Teile werfen.
- [Heizlastberechnung - Teil 1: Heizlast eines Raumes und Hauses ermitteln](/posts/2022-10-09-heizlastberechnung)
- [**Heizlastberechnung - Teil 2: U-Wert ermitteln**](/posts/2022-10-12-uwert-ermitteln)
- [Heizlastberechnung - Teil 3: Eignen sich meine Heizkörper für eine Wärmepumpe?](/posts/2022-10-13-heizung)

## Was ist der U-Wert?
Der U-Wert wird auch *Wärmedurchgangskoeffizient* genannt. Er beschreibt, wieviel Wärme durch ein Bauteil dringen kann - also beispielsweise vom Wohnzimmer durch die Mauer nach draußen. Der U-Wert gibt an, wieviel Wärme in Watt pro Quadratmeter entweicht - und zwar bei einem Temperaturunterschied von einem Grad Kelvin. Entsprechend wird der U-Wert in W/(m²*K) angegeben - gesprochen "Watt pro Quadratmeter und pro Kelvin".

Je kleiner der U-Wert, desto weniger Wärme gelangt nach außen - desto geringer unsere Heizkosten. Ungedämmte Mauern und Decken oder alte Fenster können leicht einen U-Wert von 1 bis 3 W/(m²*K) haben. Dreifachverglaste Fenster gibt es heute mit einem U-Wert von bis zu 0,5 W/(m²*K), wer sein Dach dämmt und KfW-Förderung ins Auge fasst wird auch dem U-Wert 0,14 W/(m²*K) begegnen.

Dem U-Wert kommt eine kritische Bedeutung bei der Heizlast eines Gebäudes zu: Ein ungedämmtes Dach mit dem U-Wert 2 W/(m²*K) führt bei einem 100m² großen Haus und einer Norm-Auslegungstemperatur von -9°C zu **Transmissionswärmeverlusten von 6.000 Watt**. Bei 2100 Heizstunden lassen sich so überschlägig 12kW Heizenergie darauf zurückführen - je nach Kosten je kWh also 1200€ (bei 10ct je kWh) bis 2400 (20ct je kWh).   

## Welchen U-Wert habe ich überhaupt?
Hier wollen wir uns besonders mit dem U-Wert im Kontext der [Heizlastberechnung](/posts/2022-10-09-heizlastberechnung) beschäftigen. Insofern lautet die Frage ja erstmal: Welchen U-Wert muss ich für meine Außenwand angeben? Welchen für mein Dach? Aufschluss gibt oft erstmal die Bauakte. Hier ist festgehalten, mit welchen Materialien Wände und Decken errichtet wurden.

<figure>
  <img src="/images/heizlast/bauakte.png">
  <figcaption>Hier wurde mit Bimshohlblocksteinen gebaut</figcaption>
</figure>

### Beispielwandaufbau im Netz
Im einfachsten Fall findet ihr Informationen zu so einem Wandaufbau im Netz: So gibt es beispielsweise ein [Dokument des _energie institut hessen_ in dem "U-Werte typischer Außenwände"](https://cdn.website-editor.net/937ea8635222425aabf24e945f97dd23/files/uploaded/Artikel-Aussenwaende-1945-Sem_v1_V4WNWvfhScuPxU4FD8Aw.pdf) vorgestellt werden. Dort findet sich ein eigner Abschnitt "Außenwände aus Bimshohlblocksteinen 30cm". In einer detaillierten Tabelle wird genau ausgeführt, auf welchen U-Wert man üblicherweise kommt, werden zusätzlich Wärmeübergangswerte, Putzschichten etc bedacht. 

<figure>
  <img src="/images/heizlast/wandaufbau.png">
  <figcaption>Wandaufbau einer Außenwand gemäß energie institut hessen</figcaption>
</figure>

Im obigen Fall hat die Wand also einen U-Wert von 1,23W/(m²*K). 

### Exkurs: U-Werte umrechnen und Dämmung hinzuaddieren
Nehmen wir nun an, die Wand wurde im Nachgang noch mit 14cm Styropor gedämmt. Zwar findet sich auch hierzu eine Information beim _energie institut hessen_: Bei 12cm Dämmung käme man auf 0,24W/(m²*K). Wir wollen es aber genau für unsere 14cm Dämmung. Kurze Google-Suche: Styropor hat einen Lambda-Wert von 0,035 W/mK. Um daraus den U-Wert zu berechnen nehmen wir den Kehrwert (1/0,035) um den Wärmewiderstandswert zu erhalten. Diesen multiplizieren wir mit 0,14m - und bilden erneut den Kehrwert: 14cm Styropor haben demnach einen U-Wert von 0,25 W/(m²*K). Oder anders ausgedrückt: `1/((1/0,035)*0,14) = 0,25`

Wie berechnet man aber den U-Wert zweier verbundener Bauteile (Styropor + Wand)? Analog zu zuvor: Man addiert den Wärmewiderstandswert der beiden Bauteile - also wieder den Kehrwert der U-Werte. Also: Kehrwert beider U-Werte addieren - und davon dann wieder den Kehrwert bilden:
 `1/(1/1,23 + 1/0,25) = 0,207` - der U-Wert meiner Außenwand beträgt (aufgerundet) 0,21 W/(m²*K)".

### Kompletten Wandaufbau selber berechnen
Es gibt auch Tools im Netz die es erlauben, den kompletten Wandaufbau selber abzubilden. Mit beispielsweise [ubakus.de](https://ubakus.de) können bequem verschiedene Baustoffe wie Ziegel, Putz und Dämmung aus einem Katalog gewählt werden. 

<figure>
  <img src="/images/heizlast/ubakus.png">
  <figcaption>Wandaufbau mit <i>ubakus.de</i>, 30cm Hohlblockstein + 10cm Styropor</figcaption>
</figure>

Das Tool stellt den Wandaufbau schematisch dar, gibt den U-Wert aus und bietet darüber hinaus auch viele weitere Informationen - wie Amortisationsrechner für Dämmmaßnahmen, Hitzewiderstand etc pp.

### Energieberatung
Wer ein Energiegutachten zu seinem Haus hat, kann dort ebenfalls relevante Informationen finden. Nicht immer wurde im Rahmen des Gutachtens zwangsläufig auch eine Heizlastberechnung vorgenommen; insofern kann es schon vorkommen, dass man ein entsprechendes Gutachten sein Eigen nennt - und dennoch auf die Freude einer eigenen Heizlastberechnung nicht verzichten muss.

<figure>
  <img src="/images/heizlast/wandaufbau-gutachten.png">
  <figcaption>Wandaufbau in einem Gutachten</figcaption>
</figure>

Wie unschwer zu erkennen ist, wird aber auch in machen Gutachten der Wandaufbau eher pauschaliert betrachtet: wer sich vorstellt, dass durch Hightech-Verfahren eine realitätsgetreue Abbildung des Wandaufbaus erzeugt wird, auf der die Berechnung dann fußt: Weit gefehlt.  


### Weitere Tabellen bei bekannten Wandaufbauten
Um sich große Rechnerei und Gutachten zu ersparen kann man auf noch generalistischere Übersichtsseiten zurückgreifen. In [dieser älteren U-Wert Übersicht zu Fassadendämmplatten von Caparol](https://www.sonnen-herzog.com/wp-content/uploads/u-wert-tabelle-enev-2009-1.pdf) finden sich ebenfalls übliche Wandbaustoffe und deren U-Werte ohne Dämmung oder mit Dämmung zwischen 6cm und 40cm. Schauen wir dort nach 30cm Hohlblocksteinen finden wir für verschiedene Dämmstoffdicken folgende Angaben:

* **10cm** Styropor: **0,28 W/(m²*K)** (der Wert der auch für 10cm Styropor bei _ubakus.de_ berechnet wurde)
* **12cm** Styropor: **0,24 W/(m²*K)** (der Wert der für 12cm Styropor auch bei _energie institut hessen_ angegeben wurde)
* **14cm** Styropor: **0,21 W/(m²*K)** (der Wert, den wir weiter oben für 14cm Styropor auch berechnet haben)

Nach allen Überlegungen zur Ermittlung des U-Wertes von Außenwänden halten wir also fest: Grobe Übersichtstabellen erfüllen schon ihren Zweck, wenn man zumindest die Wandart und -stärke sowie ggf. die Dämmung kennt.  

### Pauschalwerte bei unbekannten Aufbauten
Auch für andere Bauteile gibt es im Netz in aller Regel grobe Werte an denen man sich orientieren kann, sollte man in Bauakte & Co nicht fündig werden. Gerade die Fenster im Haus können aus unterschiedlichen Baujahren in unterschiedlichen Qualitäten eingebaut worden sein. Grob lassen sich hier folgende Werte annehmen:

| Fenstertyp                 |  U-Wert in W/(m²*K) |
|----------------------------|--------------------:|
| Einfachverglasung          |                 5,6 |
| 2-Scheiben Isolierglas     |                 2,8 |
| 2-Scheiben Wärmeschutzglas |           1,0 - 1,2 |
| 3-Scheiben Wärmeschutzglas |           0,5 - 0,7 |

Beim _Institut Wohnen und Umwelt_ findet sich darüber hinaus eine [umfassende Liste von Pauschalwerten für Bauteile](https://www.iwu.de/fileadmin/publikationen/energie/enev/2004_denaEtIWU_LogaEtAl_Arbeitshilfe-f%C3%BCr-die-Ausstellung-von-Energiep%C3%A4ssen.pdf) wie  Außenwände (Tabelle 5), Geschossdecken (Tabelle 6), Dachschrägen (Tabelle 7), Fußböden (Tabelle 8) und Fenster (Tabelle 9). Hier kommt man oft schon zum Ziel, wenn man das Baualter/Baujahr kennt und grob weiß ob die Wand/Decke aus Holz oder Beton besteht.



## Fazit
Auch wer keine exakten Informationen zu den U-Werten seines Gebäudes zur Hand hat, kann im Netz zumindest hinreichend gute Pauschalwerte finden, die sich zumindest für einen ersten Wurf einer Heizlastberechnung gut eignen.
