---
layout: post
title: S.I Units are boring - let's replace them!
author: Joshua Britain
tags: Project Web
---

The S.I Units (International System of Units) is the standard system of measurement used to give values for 1 unit of something. Each of these units are derived from 7 base units for time, length, mass, current, temperature, amount of substance, and luminous intensity. The standard unit of mass, the kilogram, was until very recently (2019) defined by the International Prototype of the Kilogram, a lump of platinum and iridium sitting in the Archives Nationales, and it was from this that I got the idea: *what would be to stop someone from breaking into the Archives Nationales and replacing the International Prototype of the Kilogram with say, a dead pigeon?*

<figure style="display: inline-block">
    <img src="/assets/posts/si/kilogram.JPG">
    <figcaption>A replica of the prototype kilogram on display.</figcaption>
</figure>
<figure style="display: inline-block">
    <img style="max-height: 248px" src="/assets/posts/si/pigeon.jpg">
    <figcaption>A dead pigeon (or is he pining for the fjords?)</figcaption>
</figure>

Changing the definition of the kilogram would have a very large effect because a lot of units are dependent on it. Force for example is measured in kilogram metres squared, so if we make the kilogram twice as massive, suddenly all measures of force will give values half as big.

But why should we just redefine the kilogram when we ccould redefine **everything**?

Introducing...

# Nacia Netradicia
**My alternative unit system**

Nacia Netradicia is Esperanto for “national unconventional”, intended to be the exact opposite of the translation of SI (Standard Internationale): “international standard”. I've used Esperanto since it was meant to be a universal language, so it seemed fitting. I have actually made this a full on website and Github repo you can check out [here](https://jbritain.github.io/not-si-units/) if you want to read about all of the units.

With the help of my physics class, I have redefined all of the units as follows:

| Measurement | Base SI Unit | Equivalent |
|-------|-------|-------|
| Amount of Substance | Mole (mole) | Aardvark (a) |
| Length | Metre (m) | Banana Muffin (🧁) |
| Mass | Kilogram (kg) | Pigeon (🕊️) |
| Temperature | Degrees Kelvin (K) | Rimmer (🥣) |
| Luminous Intensity | Candela (cd) | Shiny (✨) |
| Electric Current | Ampere (A) | Spoon (🥄) |
| Time | Second (S) | Uncomfortable Pause (uₚ) |

There are also conversion ratios for each one and a conversion calculator, but since the ratios are subject to change (for example we decided a banana muffin wasn't big enough), I'll not include them here. I'm also slowly writing up descriptions for some of the non base units having already done so for the base ones. You may notice that some are named - I let my physics class each pick a unit and get it named after them, except for Magnus whos last name was taken so has his first name used instead. The base units are all named from inside jokes.

# The Technical Details

To start off, I have two csv files - [base.csv](https://github.com/jbritain/not-si-units/blob/main/unitGen/base.csv) and [units.csv](https://github.com/jbritain/not-si-units/blob/main/unitGen/units.csv). Originally I had JS load and display the units in a table dynamically, but then my good friend [The Goose Lord](https://github.com/TheProgramableTurtle) raised a very good point - users of the 3DS Browser would not be able to view the site in the 3DS Browser.

<figure>
    <img src="/assets/posts/si/3ds.webp">
    <figcaption>A StackOverflow comment demonstrating how 3DS users are often forgotten and discriminated against.</figcaption>
</figure>

<figure>
    <img style="max-height: 250px" src="/assets/posts/si/3ds.jpg">
    <figcaption>The website running on my 3DS XL, albeit not very well.</figcaption>
</figure>

As a result, I knew I had to go back to the drawing board, so I wrote a [python script](https://github.com/jbritain/not-si-units/blob/main/unitGen/generate_units.py) (warning, hacky code!) to generate markdown files for each unit from the CSV file. Then, through the use of Liquid filters since I was already using Jekyll, the page will now generate the table to be served statically, meaning it can be happily viewed on the 3DS browser. This also has the happy side effect of allowing a page for each unit where I can write up a description for it.

To calculate the conversion ratios for non base units, I found [this list](https://physics.nist.gov/cuu/Units/units.html) of formulae expressed in such a way that I can easily have my python script calculate the values for me without having to do any rearranging or algebra.

The conversion calculator just multiplies or divides your input by the relevant ratio.

# Conclusion

What started off as a joke actually became a fun project which improved my knowledge of dimensional analysis. A further fun project would be to actually attempt to solve physics problems using these units, I have a possible plan to create a program that converts physics exam past papers for my country to use NN units, so stay tuned!
