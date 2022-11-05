---
title: "Replace Genius Pro Extruder"
date: 2022-11-05T11:12:03+01:00
author: 'Daniel'
lang: en
---
I broke the hotend of my Genius Pro - and decided to exchange the entire extruder with the "all metal" one. This is a short overview.


## How it started
Some month ago I learned about the new [arachne perimeter generator](https://blog.prusa3d.com/prusaslicer-2-5-is-here-new-perimeter-generator-step-file-support-lightning-infill-and-more_70562/). It is now possible to generate wider or thinner perimeter as it is needed. And I wondered: Will this allow me to print (almost) same quality prints with bigger nozzle sizes?
So I replaced my .4 nozzle with a .6 nozzle and went for it. 

## How it went
Even though I didn't notice much of a difference with the first few prints: Over time the prints became worse; I considered fine-tuning some print parameters as different nozzle sizes would probably also result in other extrusion rates, retract values etc. However, it does not seem to be too dramatic - so I went with it for some time.

Which was a mistake.

<div class="gallery-grid">

<figure style="font-size:small;">
  <img src="/images/extruder/print-quality.jpeg" style="">
  <figcaption>After degrading print quality for a longer time, this print motivated me to investigate</figcaption>
</figure>

<figure style="font-size:small;">
  <img src="/images/extruder/stringing.jpeg" style="">
  <figcaption>Especially the stringing was something I wanted to fix. I varied hotend temperature, retraction and flow rate.</figcaption>
</figure>
</div>

After a print with bad stringing and a lot of other issues motivated me to investigate that topic, I started printing some test prints with the goal to reduce stringing. Temperature, retraction, flow rate… non of these typical candidates yielded improvements. I decided to check the hotend - as a plugged nozzle and similar issues can also cause that kind of issues:

<div class="gallery-grid">
<figure style="font-size:small">
  <img src="/images/extruder/hotend.jpeg" style="">
  <figcaption>I finally decided to check the hotend and found a mess - which I didn't excactly improve when trying to fix it.</figcaption>
</figure>

<figure style="font-size:small;">
  <img src="/images/extruder/extruder-gear.jpeg" style="">
  <figcaption>But also the ball bearings of the extruder gear… looked a bit worn out.</figcaption>
</figure>
</div>

The picture above doesn't even tell the whole story: The hotend was completely coated with filament. And when I started removing parts of the filament, I even managed to rip of the control cable. And let's put it that way: A part of the nozzle also decided to stay with the hotend… What a success 🎉. 

## How it ended
I thought about replacing just the hotend honestly - you usually get it for around 20€. At the same time I was wondering if I couldn't easily swap the extruders and at the same time fix some other issues such as the cheap plastic extruder lever of the default extruder or the limited temperature capacity. So I went with the ["Full Metal Extruder" that is offered by Artillery](https://artillery3d.com/products/genius-whole-extruder-42)  - and which I found discounted.

The extruder can be replaced with just three screws - and so after a few days, the new extruder was mounted.

<div class="gallery-grid">

<figure style="font-size:small">
  <img src="/images/extruder/new-extruder.jpeg" style="">
  <figcaption>New all metal extruder mounted to the Genius Pro.</figcaption>
</figure>

<figure style="font-size:small;">
  <img src="/images/extruder/new-stringing.jpeg" style="">
  <figcaption>Even though not yet perfectly dialed in: The stringing improved a lot right away.</figcaption>
</figure>

<figure style="font-size:small;">
  <img src="/images/extruder/newprint.jpeg" style="">
  <figcaption>The first prints also looked promissing.</figcaption>
</figure>

</div>