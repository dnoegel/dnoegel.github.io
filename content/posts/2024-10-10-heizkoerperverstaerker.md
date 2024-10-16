

---
title: "Heizkörperverstärker"
date: 2024-10-10T19:02:00+02:00
author: 'Daniel'
lang: de
draft: false
keywords: 'W1209, Thermostat, Temperatursensor ,Heizkörperverstärker, radiator booster, radiator amplifier'
---
Was sind Heizkörperverstärker, was haben sie mit Wärmepumpen zu tun - und wie kann man sie kostengünstig selber bauen? Mit diesen Fragen habe ich mich beschäftigt.   
<!--more-->

## Heizkörperverstärker - Was ist das?
<div class="gallery-grid" style="width:400px;float:right">
<figure style="font-size:small;margin:auto">
  <img src="/images/radiator_booster/fan_under_radiator.jpeg">
  <figcaption>Selbstgebauter Heizkörperverstärker</figcaption>
</figure>
</div>
Bei "Heizkörperverstärkern" handelt sich in der Regel um Lüfter, die die natürliche Konvektion von Heizkörpern (Radiatoren) verstärken. Klassische Heizkörper verteilen die Wärme im Raum, indem sie sich erwärmen und so einen warmen Luftstrom erzeugen, der den Heizkörper von unten nach oben durchzieht und sich so im Raum verteilt.

Heizkörperverstärker verfügen über einen Temperaturfühler, der erkennt, wenn sich der Heizkörper erwärmt - so dass dann automatisch die Lüfter dazugeschaltet werden können. Die Lüfter drücken (oder ziehen) zusätzlich Luft durch den Heizkörper und verstärken so die natürliche Konvektion. Sobald der Heizlkörper abkühlt (weil die Zieltemperatur im Raum erreicht wurde), schalten sich auch die Lüfter wieder ab - so dass diese jeweils nur für kurze Zeit laufen.

## Und warum Wärmepumpe?
Wärmepumpen arbeiten besonders effizient, wenn sie auf niedrigen Vorlauftemperaturen betrieben werden - bspw. 55°C und weniger. Gleichzeitig bedeutet eine niedrigere Vorlauftemperatur auch, dass Heizkörper weniger Wärmeleistung an den Raum abgeben können - unter anderem, weil die natürliche Konvektion schwächer ausfällt je geringer die Temperatur des Heizkörpers ist.

Um diesen Effekt auszugleichen, gibt es für Wärmepumpen verschiedene Möglichkeiten:

- Nutzung von **Flächenheizungen** wie Fußbodenheizungen - diese funktioneren auch bei geringen Vorlauftemperaturen besonders gut
- Größere **Dimensionierung der Heizkörper** (Austausch), um die geringere Wärmeleistung auszugleichen
- Nutzung von **Niedertemperaturheizkörpern**  (Austausch) - diese verstärken die natürliche Konvektion durch Lüfter

Da alle drei Optionen - gerade in der Altbausanierung - relativ kostspielig sind, gibt es eine vierte Option: 

- Nachrüsten der Heizkörper mit sog. Heizkörperverstärkern

Hierfür gibt es fertige Geräte wie etwas "Heatfan 5" oder "SpeedComfort" für etwa 130€ für ein Heizkörper-Set.


## Selber bauen
<div class="gallery-grid" style="width:250px;float:right">
<figure style="font-size:small;margin:auto">
  <img src="/images/radiator_booster/fan_magnet.png">
  <figcaption>Lüfter mit angeklebten Magneten</figcaption>
</figure>
</div>

Ein Heizkörperverstärker lässt sich grundsätzlich für etwa 30€ relativ einfach selber bauen. Dafür wird benötigt:

* Ein W1209 Thermostat-Modul, bspw. von [AliExpress (1,29€)](https://de.aliexpress.com/item/1005002587026097.html?spm=a2g0o.productlist.main.1.7ecc426ciatmSc&algo_pvid=f57046fc-9399-4ce5-bdc1-86876911b674&algo_exp_id=f57046fc-9399-4ce5-bdc1-86876911b674-0&pdp_npi=4%40dis%21EUR%211.37%211.29%21%21%211.47%211.38%21%402103896117284938671557482ed1dd%2112000021277466005%21sea%21DE%212926692221%21X&curPageLogUid=vw8zIEQqJkx4&utparam-url=scene%3Asearch%7Cquery_from%3A) oder [Reichelt (3,75€)](https://www.reichelt.de/entwicklerboards-thermostat-12-v-digital-w1209-debo-xh-w1209-t-p223621.html?PROVID=2788&gad_source=1&gclid=Cj0KCQjw05i4BhDiARIsAB_2wfCdnt-aG0Y68WzJx-6G8yBwxYLK4JpeBJnQecC4IEbRr4DXKhC2VecaAnvEEALw_wcB) 
* 3x 120mm Lüfter, bspw. von [Xilence (je 2,72€)](https://www.amazon.de/Xilence-XPF120-R-120mm-Geh%C3%A4usel%C3%BCfter-schwarz/dp/B01L6QQJ88/ref=pe_27091401_487027711_ci_mcx_mi_sccl_2/262-2744469-0699107/ref=ci_mcx_mi?_encoding=UTF8&pd_rd_i=B01L6QQJ88&psc=1&pd_rd_w=yieQi&content-id=amzn1.sym.2135b44a-d37f-481e-b630-d91bec031b4b%3Aamzn1.symc.97169720-7bb7-4a7f-8085-5fa344775fa5&pf_rd_p=2135b44a-d37f-481e-b630-d91bec031b4b&pf_rd_r=QVBX1EVXKKJPHPJHYRNM&pd_rd_wg=ndzDm&pd_rd_r=6618ee50-0b4b-4474-88cc-b80f1fc6ce46)
* [Regelbares Netzteil (15,89€)](https://www.amazon.de/dp/B0CKHWLBP8/ref=pe_27091401_487027711_TE_SCE_dp_3)

Hinzu kommen ggf. Magnete und Kabel.

Die Lüfter werden nun in Serie geschaltet und mit dem W1209 Thermostat verbunden. Die Magnete werden oben auf die Lüfter geklebt und damit unterhalb der Heizung befestigt.

Das W1209 Thermostat übernimmt nun die Lüftersteuerung und schaltet die Lüfter ein, wenn der Heizkörper sich erwärmt - und wieder ab, wenn der Heizkörper auskühlt. So laufen die Lüfter nur, wenn es auch wirklich nötig ist.  

## Weitere Optimierungsmöglichkeiten
Der oben geschilderte Aufbau ist relativ simpel. Insbesondere führt die direkte Befestigung der Lüfter am Heizkörper dazu, dass die Vibrationen der Lüfter über den Heizkörper recht deutlich zu vernehmen sind. Dies kann bspw. mit Gummipuffern gemildert werden - durch die Magnet-Montage sind die Möglichkeiten bei dieser Bauweise allerdings eingeschränkt. 

Eine andere Möglichkeit, die Lautstärke zu mindern, besteht darin, die Lüfter langsamer laufen zu lassen. Dazu kann mit dem regelbaren Netzteil die Spannung bspw. auf 9 Volt reduziert werden. 

## Effekt und Messungen
<div class="gallery-grid" style="width:400px;float:right">
<figure style="font-size:small;margin:auto">
  <img src="/images/radiator_booster/without.png">
  <figcaption>Heizen in meinem Büro ohne Heizkörperverstärker (bis 16 Uhr)</figcaption>
</figure>
</div>

Nutzen und Effekt des Heizkörperlüfters lassen sich leider nur aufwändig messen: Klar, die Luft die den Heizkörper durchströmt, merkt man schon - nur wird der Raum wirklich schneller warm? Funktionieren niedrigere Vorlauftemperaturen nun besser? Kann ich das Thermostat etwas runterregulieren und erhalte die gleiche End-Temperatur?

Auch vergleichbare Testkonditionen sind nicht ganz einfach: Sind es draußen 5°C oder 10°C? War der Raum vorher schonmal aufgeheizt (und strahlen bspw. Gegenstände noch etwas Wärme ab) oder wird er erstmalig aufgeheizt. Befinden sich Personen im Raum? Laufen Küchengeräte oder Computer?
<div class="gallery-grid" style="width:400px;float:right">
<figure style="font-size:small;margin:auto">
  <img src="/images/radiator_booster/with.png">
  <figcaption>Heizen in meinem Büro mit Heizkörperverstärker (bis 14 Uhr)</figcaption>
</figure>
</div>

Es ist merkbar, dass sich die Wärme durch den Einsatz des Heizkörperverstärkers schneller im Raum verteilt. In meinem Arbeitszimmer beispielsweise habe ich subjektiv eine angenehmere Wärme als in anderen Räumen, obwohl dort dieselbe Zieltemperatur eingestellt ist. Das liegt vermutlich daran, dass die verbesserte Luftzirkulation die Wärme gleichmäßiger im Raum verteilt.

Die Diagramme rechts veranschaulichen den Temperaturverlauf (rote Linie) und die Ventilposition (blaue Linie) der Heizung. Über das digitale Thermostat konnte ich die Ventilposition auslesen: Bei 100 % ist das Ventil vollständig geöffnet, sodass maximal warmes Wasser durch den Heizkörper fließt, während 0 % bedeutet, dass das Ventil geschlossen ist. Die Ergebnisse zeigen, dass bei Verwendung des Heizkörperverstärkers das Ventil insgesamt kürzer komplett geöffnet ist und das Thermostat schneller die Heizleistung herunterregelt. Ohne den Verstärker hingegen bleibt das Ventil eine Zeit lang komplett geöffnet, bevor es schließlich vollständig schließt. Beim Einsatz des Verstärkers hingegen regelt das Ventil auf etwa 30-40 % herunter, anstatt vollständig zu schließen.

Interessant wäre, wie viel Energie tatsächlich gespart wird, um den Raum mit und ohne Heizkörperverstärker aufzuheizen. Diese Daten ließen sich jedoch nur über den gesamten Gasverbrauch ermitteln, was aufgrund der Beheizung des gesamten Hauses nicht leicht isolierbar ist. Besonders aufschlussreich wäre es zudem, die Effekte des Verstärkers bei niedrigeren Vorlauftemperaturen zu untersuchen – etwa, wenn die Heizkörper für die Raumgröße unterdimensioniert sind. Da meine aktuelle Gasheizung allerdings eine Mindestvorlauftemperatur von 55 °C hat, konnte ich diese Szenarien bisher nicht analysieren. Trotzdem deuten die Tests darauf hin, dass auch bei geringeren Ventilöffnungen eine ausreichende Erwärmung des Raumes möglich ist.


