---
layout: page
title: Making labs more sustainable with FRED
description: A journey to find and easily fix hidden emissions of research labs
img: assets/img/fhf.jpg
importance: 1
category: work
---

## Making research labs more environmentally friendly with “FRED”

We are all aware that switching to renewable, zero-emissions energy as fast as possible is critical to mitigate the impact of climate change. 
But there is another requirement to reach net zero emissions: we need to drastically reduce the amount of energy we use. The International Energy Agency (IEA) has estimated that, even with widespread renewables uptake, we need to [triple the current rate of energy efficiency improvements in buildings over the next ten years](https://www.iea.org/reports/net-zero-by-2050). 

Research labs should not be exempt from this. But how much energy do research labs use? I had a look at the research building I work in at the University of Melbourne (Chemistry).

Over the past week, binning the data into hourly blocks, there was a fairly constant baseline of around 1,100 kWh/h energy use (that is, 1,100 kWh of energy is consumed each hour). During working hours, consumption peaks between 1,500-1,900 kWh/h. To contextualise these numbers, the fridge/freezer in a residential house might use 0.25 kWh/h. 
 
Let’s average that out some more. Over the past year, binning the data into daily blocks, energy consumption was around 27-28 MWh per day on the weekend and around 30-40 MWh per day Mon-Fri. Adding up over a year, from September 2022-September 2023 our building consumed 11,575 MWh of energy. (I have no idea why the energy use is so different in Jan-March 2023 than the rest of the year - maybe some change in how the energy use was metered?).
 

These numbers are pretty hard to understand without context. How do we compare to other industries or residential energy use? To do this, I’ll take average/typical numbers for energy use (per year) for each sector and divide by m2 of floor space, which is a little arbitrary (why not divide by number of people working in each building?) but it’s how the data is presented in the [DCCEW “Commercial Buildings Energy Consumption Baseline Study 2022”.](https://www.energy.gov.au/publications/commercial-buildings-energy-consumption-baseline-study-2022).

By this measure, our building is using about 40x the energy as a residential space and 3.2x a medical space. This isn’t surprising, as research buildings have lots of equipment (lasers, spectrometers, liquid nitrogen tanks, air extraction/flow fans, etc) and we have hundreds of students (undergraduate and graduate) doing research or coursework experiments during the day.
 
With this in mind, I set about to see if we could make research labs in our building more efficient without having to rebuild them or buy any expensive new equipment.
Turning off lights and equipment after being used is already pretty common practice, so that didn’t seem like a useful avenue to explore. Our lab has some very power-hungry lasers and heating equipment, but in both cases, these are on turned on when needed, and necessary for research. But then I came across a “Chemistry World” article, “How to run a sustainable chemistry lab” (December 2021), in which fume hoods identified as a major culprit of energy waste in labs.
 
A fume hood or fume cupboard is a big cupboard-like equipment with a clear plastic, vertically closing slide door or ‘sash’. Researchers working with hazardous chemicals will keep their experiment in the fume hood, and powerful fans at the top of the fume hoods extract dangerous chemical vapours. 
 
When the fume hood sash is closed, the fans in each fume hood draw little air, and energy use is low. But when open, fume hood fans can consume around 2-3 kWh per hour, about the same as 3 residential houses. This is a necessary evil when the researcher is doing their experiments in the fume hood (typically a few hours a day for a chemist).

But above all else, I already knew from our lab that researchers leave the fume hoods open a lot. No amount of asking nicely seems to change this and university research labs have no real organisational structures to enforce behavioural changes. To make matters worse, our building probably contains 50-100 or so fume hoods, most of them without any features to automatically close or turn off if they are left open.

This was the ‘easily avoidable’ energy consumption I was looking for – we just need to figure out how to get research students to close their fume hoods when not in use!

Others have made elaborate stickers that warn users of the horrible energy consumption of a fume hood left open. I tried this in our lab but it seemed mostly useless. The fume hoods were still left up.

At this point I’d been learning Python and playing around with Raspberry Pi microcontrollers for fun, so I decided to build a retrofit device which would detect fumehoods being left open and somehow encourage people to close them.

The result was FRED (Fumehood Reduction in Energy Device). FRED is a cheap fumehood retrofit using a Raspberry Pi Pico (under AUD$10 at the time of writing) and a few off-the-shelf electronic items (two buzzers and a magnetic switch). The whole thing costs just $20-25 per fumehood. I also designed a little 3D printable case that helps to enclose it all and line up the sensors.
 
Once assembled, which requires only a few minutes (a bit of soldering + upload the code onto the RPi Pico), FRED is attached to a fume hood. The magnetic Reed switch tells the Pi if the fume hood is open or closed.

If the fume hood is left open for more than 1 hour, FRED starts to make a loud and annoying buzzing noise.

With a little more set up work, an advanced version of FRED also posts facts about energy consumption and climate change onto our lab’s Teams channel.

Last but not least, FRED plays a very happy and satisfying little chromatic riff when the fume hood is closed. (The carrot approach).

Annoyed by a loud noise or guilted by climate facts, one of our many research students then runs into the lab to close the fumehood, quiet FRED and restore their inner calm.

Of course FRED logs data too so I ran a few experiments to see if it was working. I left FRED with the buzzers off in a fume hood for 1 month to log baseline data, then turned on FRED’s buzzers and recorded the effect on how often and how long the fumehood was left open.

Using some calculators available online, I converted time left open into an estimated energy consumption and plotted the results. There’s a very clear reduction in energy consumption of around 33% or around 3 MWh per year per fume hood. When installed in all four fumehoods in our lab, this equates to about 13 tCO2e avoided, roughly equivalent to removing 3 cars from the road. (And saves the Uni around $2,500 per year in energy bills).

But that’s just for one lab – we estimated there are around 100 fume hoods in the Chemistry department alone. That means we could reduce the total energy consumption of our department by around 300 MWh per year, around 2-3% of the current 11,500 MWh yearly consumption figure, and over 300 tCO2e avoided (they yearly equivalent of approximately 70 cars). That’s pretty decent for a retrofit that costs $25 and only takes a few minutes to install.
 
It seems that the best way to disseminate FRED and push the impact is to open-source it all, so I’ve made the code, 3D print files for (optional) case, and instructions available on GitHub [here]( https://github.com/nrmkirkwood/FumeHoodFred).


## Sustainability in labs

Research labs like the one I work in produce next-generation solar cells which 

Programs I'd seen advertised advertised at the University of Melbourne were all fairly office-specific - turning off lights and computers, getting coffee in re-usable cups, running swap meets etc. 


## FRED: 'Fumehood Reduction in Energy Device'



Every project has a beautiful feature showcase page.
It's easy to include images in a flexible 3-column grid format.
Make your photos 1/3, 2/3, or full width.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/1.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/3.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Caption photos easily. On the left, a road goes through a tunnel. Middle, leaves artistically fall in a hipster photoshoot. Right, in another hipster photoshoot, a lumberjack grasps a handful of pine needles.
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This image can also have a caption. It's like magic.
</div>

You can also put regular text between your rows of images.
Say you wanted to write a little bit about your project before you posted the rest of the images.
You describe how you toiled, sweated, *bled* for your project, and then... you reveal its glory in the next row of images.


<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    You can also have artistically styled 2/3 + 1/3 images, like these.
</div>


The code is simple.
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).
To make images responsive, add `img-fluid` class to each; for rounded corners and shadows use `rounded` and `z-depth-1` classes.
Here's the code for the last row of images above:

{% raw %}
```html
<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
```
{% endraw %}
