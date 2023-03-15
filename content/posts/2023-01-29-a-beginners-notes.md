---
title: "A beginner's notes to 3d printing"
date: 2023-01-29T18:48:00+02:00
author: 'Daniel'
lang: en
draft: true
---
A year ago I bought my first 3D printer: An Artillery Genius Pro. Here you'll find some notes and thoughts about it. 

## The printer
3D printing is one of those hobbies that start before the actual hobby: I started looking around for a suitable printer almost a year before I did the purchase - read a lot of blog posts, reviews - and consumed way too much Youtube videos.
I wanted to start with a simple beginner's model - that isn't too much fiddeling but still has a reasonable price point. I especially had a look at various Creality Ender printers and Artillery Sidewinders… but also checked out various delta printers and of course Prusa printers. 

At the end of the day I found that especially the Creality Ender model 3 seem to be more on the "fiddling" end of things - even though commonly used and recommended. But if you watched 20 videos of people explaining when which part of their Ender 3 broke, was replaced or showed serious wear… you have the feeling that it might not be the right choice for beginners. Even though I have to admit: The ender 3 is so popular - of course you will find most reports about problems for that printer. And you will also find most fixes and tips for that printer.

The Prusa was honestly a bit expensive for me - and at the same time I didn't want to spend 750€ on a printer that I still had to assemble - with the constant fear that some problem was related too my assembly. 

I finally went with the Artillery Genius Pro - which was something like the smaller brother of the Sidewinder X2 for a reasonable price and with all the bells and whistles such as 230V bed (heated quickly), a filliament runout sensor and auto bed levelling.  


## Setting it up
After assembling the printer one could have the expectation to directly start printing. That is not the case. Usually there are some steps involved earlier:

* Levelling the bed manually
* Running the automatic bed levelling
* Adjusting the z-offset
* Find the correct bed and nozzle temperature to print with

This will usually allow you to print some of the demo prints that are shipped with most 3D printers on an SD card or USB stick. If you want to print something more useful right away, you will also need to find a slicer in order to prepare your 3D model for the print (i.e. slicing it). And if you want the prints to be accurate and within tolerance you might also need to calibrate _e-steps_, _flow_, _elephant footprint compensation_ or _horizontal expansion_ (depending on slicer and issue of your prints). 

Actually there are a lot of guides online that will help you to understand, what the strange outcome of your print is called (e.g. "z wobble") and how to fix it. 

- https://www.matterhackers.com/articles/3d-printer-troubleshooting-guide
- https://www.3dsourced.com/rigid-ink/ultimate-3d-printing-troubleshooting-guide/
- https://www.3dplatform.com/Blog/2020/July/Solutions-to-Common-Printing-Problems
- https://bitfab.io/blog/3d-printing-problems/

Please understand the following points as notes to future Daniel - if you spent some evenings to dial those values in you'll understand ;-).  

### Bed levelling
Bed levelling is the process of making sure that the bed is adjusted in a way that the nozzle of the 3D printer has the same distance to the bed without changing its position on the Z-axis. A bad levelling will result in a print being to narrow to the bed at some places - and being too far away in other places. And this again will usually result in bad first layers of the print and hence bad adhesion, ugly surfaces or failed prints. 

For common cartesian printers there are 4 screws below the bed that will allow you to make sure, that the bed is level (by adjusting any of the 4 corners). Most probably you already have heard of the "paper method" where you adjust the bed in a way, that in each corner the nozzle is barely scratching a typical piece of paper. This way you make sure, that in each corner the distance between nozzle and bed is the same - and the bed is level.

It is important to understand, though, that at this point the actual distance doesn't matter too much. If it is a paper or a penny between nozzle and bed doesn't make a big difference. Usually you would still make sure afterwards that the distance is perfect using e.g. _z-offset_.

<div class="gallery-grid" style="float:left">
<figure style="font-size:small;width:300px;">
  <img src="/images/3d-print/bed-visualizer.png" style="">
  <figcaption>The octoprint plugin "bed visualizer" allows you to see the ABL mesh</figcaption>
</figure>
</div>


After you leveled the bed manually you can use "auto bed levelling" (ABL, if your printer has that). With ABL your printer will automatically approach e.g. 16 different positions on the bed with the ABL probe. This way the printer has a model of the bed position by having very accurate measurements of those 16 positions. So the printer can calculate if the bed is level - and if not how the correction would look like. Obviously it would be very expensive and complex to automatically correct the physical location of the bed. Therefor ABL compensates for any offset in software - by adding or subtracting a certain Z-value to any z-position that the nozzle is trying to reach in future. This is also the reason why ABL doesn't really free you from manual leveling: You will still need to make sure that the bed is somewhat level - so ABL can do the rest. The benefit of ABL is, that it can compensate for minor errors in levelling - and also compensate physical deformations of the bed that couldn't be fixed with manual levelling.  

### Z-Offset
After all the levelling your bed should be in perfect shape. However: It is still undefined, how far your nozzle is actually away from the printer. This is even the case for ABL - as there is no information how long your nozzle is; or how much shorter your nozzle is than the ABL probe.

The Z-Offset allows you to ensure that for Z=0 the nozzle has the perfect distance from the bed. What "perfect" means is something that is cause of a lot of drama in the internet. From my experience I usually have good results when ensuring that Z=0 is at a point where a thin paper strip matches between nozzle and bed. 

The Z-Offset can usually be defined in your slicer or your 3D printer. I always configure it via printer:

* Move the nozzle to Z=0 by pressing the corresponding button in your printer menu
* After the nozzle moved to that position try to fit a thin paper strip between nozzle and bed
* Adjust the nozzle distance pressing the Z+=0.025 or Z-=0.025 buttons on the printer until you can feel a little resistance on the paper strip when moving it back and forth
* Save the new configuration to EEProm

{{< admonition type=warning title="Warning" open=true >}}
Attention: It cost me some evenings to find out that (when you are doing this over and over again) you actually need to make sure that the printer loads the changed value for Z-Index. This usually happens by homing the print head again - or running the g-code `G28 Z` in e.g. octoprint. Only after that it makes sense to check the position of Z=0 again.
{{< /admonition >}}

Once the z-offset is dialed in you can check your calibration by printing a 20x20 square (only one first layer). This is printed quite quickly - and allows you to check if your z-offset is too close (first layer squished into the bed) or too far (first layer not sticking or gaps between the lines). You can even print one of those 20x20 squares in every corner and in the middle of the bed to check the overall levelling of your bed. However, from my experience with manual levelling + ABL as described before usually it is sufficient to print one of those squares in the middle. Usually today I just watch the first layer of my prints very closely… and only repeat this while levelling process when I see issues with the first layer.  

### E-Steps
Another important calibration are the so called "e-steps". They define how many steps the extruder needs to move in order to extrude 1mm of filament. Wrong values might lead to under- or overextrusion. 

<div class="gallery-grid" style="float:right;">
<figure style="font-size:small;width:300px;">
  <img src="/images/3d-print/e-steps.png" style="">
  <figcaption>The command M503 shows the current e-step configuration</figcaption>
</figure>
</div>

Usually e-steps are configured by adding a mark to the filament 120mm above the extruder. Now you extrude 100mm of filament, e.g. with the command `G1 E100 F100`. Now you measure how much filament was extruded by measuring the filament from the extruder to the mark you made earlier. If you measure 20mm the e-steps are dialed in perfectly (as we set the mark at 120mm and just extruded 100mm). 

In order to set the correct e-steps, proceed as follows:

- Read the current e-steps using the `M503` command. In my case: `e_steps = 509.15`
- Measure the actually extruded filament: `120 - length_extruder_to_mark = extruded_mm`. In my case: `120 - 30 = 90`
- Calculate the number of e-steps that would have resulted in the correct extrusion: `e_steps * 100 / extruded_mm = new_e_steps`. In my case `509,15*100/90 = 565,72`
- Set the new e_steps with `M92 E565.72` and save with `M500`


### Flow
Once we made sure that the extruder extrudes exactly the amount of filament requested by adjusting the e-steps, we can now fine tune how much filiament we want to extrude. The so-called "flow rate" is a multiplier that allows us to increase or decrease the amount of filament that is extruded per second while printing. 

Typically, the calibrations happens by printing an empty 20x20 cube and measuring the wall thickness: If the walls are too thick, the flow rate is reduced; if the walls are too thin, the flow rate should be increased.

For more details and the comfortable online calculator visit https://3dprintbeginner.com/flow-rate-calibration/.

## Slicer
In order to 3D print you will usually need a 3D model (often delivered in form of a .STL file) - and translate that into instructions for the 3D printer. This process is called "slicing" - the corresponding software is called "slicer". The software will "slice" the 3D model into individual layers and for each layer create instructions where the nozzle of the printer needs to go and how much filiament it should extrude.

The output format of slicers is called "gcode" - this file format can be consumed by all modern consumer 3D printers. It usually looks something like this:

```
…
G1 X102.092 Y110.276 E463.18115
G1 X101.958 Y110.929 E463.20356
G1 X101.721 Y111.587 E463.22708
…
```

Each line is an instruction, [`G1` is "linear move"](https://marlinfw.org/docs/gcode/G000-G001.html). The following parameters are the X and Y coordinate the print head should move to - and the amount of filament that should be extruded (more precise: how many e-steps the extruder should move). Of course there are [many other](https://marlinfw.org/docs/gcode/G000-G001.html) instructions as well, e.g. to set temperature, set speed etc. This way, line by line, layer by layer the 3D model is printed. 

A small 3D print might easily have 10.000 of those instructions, more complex and bigger once even more.  

Common slicers are:

- [Slic3r](https://slic3r.org/)
- [Prusaslicer](https://www.prusa3d.com/de/page/prusaslicer_424/)
- [UltiMaker Cura](https://ultimaker.com/de/software/ultimaker-cura)

Personally I used Prusaslicer a lot as I liked the way it handles presets and configurations - but also Cura is a very popular choice and has a wide range of plugins in the market place that is powered by the community. For each of the slicers I'd recommend: Especially at the beginning try to stick with the "beginner" configuration set and don't change to many settings at once: It is very easy to mess up the configuration - but very hard to find out, what setting exactly now results in poor prints, especially if you perhaps configured that setting weeks or month ago.



## Octoprint

### Plugins

#### Octolaps
#### Print time genius
#### Preheat button
#### Telegram
#### Slicer Thumbnails

## CAD

## Getting 3D printable files

## Prints


