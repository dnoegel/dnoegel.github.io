---
title: "Opening my front door with a fingerprint sensor"
date: 2022-12-07T21:11:03+01:00
author: 'Daniel'
lang: en
---

In this simple project I want to build a box that allows my children to enter the house with a fingerprint sensor. At the same time this project doubles as a prototype for video doorbell with fingerprint sensor that I might want to build later.

## Prerequisites
In my case we are owning a Nuki door lock already: So basically a small box on the inside of my front door that turns the door key when told so via Bluetooth. In my case I am also owning the so called "Nuki Bridge": That again is a small box in the hallway that can be told via API to tell the small box on the front door via Bluetooth to turn the key. What a wonderful new world!

The idea of this project is simple: I will connect a R503 sensor with an ESP32, that will then (if a valid fingerprint is detected) send that API request to the Nuki bridge.

Similar should be possible for any other smart door lock that has some kind of API. To my understanding it should even work without the Nuki bridge as the ESP32 could also directly communicate with the Nuki door lock via [Bluetooth API](https://developer.nuki.io/page/nuki-smart-lock-api-2/2). Examples for this are projects such as [NukiBleEsp32](https://github.com/I-Connect/NukiBleEsp32) or [Nuki_Hub](https://github.com/technyon/nuki_hub).

Long story short, you need:
- an API enabled smart door lock
- an ESP32 (or another microcontroller that has (depending on your door lock) Wifi or Bluetooth connectivity)
- a R503 fingerprint sensor

## The project build
Everyone in the software industry knows: Nothing holds longer than a prototype. So I decided to invest some extra effort to make the box for the finger print sensor "somewhat nice". It ended up being more on the beefy sides of things… but it serves its purpose.

<div class="gallery-grid">
<figure style="font-size:small;">
  <img src="/images/fingerprint/gluing.jpeg" style="">
  <figcaption>Gluing the box</figcaption>
</figure>

<figure style="font-size:small;">
  <img src="/images/fingerprint/glued.jpeg" style="">
  <figcaption>Bringing the pieces together</figcaption>
</figure>


<figure style="font-size:small;">
  <img src="/images/fingerprint/cover.jpeg" style="">
  <figcaption>As the wood was a bit thick, I had to drill a countersink hole.</figcaption>
</figure>


<figure style="font-size:small;">
  <img src="/images/fingerprint/board.jpeg" style="">
  <figcaption>The wiring is quite easy, as the r503 sensor uses UART. Wires 5 & 6 (blue/white) are not needed in my case.</figcaption>
</figure>
</div>

## A word of warning
{{< admonition type=warning title="Warning" open=true >}}
Even though capacitive fingerprint sensors and especially R503 seem to be reasonable well proven: Putting it out there and running home-brewed software to decide if someone should enter your house or not is per se… ~~dump~~ and idea worth careful consideration.  
{{< /admonition >}}

Some possible pitfalls are:
- If the box is accessible an attacker could swap the sensor with an own, preconfigured one (as the fingerprints are stored on the R503). Therefore your should make use of the password functionality of the sensor.
- Make use of the `confidence` value of the sensor: Only open the door if the sensor is confidence enough about the match.
- The so called FAR (False Acceptance Rate) indicated how likely it is, that the sensor will mistake two different fingers for the same. For R503 I found values such as `<0.001%` - which is one out of 100.000.
- There could be other attacks to the sensor that I am not aware of.
- There could be stupid programming mistakes that I am not aware of.

It could therefore also make sense to think twice about:
- if to use this system at all
- where to place it and how visible it should be
- deactivating it for longer journeys
- making it only usable in certain time windows
- ensuring that the ESP is not accessible


## The project
The actual wiring is quite easy: The R503 just needs 3.3V from the ESP32 and ground for power supply. Additionally I connected the yellow TXD Pin of the R503 to P17 Pin of the ESP32 and the brown (sometimes also green) RXD Pin of the R503 to the P16 Pin of the ESP32.

Using the library manager of the Arduino IDE it is quite simple to use the [Adafruit Fingerprint Sensor Library](https://github.com/adafruit/Adafruit-Fingerprint-Sensor-Library/). It also includes a lot of examples for registering new fingerprints, checking fingerprints etc.

The main logic of this project so mainly is arranging this examples in a way that you can trigger a "new finger registration" by putting your "admin" finger on the sensor for some longer time. The sensor then blinks and will tell you to show the new finger two times. After that the sensor stores the finger hash internally and will be able to identify the finger in the future. 

<figure style="font-size:small;width:640px;margin:auto">
<video width="300" controls>
  <source src="/images/fingerprint/door2.mov" type="video/mp4">
</video>  <figcaption>Undoubtedly the bue masking tape adds a lot of charm to the box.</figcaption>
</figure>


The rest then again is simple: If the sensor sees a know finger, it will trigger an API Call that - you might guess it - will tell my Nuki bridge via HTTP to tell the Nuki door lock via Bluetooth to turn the key a few times. The door jumps open - and in walk 2 joyfully squealing kids with muddy boots and run with them through the house. With regards to my idea to let the kids in unguarded I just can say: Be careful what you wish for.

As always the code can be found on [Github](https://github.com/dnoegel/fingerprint-doorlock).
