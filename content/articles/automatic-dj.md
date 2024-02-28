---
title: "Automatic DJ Project"
date: 2019-03-24
draft: false
---

[This project](https://github.com/lucaspauker/auto-mixer) done for my CS106X class mixes two given mp3 files together and has a visualization to go with it.

## Introduction
I have always been awestruck by DJing and have had little idea of how it is done. I set out to create a python script to mix songs to understand the process a bit better. I first found that DJs use many effects in their mixes. These effects include changing tempo of songs, changing the pitch, as well as adding noises from other tracks.

For my project, I focused on three effects: _repeat_, _fade_, and _speed_. The _repeat_ effects repeats a portion of the audio segment some number of times. The _fade_ effect fades the audio by some amount of decibels. Lastly, the _speed_ command allows the user to speed up or slow down the song.

The general flow of mixing the songs was this:

1. Normalize tempo of the songs
2. Add drum file at the same tempo as the songs
3. Add effects according to a user-created effects file
4. Add a GUI that is syncronized with the song

## Libraries Used
In order to accomplish the above, I leveraged many python libraries.

- _[Aubio](https://github.com/aubio/aubio)_: Aubio is a library that helps with analysis of sound files. I used aubio to get the tempo of songs and to detect beats.
- _[Pydub](https://github.com/jiaaro/pydub/)_: Pydub is a great library for audio manipulation. I used pydub to apply audio effects.
- _[Pygame](https://github.com/pygame/pygame)_: Pygame is a library that allows you to create graphics and games. I used pygame to create the graphic to go along with the audio.

## Challenges
1. Synchronization of songs
My first pass at a solution for normalizing the tempo of the songs included calculating the bpm of both songs, then speeding one up to match. This was not a successful approach, however, since the bpm of songs is not constant throughout due to tempo changes as well as speeding up or slowing down. To solve this problem, I built a system that iterated through all the beats in the songs and made sure they were lined up properly. By adjusting the fidelity of how often to assert that the beats were aligned with the drums, I could line the beats up more accurately. However, this took much more time, creating a tradeoff between speed and accuracy.
2. Coming up with a way to mix songs
Finding a good approach to mixing songs was difficult, as I had little prior experience. I settled on two ways of mixing: overlaying, and adding custom effects.
3. Customizing drum track
I overlaid a custom drum track made using csound. Making this drum track flexible with the needs of my project was difficult and required becoming familiar with csound.

## Extensions
- Adding more effects
  - Adding effects such as added noises and changing pitch would present interesting technical challenges.
- Random element
  - I am curious to see how randomizing effects would sound.
  - There is a possibility for using machine learning for a computer to make dj mixes that sound good.
- Working with audio stream
  - An interesting capability would be for a program to be able to edit an audio stream on the fly.
  - This could be used to mix live music.

