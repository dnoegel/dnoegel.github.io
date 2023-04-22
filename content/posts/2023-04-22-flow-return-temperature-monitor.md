

---
title: "Monitoring the flow and return temperature of my heating system"
date: 2023-04-22T18:48:00+02:00
author: 'Daniel'
lang: de
---
To better understand the efficiency and behaviour of my old gas heater, I created a simple monitor for flow and return temperatures.  
<!--more-->

## The idea
When planning to switch from old gas heaters to e.g. a heat pump or when just wanting to save gas it can be useful to understand, 
if (a) the gas heater is able to run on lower flow temperatures and (b) if it is dimensioned the correct way. With my [smarten gas meter (DE)](/posts/2022-10-12-gaszaehler)
and my [heat load calculation (DE)](/posts/2022-10-29-heizlastberechnung) I already have a good understanding - and still wanted to understand better, how my heating system is working.

## The circuit
For the circuit I am using one of my trusted ESP32 and two DS18b20 temperature sensors. After connecting voltage & ground the data pin (middle) is connected with Pin 3 and a 4.7kΩ resistor bridges data with V+. Multiple sensors can be connected this way, they all can be connected to e.g. PIN 3 on ESP32 and be told apart via `DallasTemperature::getTempCByIndex` later on. 

<div class="gallery-grid" style="">
<figure style="font-size:small;margin:auto">
  <img src="/images/flow-return-monitor/schematics.png">
  <figcaption>Schematic overview of the circuit</figcaption>
</figure>
</div>


| DS18b20     | Description                        |       ESP 32 Pin |
|:------------|------------------------------------|-----------------:|
| Voltage     |                                    |          Voltage |
| Ground      |                                    |           Ground |
| Data        |                                    |            Pin 3 |
| Data        | Bridge/pull up with 4.7kΩ resistor |          Voltage |

With this setup the DS18b20 sensors are attached to the pipes of my heating system, one to the flow and one to the return.

## The sketch
The sketch makes use of the libraries `OneWire` and `DallasTemperature` and does nothing more than reading those temperatures and sending them to my home automation system (IoBroker) using MQTT.

```c

#include <OneWire.h>
#include <DallasTemperature.h>

OneWire oneWire(ONE_WIRE_BUS);
DallasTemperature sensors(&oneWire);

char charBuf[40];


void loop()
{
  char tempStr[10];

  sensors.requestTemperatures();

  // Sensor 1
  float temp1 = sensors.getTempCByIndex(0);
  Serial.print("Temperature 1: "); Serial.print(temp1);  Serial.println(" °C");
  dtostrf(temp1, 2, 2, tempStr); 
  client.publish("mqtt.0.tempmon.temp1", tempStr);

  // Sensor 2
  float temp2 = sensors.getTempCByIndex(1);
  Serial.print("Temperature 2: "); Serial.print(temp2); Serial.println(" °C");  
  dtostrf(temp2, 2, 2, tempStr); 
  client.publish("mqtt.0.tempmon.temp2", tempStr);

  delay(5000);

}
```

For the full sketch including web updater, wifi setup etc. visit the [Temperature Monitor](https://github.com/dnoegel/temperature-monitor) repository.

## The result

<div class="gallery-grid" style="">
<figure style="font-size:small;margin:auto">
  <img src="/images/flow-return-monitor/flow-return-april.png">
  <figcaption>Flow and return temperatures of a day in april (14°C average temperature that day)</figcaption>
</figure>
</div>
Looking at a day in april, it can be well seen, that over night the burner did not run and the flow and return temperatures sunk quite substantially. After the initial heating of the house in the morning, there were not too many burner runs. 

<div class="gallery-grid" style="">
<figure style="font-size:small;margin:auto">
  <img src="/images/flow-return-monitor/flow-return-march.png">
  <figcaption>Flow and return temperatures of a day in march (4°C average temperature that day</figcaption>
</figure>
</div>

For a colder day in march, the picture is quite different: Not only did the burner run over night a few times - there a substantially more burner runs (i.e. strong reheatings of the flow temperature) throughout the day. From my understanding this is a result of the old gas burner being a bit to big for the house, especially due to window changes and insulation of walls and roof. 

Another interesting finding is, that the max flow temperature always seems to be roughly 50°C. From my understanding this temperature should be depending on the outdoor temperatures (e.g. 55°C for my standard design temperature of -9°C outside and e.g. 40°C for 5°C outside). This seems not to be the case - either due to a defect or the age of my heater. 