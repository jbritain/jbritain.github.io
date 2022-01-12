---
layout: post
title: Calculating Suicide Burns in Kerbal Space Program
author: Joshua Britain
---

A suicide burn (technically called a 'hoverslam') is quite a simple idea. You have a falling rocket, and you want to have it stop as it hits the ground (as in slow down via engine power, as opposed to stopping because it hit the ground). Basically, you want to bring your velocity down to zero metres per second just before you hit the ground. It sounds simple (not really, but let me make it dramatic, OK?). And that's what I thought. As you might expect, it wasn't.

I play a game called Kerbal Space Program. It's a fun simulation of a solar system populated by little aliens called Kerbals, and the aim is to build rockets or other spacecraft in order to get to, well, wherever the hell you want to go. It's good fun, and it can go from being as simple as building a little rocket that gets to orbit to [sending a replica helicarrier to one of the moons of the Jupiter equivalent](https://www.youtube.com/watch?v=kPprYMcS5lA). In this game, you can indeed perform a suicide burn as described earlier, but the question is this: **how do you know when to start burning?** I wanted to understand the maths behind this, and set out to do so.

# Plan A - Creating a Formula
My initial plan was to take the formulae for all of my variables and combine them into one big formula. About an hour later, having done some googling and fucking around with [Desmos](https://www.desmos.com/), I realised this was not going well.

<figure style="text-align: center;">
    <img style="max-height: 80vh;" src="/assets/posts/ksp/formulaWorking.jpeg">
    <figcaption>My initial working, in which I attempted to create a formula. Feel free to take the piss out of my handwriting and physics skills.</figcaption>
</figure>

I pretty much had no idea what I was doing and the formula I had created was a complete mess. The main problem I could identify was as follows:

- I calculate the force due to gravity at a given position using $$ F=\frac{Gm_1m_2}{r^2} $$

- However, in order to calculate my position, I need to factor in gravity to work out how far I've fallen.

Now I'm sure there is a solution to this but my knowledge of maths and physics hasn't provided one, so I was stuck. I mentioned this to my dad, and he suggested that instead of trying to make one big-ass formula where you plug in some variables and get an answer, why not write a program to solve it iteratively?

# Plan B - An Iterative Solution

So, having decided to brute force it instead, I opened up a new Python file, and got programming. My plan for the physics itself was pretty simple really:

- Calculate force upwards from engine
- Calculate force downwards from gravity
- Divide both forces by the mass to get acceleration
- Subtract gravity acceleration from engine acceleration
- Add combined acceleration to velocity
- Add velocity to position

*Note: at this point the solution operates one-dimensionally - assuming you fall straight down and that you have no horizontal velocity. Air resistance is also of course negligible.*

## Fuel Consumption
My first major issue was calculating fuel consumption. I followed [this page on the KSP Wiki](https://wiki.kerbalspaceprogram.com/wiki/Specific_impulse) to do it, but initially got stuck, getting wildly different values to what I should have been getting. Eventually I found the problem - The I<sub>sp</sub> listed on the wiki was actually the I<sub>sp,g<small>0</small></sub>, and so I had to factor in gravity to the equation. Therefore my fuel consumption formula is as follows:

$$ \frac{T}{I_{sp} \cdot 9.8} $$
(C=Consumption, T=Thrust of engine)

This was now giving me the correct value and I knew this because in KSP you can right click on an engine and it will show you the fuel consumption.

## Checking if we had landed or not
Now my craft was being simulated, the next thing to do was work out if we had landed, and if not, what to do about it. Here's what I came up with

We have a variable called `precision`, which starts at 1. This is essentially the time delta for the loop that simulates the rocket.
We also have a variable called `startOffset`. This is how long the craft waits before burning the engine.
In this case, I'm also creating something called the 'offset modifier'. This is equal to $$ \frac{1}{10P} $$. With all this, I

- Simulate until craft has landed, run out of fuel, or crashed
- If we have landed: congratulations, suicide burn. You should start burning at the start offset.
- If we have crashed and run out of fuel, the suicide burn is impossible.
- If we have run out of fuel, we burned too early. Increase the startOffset by the offset modifier.
- If we have crashed, we burned too late. Since this should only happen now once we have increased the start offset, we have obviously gone past the point where the burn is impossible. Double the precision, and decrease the startOffset by the modifier.

This should have worked, and I was stumped for a couple of days when for some reason it would suddenly start drastically increasing both start offset and precision. Eventually after some debugging, I noticed that the acceleration of the craft went from a reasonable number at precision 15.92 to a huge number at precision 15.93. I have no idea what caused this specific behaviour but I discovered the source was that I wasn't factoring the precision into acceleration - so if we were at double precision the craft would accelerate twice as fast. Having changed this, voila, it worked!

# Creating a website
Having got my Python prototype working, I decided to create a website so that anyone else who is conveniently heading directly downwards in a craft with only one engine to a planet with no atmosphere could also calculate when to start burning. I decided not to style it with [Primer CSS](https://primer.style/css) for a change, and instead went with Bootstrap. The reason was that the inspiration for this site partially came from [alexmoon's Launch Window Planner](https://alexmoon.github.io/ksp/). I liked the UI style of this website and so shamelessly copied it, including using an older version to get the same aesthetic. As it is, I quite like bootstrap's column system so I think I'll actually use the latest version in my next few web projects.

*Edit: since then I have updated the site to use Bootstrap 5.*

<figure style="display: inline-block">
    <img style="max-width: 40vw;" src="/assets/posts/ksp/site.png">
    <figcaption>My site.</figcaption>
</figure>
<figure style="display: inline-block">
    <img style="max-width: 40vw;" src="/assets/posts/ksp/windowplanner.png">
    <figcaption>alexmoon's Launch Window Planner.</figcaption>
</figure>

Surprisingly, converting the tool into a website went without a hitch; I translated my Python into JavaScript, created a form to input the numbers, and now I have a working calculator! You can view it [here](https://pr0x1mas.github.io/KSP-Suicide-Burn-Calculator/).

# Future plans
This tool is not finished and I plan to add the following
- A dropdown menu to select your engine instead of manually adding the stats (I'll need a script to pull the data from the wiki, I'm not manually copying it in like with the planets)
- Support for surface velocity, assuming you constantly burn retrograde
- Multiple engine support (the maths is gonna get confusing fast)
- Auto calculation of the surface height based on elevation data from the game (I have no idea if this is even possible to obtain) and coordinates
- Atmospheric support (I have no idea what I'm doing but my friend Magnus says he might try the maths)

Overall this project has been fun to work on, and I am looking forward to continuing it!
