
---
title: "TRuDI: Warum wir Stromzähler mit blinkenden LEDs auslesen"
date: 2026-04-12T11:00:00+02:00
author: "Daniel"
language: de
draft: false
keywords: "Smart Meter, SMGW, TRuDI, HAN, Homeassistant, D0 Schnittstelle, ppc auslesen"
---

2017\. Unter dem klangvollen Namen BundesDisplay machen sich die "Physikalisch-Technische Bundesanstalt" (PTB, das nationale Metrologieinstitut) und der Verband der "Elektro- und Digitalindustrie", dem "Letzverbraucher" (das sind wir) mehr Transparenz und Komfort über ihre Stromzähler zu geben. 

TRuDI (Transparenz- und Display-Software) wird geboren. 

Von nun an kann der Benutzer bequem zu Hause seine Zählerstände nachvollziehen: Ein Interface, Alle Hersteller, ein Client. Es hätte so schön sein können.

### Wie meine Reise begann

Seit etwa zwei Jahren habe ich ein Smart Meter zu Hause.  Eine moderne Messeinrichtung mit Smart-Meter-Gateway (wichtig!). Ein intelligentes Messsystem (IMSys). Schon bei den Namen herrscht Verwirrung. Und da die kleine Box an meinem Stromzähler einen LAN Anschluss hat, fragte ich mich natürlich, wie ich das Ding auslesen kann.

Gar nicht mal so einfach: Nachdem ich herausgefunden habe, dass die Box fix auf IP 192.168.1.200 lauscht (was jeden Laien schon vor Herausforderung stellt, wenn das Heimnetz nicht zufällig in diesem Bereich unterwegs ist), das nächste Hindernis: Zugang nur durch den Netzbetreiber. Mail, Formular und einige Tage später dann: Der Zugang ist da.

Was mich erwartete, war eine relativ langsame Seite, die sich etwas wie 2003 anfühlt.

### Das muss sich ja automatisieren lassen

Da ich meine Energiedaten gerne Langzeit-auswerten möchte, stand die Frage im Raum: Wie lese ich das automatisch aus? Wie bekomme ich das in Homeassistant & Co?

Eine offizielle Schnittstelle ist nicht auszumachen. Und so baue ich mir einen kleinen Scraper, der die Seite ausliest. Hidden Fields, Session Tokens und neuerdings auch Session Cookies und elendig lange Antwortzeiten. Das muss doch besser gehen?

Und so stoße ich auf [TRuDI](https://bitbucket.org/dzgtrudi/trudi-public.git): Die offizielle Software gibt uns "Letztverbrauchern" endlich den Zugriff, den wir brauchen.

> Mit TRuDI (Transparenz- und Display-Software) stellt die [Initiative] Bundesdisplay eine herstellerübergreifende, standardisierte Visualisierungslösung bereit […]. TRuDI bietet dabei eine Display Funktion, mit der Messwerte, die im SMGW vorhanden sind für den Letztverbraucher angezeigt werden. Darüber hinaus steht eine sogenannte Transparenz Funktion zur Verfügung. Im Rahmen dieses funktionalen Merkmals ist der Letztverbraucher mit Hilfe der Software in der Lage, Tarifrechnungen, die auf Basis der Messwerte des SMGWs in der Systemlandschaft des Lieferanten durchgeführt hat lokal nachzuvollziehen und damit seine Rechnung zu überprüfen.

Und ich sehe natürlich TRuDI als meine Chance, die standardisierte API zu entdecken, die von dieser offiziellen Software benutzt wird.

### Ein Irrtum

Weit gefehlt: In Deutschland gibt es fünf zertifizierte [SMGW-Hersteller](https://www.bsi.bund.de/DE/Themen/Unternehmen-und-Organisationen/Standards-und-Zertifizierung/Smart-metering/Smart-Meter-Gateway/Zertifikate24Msbg/produkte.html). Jedes dieser SMGWs bietet eine andere Schnittstelle. Nicht ein bisschen anders. Komplett anders. Woher ich das weiß? Ich habe mir TRuDI im Detail angesehen. Die Spezifikation, der Standard ist eine .NET Deskop-App. Fünf C#-Projekte, fünf Adapter, fünf verschiedene Protokolle.[^1] Das ist der Standard. Kein PDF, keine API. Eine Electron-App.

[^1]: Genau genommen sind es sechs Adapter in TRuDI — ein Hersteller scheint nicht offiziell BSI-zertifiziert zu sein. Aber das ist ein anderes Thema, das hier zu weit führt.

Die Formate? It depends:

**Hersteller 1**: HTML Tabellen Scrapen (ab Version 4 gibt's auch XML im COSEM Envelope). 2004 lässt grüßen.

**Hersteller 2**: GZip komprimiertes XML mit eigenem Namespace.

**Hersteller 3**: TRuDI XML - hier scheint(!) die Spezifikation `VDE AR E 2418-6` umgesetzt worden zu sein (auf die ich keinen Zugriff habe). 

**Hersteller 4**: JSON; Werte als Strings "125.34 kWh". Split am Leerzeichen und hoffen, dass es passt

**Kersteller 5**: Json und XML (scheinbar für Verträge und Messwerte unterschiedlich)

Und so findet sich im [Lastenheft](https://www.ptb.de/cms/fileadmin/internet/fachabteilungen/abteilung_2/2.3_elektrische_energiemesstechnik/2.34/download_234/20180119_Lastenheft_TRuDI_final_clean.pdf) von TRuDI auch der Hinweis

> Herstellerspezifischer Software Adapter: Softwarekomponenten, die das herstellerspezifische HAN Protokoll auf VDE AR E 2418-6 (Greenbutton/OFFIS) wandelt. Der Adapter wird von jedem Hersteller in einer einheitlich vorgegebenen Form zugeliefert und vom Hersteller der TRuDI Software in die Software-Lösung der Initiative Bundesdisplay integriert. Die Software-Lösung erkennt automatisch, welcher Adapter bei der Anbindung eines SMGWs eines spezifischen Hersteller zu verwenden ist. Details zum herstellerspezifischen Software-Adapter sind in Kapitel 3.3 enthalten

Nicht falsch verstehen: Einheitlich ist nur die [Schnittstelle](https://bitbucket.org/dzgtrudi/trudi-public/src/9a6571325e37789c9080b8177d76a3c2ea2be473/doc/han-adapter.md) zwischen TRuDI und den ([scheinbar proprietären](https://bitbucket.org/dzgtrudi/trudi-public/src/9a6571325e37789c9080b8177d76a3c2ea2be473/private-packages/)(?)) Adaptern der Hersteller. Der Nutzen für die automatische Auslesung via Homeassistant & Co ist begrenzt. 

Stöbert man weiter, stößt man auf allerlei Kuriositäten: Manche HAN Gateways haben Ruhezeiten in jedem 15-Minuten-Slot oder Tagesbeginn abhängig vom Zählertyp. Und Zeit wird mal in Epoch-Sekunden, mal in Epoch-Millisekunden, mal in ISO 8601 gemessen. Manche Hersteller implementieren keine Statuscodes und antworten mit deutschen Fehlermeldungen in HTML. Andere unterstützen keine Paginierung.

Überall dies legt TRuDI den wohlig warmen Mantel der Schein-Standardisierung. Hinten raus Web Scraping mit Session-Cookies und HTML-Fehler-Matching. Vorne raus ein wohlformatiertes XML. VDE AR E 2418-6.

### Keine API

In einem Wort: Bei den Smart Meter Gateways herrscht völliger Wildwuchs. TRuDI ist ein Versuch, im Nachgang diesen Wildwuchs wegzuabstrahieren und via Desktop-App zugänglich zu machen. Mit einer Schnittstelle hat das nichts zu tun.

Wer also sein Smart Meter Gateway automatisch auslesen möchte, muss es oftmals aus den geradezu kruden Weboberflächen reverse-engineern. Und wer gar plant, eine Software-Lösung zu bauen, die mehrere Hersteller unterstützt: Kaum möglich.

### Warum einfach einfach besser ist

Und genau hier kommen die D0 Schnittstellen ins Spiel. Es ist doch merkwürdig, dass jeder Nutzer eines SMGW zu Hause ein intelligentes System mit (oftmals) LTE oder gar LAN Interface hat - und dann mit einer Taschenlampe vor dem Gerät steht, damit eine kleine Leuchtdiode durch Blinken bekannt gibt, wieviel Strom verbraucht wurde. 

Das hat für einigen Spot gesorgt - inklusive "[Realer Irrsinn](https://www.youtube.com/watch?v=aqHauk3bNFA)" bei Extra 3. Doch nach meinem Ausflug in die Welt der SMGW HAN Schnittstellen weiß ich, dass diese D0-Schnittstelle etwas ist, das auch in 100 Jahren noch lesbar ist. Während bei TRuDI und seinen hinter "private packages" versteckten HAN Integrationen schon vielleicht in 3 Jahren keiner mehr weiß, wie man es auf einem modernen PC in der neusten .NET Version zum Laufen bringt.