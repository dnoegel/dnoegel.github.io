

---
title: "How to make an AC smart"
date: 2023-11-22T20:02:00+02:00
author: 'Daniel'
lang: en
---
I have a room that - in winter - needs some heating. Instead of connecting it to my central heating system,
I decided to use my mobile AC which can also heat.  
<!--more-->

## The idea
The AC can be remote-controlled with an IR remote - which unfortunately cannot send through walls. So it is not possible to comfortably start the AC automatically or activate/deactivate it while sitting at the desk. 

So the idea is (a) to read the IR signals for on/off from the original remote - and then (b) send those signals with my ESP 32 microcontroller to the AC. The ESP 32 is then connected to my smart home system, so that it can be automated or controlled via UI.

Additionally, (c) a photo transistor is used to detect if the AC is on or not. The transistor is taped to one of the LEDs on the AC - so whenever the photo transistor detects some light I can assume that the AC is active. This is especially useful as the ON and the OFF signal of the remote are the same - so would be hard to determine automatically if the AC is currently running or not. 

## The circuit(s)
Actually there are two circuits: One for reading the original IR signal initially and another permanent one to "make the AC smart". The circuit described below focuses on the latter. For reading the IR signal you can easily check out the github repository linked below - or just open `IRemote->SimpleReceiver` from the example section in your arduino IDE.   

For the permanent circuit I am using another one of my trusted ESP 32, a BPW 40 NPN photo transistor and a KY-005 IR emitter. Furthermore, a
100kΩ resistor is used to pull up the data pin which is connected to the BPW 40 emitter. 

<div class="gallery-grid">

<div class="gallery-grid" style="">
<figure style="font-size:small;margin:auto">
  <img src="/images/climatecontrol/circuit.png" style="height:250px">
  <figcaption>Circuit overview</figcaption>
</figure>
</div>

<div class="gallery-grid" style="">
<figure style="font-size:small;margin:auto">
  <img src="/images/climatecontrol/circuit2.png" style="height:250px">
  <figcaption>Schematic overview of the circuit</figcaption>
</figure>
</div>

</div>

| From device | From pin  | To device | To pin | Description                                                                                            |
|:------------|:----------|----------:|-------:|--------------------------------------------------------------------------------------------------------|
| BPW 40      | Emitter   |     ESP32 |     34 |                                                                                                        |
| BPW 40      | Collector |     ESP32 | Ground |                                                                                                        |
| ESP32       | 34        |     ESP32 |  3.3 V | Pull up pin 34 with 100kΩ resistor. The higher the resistor, the more sensitive the phototransistor is |
| KY-005      | Minus     |     ESP32 | Ground |                                                                                                        |
| KY-005      | Vdd       |     ESP32 |    5 V |                                                                                                        |
| KY-005      | Data      |     ESP32 |     32 |                                                                                                        |

## The sketch
The sketch uses the `IRemote` library to read/send IR signals and `PubSubClient` to receive and publish messages via MQTT.

```c
#include <IRremote.hpp>
#include <PubSubClient.h>
#include <WiFi.h>

WiFiClient espClient;
PubSubClient client(espClient);

char charBuf[40];
bool acActive = false;

void loop()
{
  handleRead();
  checkACActive();
  
  delay(200);
}

void checkACActive()
{
  int photodiodeValue = analogRead(PHOTO_DIODE_PIN);
  bool newActive = photodiodeValue < PHOTO_DIODE_THRESHOLD;

  if (newActive != acActive) {
    acActive = newActive;
    String boolAsString = acActive ? "true" : "false";
    client.publish("mqtt.0.climateremote.actual_state", boolAsString.c_str());
  }
}

void sendOff()
{
  checkACActive();
  // if AC is already off: do nothing
  if (acActive == false) {
    return;
  }

  client.publish("mqtt.0.climateremote.message", "OFF");
  IrSender.sendNEC(0x80, 0x9C, 3);
}

void sendOn()
{
  checkACActive();
  // if AC is already on: do nothing
  if (acActive == true) {
    return;
  }

  client.publish("mqtt.0.climateremote.message", "ON");
  IrSender.sendNEC(0x80, 0x9C, 3);
}


void handleRead()
{
  if (IrReceiver.decode()) {
    Serial.println(IrReceiver.decodedIRData.decodedRawData, HEX); // Print "old" raw data

    IrReceiver.printIRResultShort(&Serial); // Print complete received data in one line
    IrReceiver.printIRSendUsage(&Serial);   // Print the statement required to send this data

    IrReceiver.resume();
  }

}
```
The functions `sendOff` and `sendOn` are called by the MQTT callback function whenever a certain message arrives for a certain topic. For the full sketch including web updater, wifi setup etc. visit the [ClimateRemote](https://github.com/dnoegel/ClimateRemote) repository.

## The result

<div class="gallery-grid" style="">
<figure style="font-size:small;margin:auto">
  <img src="/images/climatecontrol/vis.png">
  <figcaption>UI to control the AC via iobroker visualization</figcaption>
</figure>
</div>
