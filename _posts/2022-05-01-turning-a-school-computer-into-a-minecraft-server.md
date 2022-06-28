---
layout: post
title: Turning an Old School Computer Into a Minecraft Server - Part 1
author: Joshua Britain
tags: Project Linux Hardware Minecraft
---

Recently I came into possession of a computer my school was throwing out. It's pretty old, in fact it's from roughly 2009, a HP Compaq Elite 8000 with a Core 2 Duo E8400 and 2GB of RAM. Yes, this thing was in use until it was thrown out, and while most of the other computers in my school are newer, the room this particular model came from is still full of these old machines, and let me tell you, *they are unusable*, even on Windows 7. I guarantee the bottleneck is the RAM. Honestly, I was lucky it even worked - I only found out I could take it because I wanted a dead monitor for the VESA stand (side rant, the stand didn't even fit the monitor I had because HP added a bulge in the middle that matches a hole in the original monitor). The only problem with it was a dead CMOS battery, and even then it would boot if you set the correct system date. It certainly wasn't worth the effort of the school changing it out anyway, because they have loads of better spare computers to replace it with.

## Getting it up and running
The first hurdle I had to get over was getting it booting off a USB. As you would hope, the computer would not boot off a USB by default, and the BIOS had a password on it. Happily, it turns out there is a little green jumper inside the machine, and if you remove it before booting the PC then it will reset the BIOS password. Having done this and replaced the CMOS battery, I was able to get it to boot off something other than the hard drive. What I don't quite know is how it was storing the setup password (which was preventing me getting to the boot menu for the USB), but no matter, it works.

The next step was to clear out the 12ish years worth of dust, which I did with a hoover, I know you're not meant to do this but I pretty much did the bare minimum to clear out the fans and heatsink, and hey, it still works!

Finally, I installed Ubuntu Server 22.04 because it's what I've used in the past, and it works.

## Noise problems
Now this computer is loud. Like really loud. Here's a short audio clip of it at idle.

<audio controls>
    <source src="/assets/posts/schoolcomputer/fan.m4a" type="audio/mp4">
</audio>

What's more, the fans are not (so far as I can tell) controllable by the OS, none of the software I tried detected them on either Windows or Ubuntu Server. The BIOS of course is very bare-bones, and all that's there is 'idle fan speed' which is already set to minimum. The fans and their connectors are also proprietary as expected, and I couldn't even disconnect them without applying more force than I would like to. So it seems the only solution here would be to order more fans from one of the few eBay listings I found and somehow get the existing ones disconnected. I might do this in the future, but for the moment I think I'm just gonna have to live with it.

## Upgrade time
Back in 2009, 2GB of RAM was probably quite a respectable amount, but this isn't 2009. Running Windows on it now, the computer goes at a crawl, and while Ubuntu Server handles fine on its own, some testing showed I would not be able to use it as a Minecraft server without some upgrades.

Using Crucial's Upgrade Adviser, I found that [this Crucial memory](https://www.amazon.co.uk/gp/product/B005LDLV6S) was what I needed. I did look at equivalent versions from other brands, but this was the cheapest I could get, coming in at £22 for a 4GB stick (bringing it up to 6GB). That's not a good price by any means in my opinion, but considering this was a free computer and that they probably don't make this stuff anymore, i'm willing to pay. Ideally I would install 8GB, the maximum capacity of this machine, but I'm not forking out £44 for this thing.

Installing the RAM had a couple of hiccups. Initially, I placed the new stick in the black slot that the old stick had been in. I did this because I had to take the old stick out to get to both slots and I didn't think it would make any difference. Booting the computer back into Windows showed it did, as it BSODed on boot, tried to enter startup repair, then continued to BSOD. Going into the BIOS and running a memory test showed results that weren't promising. Hoping I hadn't bought a dead stick of RAM, I swapped them around, putting the old stick in the original slot, and bingo, it booted and worked fine!

I also dropped in a 120GB SSD because I had one laying around.

## Testing

So with the computer upgraded and running fine, I set up a Minecraft server. I installed [PaperMC](https://papermc.io/), set up the port forwarding on my router, and got some friends to join. And it worked fine! The TPS stayed at a pretty much stable 20 even with 4 or 5 players connected. Apparently there was some slowdown when loading new chunks (I wasn't on at the time), but it still remained playable.

A few days in, one of my friends who had been putting in quite a lot of time and was already fully kitted out with netherite was talking about building some farms that would not work on Spigot (the server software Paper is based on). I had initially been hesitant to use something else due to performance concerns, but decided I may as well try. I switched to [Fabric](https://fabricmc.net), and installed the [Lithium](https://www.curseforge.com/minecraft/mc-mods/lithium) and [Phosphor](https://www.curseforge.com/minecraft/mc-mods/phosphor) mods to make sure I was getting the most performance out of the server, and once again it works fine!

## Conclusion

Now I don't know about you, but I think repurposing a school computer as a Minecraft server is kinda poetic, and it also means one less computer that's still got some life in it is being thrown out. What started off as me asking one of my friends "here, do you reckon they would let me take that computer" resulted in what will hopefully be money saved in the long run, as in the past we've had to spend £6 a month to host a server, whereas now the only cost is the electricity bill. Let's just hope it's not costing more than that to keep this thing running.

![The computer](/assets/posts/schoolcomputer/PXL_20220517_224209515.jpg)
*The computer's new home, also serving as a pretty good way to prop up my second monitor amidst the chaos of my desk. I have no idea why the plant is flopping over but I have since propped it up with a harmonica.*

## Future Plans

Here are some things I want to set up/try with this computer

- Develop a system where the computer shuts down if nobody has been online for a while, and when somebody tries to connect again, use Wake-On-Lan to bring it back online. This will hopefully save some on the electricity bill.
- Run some mods, to see if I can push this thing to its limit.
- Use it as a server for some other things, such as Factorio.
- Run Minecraft, with shaders.