---
layout: page
title: Designing FRED, a retrofit device to mitigate reseach lab emissions
description: (with some analysis and musings on lab energy consumption)
img: assets/img/fhf-horiz.jpg
importance: 1
category: work
---
Last updated: 22 September, 2023

## tl;dr
I designed, tested and built a simple retrofit device, FRED, that reduces our research lab energy consumption by a couple of percent using some very cheap off-the-shelf electronics and a few dozen lines of code. 

FRED works by alerting researchers when they leave open their 'fume hoods' (cupboards where chemicals are handled). The data from a trial over 2 months in a real lab shows that each fume hood used an estimated 33% less energy after FRED was installed, corresponding to about 3.3 tCO2e avoided per fume hood. 

I've recently noticed an [incredible similar device being described by MIT researchers in a peer-reviewed publication](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8560830/) and assosciated [start-up company](https://thelichenlab.com/). Their results/product are encouragingly similar and validate that this is a good idea and the results are verifiable.

## All the details
### The need to reduce our energy demand

We are all aware that switching to renewable, zero-emissions energy as fast as possible is critical to mitigate the impact of climate change. 
But there is another requirement to reach net zero emissions: we need to drastically reduce the amount of energy we use. The International Energy Agency (IEA) has estimated that, even with widespread renewables uptake, we need to [triple the current rate of energy efficiency improvements in buildings over the next ten years](https://www.iea.org/reports/net-zero-by-2050). 

### Application to research labs

Research labs should not be exempt from this. But how much energy do research labs use? I had a look at the research building I work in at the University of Melbourne (Chemistry).

<div class="row justify-content-sm-center">
    <div class="col-sm-12 mt-3 mt-md-0">
        {% include figure.html path="assets/img/fhf-chem-week.jpg" title="Chemistry Building hourly energy consumption over 1 week" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-12 mt-3 mt-md-0">
        {% include figure.html path="assets/img/fhf-chem-year.jpg" title="Chemistry Building daily energy consumption over 1 year" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Top: Chemistry Building hourly energy consumption over 1 week. Bottom: Chemistry Building daily energy consumption over 1 year. Source: University of Melbourne Utility Report Database via Clariti (https://sustainablecampus.unimelb.edu.au/energy/on-campus)
</div>

Over a single week, binning the data into hourly blocks, there was a fairly constant baseline of around 1,100 kWh energy consumption per hour. During working hours, consumption peaks between 1,500-1,900 kWh per hour. To contextualise these numbers, the fridge/freezer in a residential house might use 0.25 kWh per hour. 
 
Let’s average that out some more. Over the past year, binning the data into daily blocks, energy consumption was around 27-28 MWh per day on the weekend and around 30-40 MWh per day Mon-Fri. Adding up over a year, from September 2022-September 2023 our building consumed 11,575 MWh of energy. (I have no idea why the energy use is so different in Jan-March 2023 than the rest of the year - maybe some change in how the energy use was metered?).

These numbers are hard to understand without a comparison to other industries or residential energy use. To do this, I’ll take average/typical numbers for energy use (per year) for each sector and divide by area of floor space. This might seem a little arbitrary - why not divide by number of people working in each building - but it’s how the data is presented in the [DCCEW “Commercial Buildings Energy Consumption Baseline Study 2022”](https://www.energy.gov.au/publications/commercial-buildings-energy-consumption-baseline-study-2022) which makes it a convenient way to compare energy consumption. For residential, I took the average home energy use in Victoria from the [AER Residential Energy consumption benchmarks (Table 9, averaged across all DNSPs)](https://www.aer.gov.au/system/files/Residential%20energy%20consumption%20benchmarks%20-%209%20December%202020_0.pdf) and the average floor area of a new house in Victoria in 2020-2021 from the [ABS](https://www.abs.gov.au/articles/average-floor-area-new-residential-dwellings).

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/fhf-building-energy-use.jpg" title="Energy use of buildings in different sectors" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Comparison of yearly UoM chemistry building energy consumption (per unit floor area) to buildings in other sectors. Sources in text. Chemistry building total floor area estimated from Google Maps and internal information.
</div>

By this measure, our building is using about 40x the energy as a residential space and 3.2x a medical space (per unit floor area). This isn’t surprising, as research buildings have lots of equipment (lasers, spectrometers, liquid nitrogen tanks, air extraction/flow fans, etc) and we have hundreds of students (undergraduate and graduate) doing research or coursework experiments during the day.

### Identifying obvious targets for energy reduction
 
With this in mind, I set about to see if we could make research labs in our building more efficient without having to rebuild them or buy any expensive new equipment.

Turning off lights and equipment after being used is already pretty common practice, so that didn’t seem like a useful avenue to explore. Our lab has some very power-hungry lasers and heating equipment, but in both cases, these are on turned on when needed, and necessary for research. 

But then I came across a “Chemistry World” article, “How to run a sustainable chemistry lab” (December 2021), in which fume hoods identified as a major culprit of energy waste in labs. There have been various [LinkedIn posts](https://www.linkedin.com/pulse/4-steps-cutting-energy-costs-fume-hoods-balcon/?trk=organization-update-content_share-article) on the topic and even [peer-reviewed publications](https://www.sciencedirect.com/science/article/abs/pii/S0360544204004906) on the huge avoidable emissions from fume hoods.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/fhf-chemworld.jpg" title="Chemistry World article on lab sustainability" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/fhf-fumehood.jpg" title="A fume hood in a chemistry lab" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: Chemistry World article on lab sustainability. Right: Own photo of a fume hood in a chemistry lab, with the clear plastic sliding door or 'sash' left half open in this case.
</div>

### Fume hoods: A major culprit in avoidable energy consumption
 
A fume hood or fume cupboard is a big cupboard-like equipment with a clear plastic, vertically sliding door called a ‘sash’. Researchers working with hazardous chemicals will keep their experiment in the fume hood, and powerful fans at the top of the fume hoods extract dangerous chemical vapours. 
 
When the fume hood sash is closed, the fans in each fume hood draw little air, and energy use is low. But when open, fume hood fans can consume around 2-3 kWh per hour, about the same as 3 residential houses. This is a necessary evil when the researcher is doing their experiments in the fume hood (typically a few hours a day for a chemist).

But above all else, I already knew from our lab that researchers leave the fume hoods open a lot. No amount of asking nicely seems to change this and university research labs have no real organisational structures to enforce behavioural changes. To make matters worse, our building probably contains 50-100 or so fume hoods, most of them without any features to automatically close or turn off if they are left open.

This was the ‘easily avoidable’ energy consumption I was looking for – we just need to figure out how to get research students to close their fume hoods when not in use!

Others have made elaborate stickers that warn users of the horrible energy consumption of a fume hood left open. I tried this in our lab but it seemed mostly useless. The fume hoods were still left up.

### My solution: FRED (Fumehood Reduction in Energy Device)

At this point I’d been learning Python and playing around with Raspberry Pi microcontrollers for fun, so I decided to build a retrofit device which would detect fumehoods being left open and somehow encourage people to close them.

The result was FRED (Fumehood Reduction in Energy Device). FRED is a cheap fumehood retrofit using a Raspberry Pi Pico (under AUD$10 at the time of writing) and a few off-the-shelf electronic items (two buzzers and a magnetic switch). The whole thing costs just $20-25 per fumehood. I also designed a little 3D printable case that helps to enclose it all and line up the sensors.

<div class="row justify-content-sm-center">
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/fhf-final.jpg" title="Latest FRED" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/fhf.jpg" title="FRED smiling with its lid on" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/fhf-proto.jpg" title="prototyping FRED" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/fhf-teamspost.jpg" title="Example of a post to MS Teams" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Top left: The latest iteration of FRED including 3D printed case (lid off). Top middle: FRED with smiley lid. Top right: An early prototype - a lot more wires. Bottom: An example of a Microsoft Teams post that FRED makes when left up for too long. 
</div>
 
Once assembled, which requires only a few minutes (a bit of soldering + upload the code onto the RPi Pico), FRED is attached to a fume hood. The magnetic Reed switch tells the Pi if the fume hood is open or closed.

If the fume hood is left open for more than 1 hour, FRED starts to make a loud and annoying buzzing noise.

With a little more set up work, an advanced version of FRED also posts facts about energy consumption and climate change onto our lab’s Teams channel.

Last but not least, FRED plays a very happy and satisfying little chromatic riff when the fume hood is closed. (The carrot approach).

Annoyed by a loud noise or guilted by climate facts, one of our many research students then runs into the lab to close the fumehood, quiet FRED and restore their inner calm. (As a side note, it quickly became clear the annoying buzzing sound was more effective than the MS Teams posting at changing behaviour, but it's fun to do both.)

### Meausing FRED's impact

Anecdotally, the fume hoods spend a lot more time closed now. The Raspberry Pi at the heart of FRED is easily capable of data logging so I ran a long-term experiment to see if FRED had a meaningful effect. I left FRED with the buzzing noises (and Teams posts) off in a fume hood for 1 month to log baseline data, then turned on FRED’s buzzers and posting and recorded for another month the effect on how often and how long the fumehood was left open. The data shows a clear reduction in time left open, driven by significantly fewer episodes where a fume hood is left open for over an hour. Using the [LBL fumehood energy use calculator available online](https://fumehoodcalculator.lbl.gov/) with inputs based on our fume hoods, I converted time left open into an estimated energy consumption.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/fhf-cumulative.png" title="cumulative time open comparison" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/fhf-results.jpg" title="results of FRED trial" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: Cumulative time fumehood left open for control (alerts off) and with FRED buzzer on (alerts on) during two measurement time periods of the same length. Right: Comparison of estimated energy use per fume hood per year before and after FRED is installed. The data is estimated by comparing data logs taken during a control month with FRED installed in a fume hood and logging data with 'alerts off' (no buzzer or posts) and a month in the same fume hood with the alerts on (buzzer and Teams posting activated).
</div>

This is a back-of-the-envelope way to calculate energy use, but the result was clear: A *significant* reduction in energy consumption of around 33% or around 3 MWh per year per fume hood. When installed in all four fumehoods in our lab, this equates to about 13 tCO2e avoided (using the [DELWP GHG coefficient of 1.09 tCO2e/MWh from 2021](https://www.esc.vic.gov.au/sites/default/files/documents/greenhouse-gas-co-efficient-2021_0.pdf)), roughly equivalent to [removing 3 cars from the road](https://www.epa.gov/greenvehicles/greenhouse-gas-emissions-typical-passenger-vehicle). FRED also saves whoever pays the bills for our lab around $2,500 per year, assuming an energy price of [$0.21/kWh](https://www.canstarblue.com.au/electricity/electricity-costs-kwh/). 

### Impact for the entire building

The data above is just for one lab – we estimated there are around 100 fume hoods in the Chemistry department alone. That means we could reduce the total energy consumption of our department by around 300 MWh per year, around 2-3% of the current 11,500 MWh yearly consumption figure, and over 300 tCO2e avoided (they yearly equivalent of approximately 70 cars). That’s pretty decent for a retrofit that costs $25 and only takes a few minutes to install.

### Due-diligence

As a final bit of due-diligence: surely there's existing ways to solve this obviously quite significant energy use problem? The answer is yes, but at a significant cost. I asked our lab system provider to quote a retrofit or refit of our furmehoods to incorporate an energy saving feature that closes the sash (or at least alarms when left open) and we got a quote of just over $4,000 per fume hood (excl GST) which is about 150-200 times more expensive than FRED.

As a final note, I've recently noticed an [incredible similar device being described by a team at MIT in a peer-reviewed publication](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8560830/) and an assosciated [start-up company](https://thelichenlab.com/). Their device even has a fun name (MASH). They arrive at encouragingly similar results to those obtained by FRED, with a more sophisticated and longer-term analysis involving many more fume hoods. 

The fact that somebody else had the same idea and executed it at the same time, wrote a paper on it, and started a company to commercialise it, proves the idea was good and the similarity of our results is a great outcome.

### Acknowledgements

The FRED project was greatly assisted by the discussions, advice and superior soldering skills of Jack Heywood-Day and Trent Ralph. Thankyou!

### Where to get FRED
 
It seems that the best way to disseminate FRED and push the impact is to make it open-source the code and make it avaialable for anyone to install, so I’ve made the code, 3D print files for (optional) case, and instructions available on GitHub [here]( https://github.com/nrmkirkwood/FumeHoodFred).