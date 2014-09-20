---
layout: post
title: Pixel Heart v2 Research
permalink: pixelheartv2research
---

Research on Pixel Heart v2 has begun. My goal with this iteration is to accomplish the following:

> Check Pixel Heart as luggage on a plane.

Pretty simple, but quite the challenging mission.

<!-- more -->

## Initial Ideas

I definitely won't be using 4x8' panels this time around. They were too heavy, making them too difficult to move from place to place. Besides, there's no way in hell I'm getting three 4x8' panels on a plane.

This time around I need to optimize for *weight* and *volume*. In order to achieve this I'm going to need to make the pixels much more modular. Ideally each pixel will be a completely independent unit and I can chain them together electrically while attaching them together physically. It would also be nice if the distance between pixels is flexible so that the Heart can play in three dimensions while still allowing a tight fit to make the 48x32 display.

## Playa Magic

As it seems to happen, Burning Man provided some inspiration this year with the Salsa Dome. Tango'd Up in Blues had built a dome where, rather than use the traditional tarp cover, they filled the space with triangular edge-lit acrylic panels.

[![The Salsa Dome](/images/salsadome.jpg)](http://lightatplay.net/)

I first saw the Salsa Dome in 2013 and thought it was wonderful, but I didn't connect the practicality of the panels at the time. After seeing it this year with Pixel Heart v2 churning in the background needless to say a bunch of LEDs turned on my head.

## Taking Measurements

Using edge-lit acrylics could be the perfect solution for Pixel Heart v2, but I'm going to check that the metrics make sense first. After some quick research I was able to find the product they used for the dome: [LuciteLux Light Guide Panels (LGP)](http://lucitelux.com/product/light-guide-panels/). These acrylic panels are edge-lit and clearly provide a significant amount of light. I'm not sure what sort of LED density was used on the Salsa Dome, but I've emailed the original makers and will hopefully get some more insight as to how they made their panels.

In order to figure out if this idea has any chance of meeting the original goal it's time for some basic calculations.

### Volume

Keeping the original 3 by 3" pixel design and assuming in the "biggest" case I go with the 8mm panels, a single pixel will have a volume of 3 x 3 x 0.315 = 2.84 inches. With 1536 pixels that gives me a minimum total volume of 4354 inches cubed.

> 4354 inches cubed for the pixels

A suitcase's dimensions can't exceed 62" [according to United](http://www.united.com/CMS/en-US/travel/Pages/BaggageChecked.aspx). I'll be using the [BA30 Elite Vacationer Pelican Case](http://www.pelican.com/cases_detail_luggage.php?Case=BA30) as my basis for measurements. This suitcase is badass coming in at 60.81" in total dimensions. Its internal dimensions are 25.85" x 16.98" x 10.93", giving it a volume of 4797.54.

> 4797.54 inches cubed for a pelican suitcase

Beautiful - that gives us 300 cubic inches of space. With a standard 30A 12V PSU at 71.4 cubic inches that's roughly 4 power supplies worth of extra space. *Looking good...* now let's check the weight.

### Weight

The [LGP spec sheet](http://lucitelux.com/wp-content/uploads/2014/02/LuciteLux_LightGuidePanelTechnicalBulletin.pdf) indicates that the material weighs 1.19 g/cm<sup>3</sup>. The dimensions of the Pixel Heart in cm are 365.76 x 243.84 x 0.8 cm = 71,349.54 cm<sup>3</sup>, giving us a rough weight of 84.91kg. *Damn*, that's heavy.

Ok, what if we used the 6mm panels? At 53,512.15 cm<sup>3</sup> it's still 63.68kg.

It's too heavy - and this doesn't even include the wiring/PCBs/LEDs. Disappointing, but on to the next idea!
