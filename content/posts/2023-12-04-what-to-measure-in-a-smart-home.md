

---
title: "What to measure in a smart home"
date: 2023-12-04T22:02:00+02:00
author: 'Daniel'
lang: en
draft: false
---
Measuring data in your home can become somewhat addicting: How much energy did my solar plant generate? How much gas did my heating system use? How is the humidity in my rooms?

Here I want to give an overview of the things I am currently measuring in my home.
<!--more-->

## How do I measure
For most historicized data I use Grafana in combination with InfluxDB. Most data is dispatched through my Iobroker instance to InfluxDB - and collected via MQTT (ESP projects such as gas meter, flow and return temperature), Modbus (Sungrow inverter) or Zigbee (Aqara sensors). Some proprietary devices such as the smart plug with energy meter or the garage door opener are connected via dedicated extensions.   


## Solar
For owners of solar power plants some kind of data and analyzing is part of the fun: Most inverters offer some kind of app driven analytics. In my case, however, I wanted to have an overview that is more "real-time", so [I connected my inverter via Modbus with my Iobroker<sup style="font-size:8pt">(german)</sup>](/posts/2022-10-09-sungrow) instance. The visual representation still leaves room for improvement - but the data is almost real-time. 

<div class="gallery-grid" style="">
<figure style="font-size:small;margin:auto">
  <img src="/images/what-to-measure/solar.png">
  <figcaption>Real time overview of power generation and consumption.</figcaption>
</figure>
</div>

Another typical analysis is the following chart: It shows

* generated energy
* energy needed
* battery charge/discharge
* export/import from/to grid
* solar forecast

By using negative values for grid exports and battery charge the overall chart still is kind-of-readable and gives a good indication at which times a lot of energy was needed and when e.g. the battery was empty or energy was bought from the grid. And the solar forecast gives you a reference a model on top to understand if you had a good or bad solar day. 

<div class="gallery-grid" style="">
<figure style="font-size:small;margin:auto">
  <img src="/images/what-to-measure/daily_generation.png">
  <figcaption>Holistic overview of a solar day</figcaption>
</figure>
</div>

## Heating
Alongside electricity, gas for heating is an important cost item to keep an eye on. I have mainly three data points I control.  

### Gas consumption
I am [reading my gas consumption with a simple compass sensor<sup style="font-size:8pt">(german)</sup>](/posts/2022-10-12-gaszaehler). This way I am easily able to control my daily gas consumption as well as the monthly data. Over time, I added more and more details to the chart - currently it shows the following data:

* daily gas consumption (grey bars)
* 10-day-rolling-average gas consumption (pink)
* daily temperature average (red)
* number of heating cycles (black line)

<div class="gallery-grid" style="">
<figure style="font-size:small;margin:auto">
  <img src="/images/what-to-measure/gas.png">
  <figcaption>Gas consumption with related data such as heating cycles, temperature and rolling average</figcaption>
</figure>
</div>

### Flow and Return
Another information I was interested in, is the **flow and return temperatures** of my heating system: The whole heating system was originally layed out for flow temperatures of 75°C and above. For the heat pump, I eventually want to install, it is relevant to reduce the flow temperature as much as possible and check if you can still heat the house with it. 

Furthermore, the **number of heating cycles** (i.e. how often the gas heater needs to start) is interesting to get an impression how efficient the overall system runs: If the gas heater has a lot of heating cycles it most likely does not run very efficient and the heat pump can be layed out undersized in comparison to the gas heater. The heating cycles are basically measured by counting how many times the flow temperature sunk below a given value (e.g. 35°C) and then went up to over 45°C again.  

<div class="gallery-grid" style="">
<figure style="font-size:small;margin:auto">
  <img src="/images/what-to-measure/flow_return.png">
  <figcaption>Flow and return temperatures as well as cumulative daily gas consumption.</figcaption>
</figure>
</div>

In the above case I observed that the flow temperature usually does not exceed 50°C - which indicated that running a heat pump might very well be possible as generally it is said that heat pumps can be run quite efficiently if no more than 55°C flow temperature is needed. 

### Valves
Another analysis I like quite a lot is the valve position of my smart thermostats: They show a percentage value which indicates how far open the valve of my radiators were at any given time. This way I have a rough indication in which room the heating energy was needed and when. This again helps me to understand in which rooms the size of the radiators is over- or undersized or just right. A constantly 100% opened valve could be an indication that a radiator is undersized - and most likely needs to be replaced if the flow temperature is decreased further.  

<div class="gallery-grid" style="">
<figure style="font-size:small;margin:auto">
  <img src="/images/what-to-measure/valve_positions.png">
  <figcaption>Valve positions during a day: You can see how from 05:30am the house is heated (i.e. valves are opening) and how at 08:00pm the last valves close for night setback</figcaption>
</figure>
</div>

### Not fully automated (yet)…
…is my [regression model for gas consumption<sup style="font-size:8pt">(german)</sup>](/posts/2023-01-06-heizwerte/). It is some sort of reference model what gas consumption can be expected in a given month based on the average temperature. 

## Rooms
### Temperature & Humidity
A classic overview is the temperature and humidity chart. Especially in insulated older houses it is important to keep an eye on the humidity - and for ventilation it is relevant to compare outdoor and indoor temperatures and humidity.  

<div class="gallery-grid" style="">
<figure style="font-size:small;margin:auto">
  <img src="/images/what-to-measure/temp_humidi.png">
  <figcaption>Overview of temperature and humidity</figcaption>
</figure>
</div>

Obviously it would be too cumbersome to check and calculate the temperatures and humidity for every ventilation of the rooms - so there is the lamp below in place - that automatically turns <span style="color:red;font-weight:bold">red</span> if ventilation is not recommended and <span style="color:green;font-weight:bold">green</span> if ventilation is recommended.

<div class="gallery-grid" style="">
<figure style="font-size:small;margin:auto;width:300px">
  <img src="/images/what-to-measure/lamp.png">
  <figcaption>Lamp indicating if venting is recommended</figcaption>
</figure>
</div>

If ventilation is recommended or not is calculated with the following "rule-of-thumb" formula:

\\[
\\text{Humidity}\_{\\text{Outdoor}} - 3 \\times (\\text{Temp}\_{\\text{Indoor}} - \\text{Temp}\_{\\text{Outdoor}}) < \\text{Humidity}\_{\\text{Indoor}}
\\]


## Other
### Washing machine
A simple overview  of number of washes (total and per-week) as well as daily and monthly power consumption.
<div class="gallery-grid" style="">
<figure style="font-size:small;margin:auto">
  <img src="/images/what-to-measure/washing_mashine.png">
  <figcaption>Washing machine stats</figcaption>
</figure>
</div>

### Garage
Probably least relevant - but still I think it's interesting that we opened our garage 4831 times in shy over 3 years.  

<div class="gallery-grid" style="">
<figure style="font-size:small;margin:auto">
  <img src="/images/what-to-measure/garage.png">
  <figcaption>Schematic overview of the circuit</figcaption>
</figure>
</div>