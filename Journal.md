# Hi-Fi Audio Player

## What am I making?
I'm making an audio devboard that has the capability to playback high quality audio formats like FLAC and WAV. It has a dedicated audio decoding chip and has a separate low power M0 MCU to control everything.

# Total Time Spent: 16 Hours

# 26 July - Initial Research

So my original goal was to create a portable battery powered audio player but seeing as I don't have enough time I decided to make a prototype development board instead.
I did a bit of research on audio decoding chips, my main goal was for them to be able to stream high bitrate flac files. 
VLSI solution had a lot of good chips for my purpose but unfortunately most of them were out of stock on lcsc. They had a really nice all in one SoC which had SD card interface and everything in built but it was not in stock on lcsc and buying from VLSI's site directly was super expensive.
I decided to stick with VS1053B as it seemed like a good option for what I need and it could also be replaced later with a newer drop in replacement.

Regarding the MCU i just went with the STM32L072 which is a low powered MCU that supports USB 2.0 FS, has 20KB RAM and enough flash for my use case.

## Time Spent: 3 Hours

# 28 July - System Layout

My goal is to make a prototype board for this and then maybe scale in down in the later versions, so for this prototype the main goal is maximize power efficiency. 
For this specific reason I went with switching buck-boost converter instead of a LDO, even though and LDO is less noisy I feel like it wastes a lot of my capacity if powering of a battery.

I decided to go with the TPS63900 as its very efficent (90% efficiency) and has a really low Iq of just 75nA.

I also started the schematic for the project.

![img](https://hc-cdn.hel1.your-objectstorage.com/s/v3/c4ba2df711e81e636ae78556154f4a8a98f5077c_20250731_210844.jpg)

![img](https://hc-cdn.hel1.your-objectstorage.com/s/v3/6032fe583b139caa401920e12a1c57cc72d7a399_image.png)

## Time Spent: 5 Hours

# 29 July - PCB Design

I quickly realized that fitting everything on a small Ipod sized board would be problematic.
I decided to go with a raspberry pico sized board for the layout. This however soon also became a problem for reasons I'll discuss later.


![img](https://hc-cdn.hel1.your-objectstorage.com/s/v3/c512baa8132ba258b6b58868bd7aeee14d886fac_image.png)

![img](https://hc-cdn.hel1.your-objectstorage.com/s/v3/6032fe583b139caa401920e12a1c57cc72d7a399_image.png)

My schematic and PCB so far


## Time Spent: 2 Hours
# 30 July - Battery Efficiency

So it turns out that the VS1053B actually does not have an internal voltage regulator and instead needs 1.8v for its core voltage and 3v3 for analog and digital parts of its circuits. 
The simple fix for this is well uhh.. another BUCK CONVERTER :skull:.

I just used another buck convert to step down to 1.8 v from 5v and yeah thats about it, also adding a ferrite bead on the analog vdd line to suppress noise.


Now the main problem that I had with implementing everything on a raspberry pico layout is something called mixed signal design. My board has mixed signals (analog and digital), and noisy switching regulators, now what I need is a lot of physical separation between them in order to make sure I don't get any interference or noise. For this reason I decided to go with a bigger board and split my design like this.


![img](https://hc-cdn.hel1.your-objectstorage.com/s/v3/777803a7ec96305a983a82e83baa9eb9c37f3a76_image.png)


This was my final (messy ahh) schematic for this board
![img](https://hc-cdn.hel1.your-objectstorage.com/s/v3/6e9070ff9d3d123bf703e327c656b994ccb89c9d_image.png)


## Time Spent: 6 Hours