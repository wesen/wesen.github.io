---
layout: post
title: Kodiak Curve Sequencer
category: articles
---

Native Instruments recently released a new collection of Blocks for Reaktor, called Kodiak. Reaktor Blocks is a collection of Reaktor instruments called “blocks” which can be patched together, similar to a real-life modular system. In this series, I will look at the individual blocks of the Kodiak series.

My favorite Kodiak is the curve sequencer. It is a step sequencer that takes a lot of inspiration from the Massive modulation sequencer. It consists of 2 sequencer lanes, each consisting of at most 16 steps. Each step is not just a single value, in contrast to the similar “SEQ-4 Mods” block, but a curve.

<div align="center">
<figure>
     <a href="{{site.url}}{{site.baseurl}}/figs/2017-08-23--curve-sequencer/PastedGraphic.png" class="image-popup">
     <img src="{{site.url}}{{site.baseurl}}/figs/2017-08-23--curve-sequencer/PastedGraphic.png">
     </a>
     <figcaption>The Curve Sequencer UI</figcaption>
</figure>
</div>

The module receives a clock and a reset signal, as well as the usual modulation inputs. Each lane is output on a separate output. A mix of the two lanes (controlled by a crossfader) is available on a separate output.

;figureCaption

<div align="center">
<figure>
     <a href="{{site.url}}/{{site.baseurl}}/figs/2017-08-23--curve-sequencer//PastedGraphic1.png" class="image-popup">
     <img src="{{site.url}}/{{site.baseurl}}/figs/2017-08-23--curve-sequencer//PastedGraphic1.png">
     </a>
     <figcaption>The Curve Sequencer block</figcaption>
</figure>
</div>

The sequencer has the usual direction controls (left to right, right to left, left to right to left, brownian, full random), sequence length, sequence offset and polarity. Things get more interesting on the right side. Clicking the CURVES button opens the curve selection panel. Drawing on the sequencer steps will place the currently selected curve into that cell, allowing the user to quickly draw up (and switch) interesting modulation curves. I find it especially fun modulating oscillator pitches with it.


<div align="center">
<figure>
     <a href="{{site.url}}/{{site.baseurl}}/figs/2017-08-23--curve-sequencer/PastedGraphic2.png" class="image-popup">
     <img src="{{site.url}}/{{site.baseurl}}/figs/2017-08-23--curve-sequencer/PastedGraphic2.png">
     </a>
     <figcaption>Curves selection</figcaption>
</figure>
</div>

The length of each curve can be either set to AUTO, in which case it matches the time measured between the last two gate signals, or set manually using the GATE control.

<div align="center"><figure>
     <a href="{{site.url}}/{{site.baseurl}}/figs/2017-08-23--curve-sequencer/PastedGraphic3.png" class="image-popup">
     <img src="{{site.url}}/{{site.baseurl}}/figs/2017-08-23--curve-sequencer/PastedGraphic3.png">
     </a>
     <figcaption>Selecting gate length</figcaption>
</figure>
</div>

The Out Mix output interpolates between both sequences, based on the position of the crossfader. Of course, the crossfader can be automated, making it possible to create crazy morphing sequences (if for example controlled by a square LFO.


<div align="center"><figure>
     <a href="{{site.url}}/{{site.baseurl}}/figs/2017-08-23--curve-sequencer/PastedGraphic4.png" class="image-popup">
     <img src="{{site.url}}/{{site.baseurl}}/figs/2017-08-23--curve-sequencer/PastedGraphic4.png">
     </a>
     <figcaption>Crossfading between sequences</figcaption>
</figure>
</div>

Finally, the killer feature is the possibility to randomize the sequences to explore the possible modulation space. Which step is randomized can be set by the switches below each step:


<div align="center"><figure>
     <a href="{{site.url}}/{{site.baseurl}}/figs/2017-08-23--curve-sequencer/PastedGraphic5.png" class="image-popup">
     <img src="{{site.url}}/{{site.baseurl}}/figs/2017-08-23--curve-sequencer/PastedGraphic5.png">
     </a>
     <figcaption>Selecting randomization steps</figcaption>
</figure>
</div>

You can then select to randomize the curve type, or the curve level, or both.

<div align="center"><figure>
     <a href="{{site.url}}/{{site.baseurl}}/figs/2017-08-23--curve-sequencer/PastedGraphic6.png" class="image-popup">
     <img src="{{site.url}}/{{site.baseurl}}/figs/2017-08-23--curve-sequencer/PastedGraphic6.png">
     </a>
     <figcaption>Randomization menu</figcaption>
</figure>
</div>

This block really has become my go-to block for modulation, as it is highly controllable, yet allows for quick experimentation due to the RANDOM functionality.

Here is for example a surprisingly versatile single osc patch:


<div align="center"><figure>
     <a href="{{site.url}}/{{site.baseurl}}/figs/2017-08-23--curve-sequencer/PastedGraphic7.png" class="image-popup">
     <img src="{{site.url}}/{{site.baseurl}}/figs/2017-08-23--curve-sequencer/PastedGraphic7.png">
     </a>
     <figcaption>Simple Curve Sequencer patch</figcaption>
</figure>
</div>
