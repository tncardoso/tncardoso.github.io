---
layout: post
title: Gaming for the lazy
tags: [arduino]
---

<div class="centered">
<img src="{{ "img/gaming/sonic.png" | prepend: site.baseurl }}"
width="120px" height="85px" class="post-icon">
</div>

I come from a time when gaming was serious business. You could not beat
a game without hours of continuous effort. There was no such thing as
saving your progress and the only difficulty level was 'Hard'.

Once, as a kid, I was playing Sonic when someone accidentally kicked the
videogame, making the screen go black. After moments of tension, images
suddenly returned, surprisingly, in the final stage of the game.
Everyone claimed that the the more experienced player should finish it,
but I didn't yield: it was my chance to shine. Of course, I would have
made a different decision if I knew I would lose in the first pit.

Fast forward some years, during a clean up, the old Sega Master System
emerged and it was fully functional. Between the old cartridges, there
was Sonic. I instantly got curious if there were other ways to finish it
and decided to connect my computer to the video-game via an arduino
board. That would enable me to record and replay game input, just like a
save-state system.

## Arduino for the rescue

Master System controller has 8 pins: 6 for each possible input, one for
ground and one for VCC. Input pins output a very small voltage when
idle. When a button is pressed, the wire is grounded, resulting in an
action. This simple protocol may be implemented using an Arduino by
connecting each action wire to an output gate and controlling these
outputs via serial port using a computer. Also, resistors are used for
each connection in order to protect the board and reduce the voltage
used for keeping inputs idle.

<div class="centered">
<a href="{{ "img/gaming/board.jpg" | prepend: site.baseurl }}"
target="_blank">
<img src="{{ "img/gaming/board.jpg" | prepend: site.baseurl }}"
width="320px" height="240px" class="post-image">
</a>
</div>

## Controller

A very simple protocol was implemented for the Arduino. Bytes are
constantly read from the serial connection, and for each possible value
an action is taken. For example, the value 0x01 activates the left
arrow, while the value 0x02 deactivates it.

A python client was developed for communicating with the arduino and
after some tweaking with sleep times (nanosecond measures were
necessary) the client was ready for use. This client allows not only for
live playing but also for recording and replaying sessions. Because
games used to be deterministic on those days --  events usually happened
at the same time, and enemies followed the same path --, replaying a
previously recorded session leads to exactly the same outcome.

In the following video you can see a playback of Sonic's first stage,
fully controlled by Arduino.

<div class="post-image">
<iframe width="560" height="315"
src="//www.youtube.com/embed/4H-_ebr436g" frameborder="0"
allowfullscreen></iframe>
</div>

## Now it's on you

If you want to dive into the details of Master Arduino Controller, just
check the source code available on <a
href="https://github.com/tncardoso/master-arduino-controller" target="_blank">
Github</a> under a three-clause BSD license and go for it.

