---
draft: true
tags:
  - physicalComputing
  - Arduino
  - p5
type: lab
---

https://itp.nyu.edu/physcomp/labs/labs-serial-communication/lab-webserial-input-to-p5-js/

Since I have whatever disease it is that makes someone use Neovim for everything, I periodically run into issues in this course relating to issues with the [Arduino-Nvim library](https://github.com/yuukiflow/Arduino-Nvim). I also insist on doing my p5 sketches with [p5.nvim](https://github.com/prjctimg/p5.nvim/blob/main/README.md). So of course as I started this lab, I ran into a number of issues, and it was hard to tell which were caused by what. Claude helped me out though.

One example was that I had followed the lab's instructions to add `<script src="https://unpkg.com/p5-webserial@0.1.1/build/p5.webserial.js"></script>` to `index.html`, but I had also installed the webSerial library through p5.nvim.

copying and pasting this from the lab for future reference:

## p5.WebSerial Sketch Checklist

- In the HTML file, include the p5.webserial library
- In the global variables of the sketch,
    - make a new instance of the library
    - include a port selector button or some way to invoke the serial port chooser dialogue box
- In the setup:
    - Make sure WebSerial is supported in this browser
    - Include a call to `serial.getPorts()` to check for available ports.
    - include `serial.on()` listeners for these events:
        - noport
        - portavailable
        - data
        - close
        - requesterror
    - include navigator listeners for connect and disconnect
- Define handler functions for all of the events above. Most of these can be simple alerts or console.log messages
- Customize the function that responds to the `data` listener (usually called `serialEvent()` in these examples), as you’ll see below.
- Decide when and how you’ll send serial data out, as you’ll see in the other p5.webserial labs.

## Getting data into p5

Was able to get here with low friction:

![[SCR-20261109-lsyo.png]]

## Graphing data over time

![[Screen Recording 2026-11-09 at 1.13.07 PM 1.mov]]

(the blips where it goes to 0 are because I accidentally removed the potentiometer from the breadboard briefly)
