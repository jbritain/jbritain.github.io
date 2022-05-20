---
title: Turning an Old School Computer Into a Minecraft Server - Part 2
layout: post
author: Joshua Britain
---

Since my previous post, I've made a few improvements to the setup for my Minecraft server hosted on a computer that formerly belonged to my school (if you haven't read [part 1]({% post_url 2022-05-01-turning-a-school-computer-into-a-minecraft-server %}), I suggest you do that first), and I have decided to document them, as much for myself (in case I want to do something similar in the future) as for anyone else.

## Automatically starting and stopping the server every day
### Bootup

The first thing I wanted to be able to do was have my server automatically start itself up every morning. I originally had plans to do this with a Raspberry Pi and WOL, but I discovered a much easier method - BIOS Power-On. Following some instructions on [HP's website](https://support.hp.com/gb-en/document/c04040117), I was able to change a setting in the BIOS that allowed the computer to automatically turn on at a certain time every morning. I initially had some issues while testing it where it flat out wouldn't work, it turned out the cause of the problem was that the system clock was set wrong. Doh!

### Starting the Minecraft server on boot

The next item in my list was having the Minecraft server start once the computer was booted, and I did this with `cron` and `screen`.

`cron` is a utility for running pre-scheduled commands, and by running `crontab -e`, I could generate a file that allowed me to add these commands. The contents of my crontab are pretty simple.


```
# crontab

@reboot /home/joshua/startServerScreen.sh
```

This launches a script called `startServerScreen.sh` when the computer boots up.

```
# startServerScreen.sh

cd /home/joshua
screen -S minecraft -d -m ./startServer.sh
```

Which in turn launches `startServer.sh`...

```
# startServer.sh

cd minecraft
java -Xmx2048m -jar server.jar
```

If you're wondering why I did this in multiple scripts, it was the easiest way I could find to do it.

### Stopping the server and shutting down

In order to shut down the server, I use `cron` again, but this time the commands are run as root, so I do `sudo crontab -e` to edit a crontab as root. In this crontab, I have two commands set

```
# crontab (as root)

30 23 * * * /home/joshua/stopServer.sh # stop minecraft server at 11:30 PM
40 23 * * * /usr/sbin/shutdown now # give it 10 minutes to backup the world in case it's quite large, then shutdown at 11:40 PM
```

And in `stopServer.sh`...

```
# stopServer.sh

pkill java # kill the Minecraft server
```

As a precaution, I have the [Textile Backup](https://www.curseforge.com/minecraft/mc-mods/textile-backup) mod installed, which backs the server up every hour and when the computer shuts down.

## Creating a console to manage the server remotely.

Now an issue we ran into was that sometimes the server just wouldn't let anyone join. Checking the logs, it would be complaining about authentication servers. Usually this could be solved by restarting the server, but I wasn't always around to SSH in and restart it, and I didn't feel like opening port 22 to the internet (I did try this briefly and immediately got bombarded with failed login attempts). The solution was to write a little server console web app, so that (with a password) my friends could restart the server at the push of a button. Here's what I came up with - it's pretty boring but does the job.

![The server console I created](/assets/posts/schoolcomputerpart2/console.png)

'Pericolo Di Caduta' is Italian for 'danger of falling', it's an inside joke based on the game 'Getting Over It with Bennett Foddy'. None of us are Italian.

If you want to see the code for the console, it's not very interesting, but you can find the code [here](https://github.com/jbritain/pericolo-di-caduta).

One other thing to note is I couldn't get node.js to broadcast on port 80 without running it as root, so I updated my root crontab to this: 

```
# crontab (as root)

@reboot iptables -t nat -I PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080

30 23 * * * /home/joshua/stopServer.sh # stop minecraft server at 11:30 PM
40 23 * * * /usr/sbin/shutdown now # give it 10 minutes to backup the world in case it's quite large, then shutdown at 11:40 PM
```

Don't ask me how it works because I just copied it from a forum somewhere.

Finally, to start my web app on startup, I changed my non-root crontab to 

```
# crontab

@reboot /home/joshua/startServerScreen.sh; /home/joshua/startNodeScreen.sh
```

And of course

```
# startNodeScreen.sh

screen -S node -d -m ./startNode.sh
```
```
# startNode.sh

cd serverConsole
npm start
```

And that's it! I hope this might be useful to anyone else setting up their own home Minecraft server, or if not at least interesting. Thanks for reading!