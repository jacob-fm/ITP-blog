---
draft: false
type: project
---
#gameEngines #audio #interactivity

The following was originally written on [Emmanuel's Blog](https://app.notion.com/p/Hypercinema-Grocery-Audio-Adventure-3de94ac518d7803ea25df1fe8384dce6) for the group (Me, Emmanuel, Zerui, and Forward).

My part in the project was everything that happened in #Godot, plus some of the audio recording.

[Releases · jacob-fm/grocery-audio-adventure](https://github.com/jacob-fm/grocery-audio-adventure/releases)

Playable Game Files

For **Grocery Audio Adventure**, we wanted to take something extremely familiar, like a supermarket, and make it feel more sensory and strange. Instead of recognizing products through packaging, labels, colors, or brands, the player walks through the space and discovers objects through sound. Each closed box represents a different food, and when the player interacts with it, they hear a sound of someone either preparing, eating, or simply shaking the food.

The audio was recorded with groceries we had laying around the house or in ITP’s kitchen. The 3D models were partially created with Blender, though some were downloaded from Sketchfab. The game was put together in Godot.

## Concept and Ideation

During brainstorming, we quickly latched onto the idea of an audio-reactive shopping experience. We toyed with a few variations on this theme. One idea was to have each item play a sound corresponding to how much it costs. We thought there might be some interesting conceptual ground there, but couldn’t think of a way to do it that would be engaging for a player/participant.

We thought about having each item play the sounds of how it arrived, e.g. the sound of an orange being picked from an orchard, then shipped in a truck, etc. This was compelling conceptually as a way to nudge consumers to think more about all the work that goes into making groceries as accessible as they are in much of the US. However, it was impractical given our time limit and that we live in New York City.

What we could do practically was to record the sounds of us interacting with different foods, which on its own isn’t so interesting, but by obscuring what each item actually is, we hoped to create intrigue for players, and to get them to be curious about what is inside each box.

## Technical Details

### Importing Models

We used Blender to create and adjust 3D models of the store, counter, and boxes (and several types of shelving units which did not make it into the game). When we first imported the models into Godot, the market model had some duplicated textures which caused strange flickering. So we found a better free model online and went with that.

![[SCR-20260922-nsln-2.jpeg]]

Our original problematic supermarket

### **First-person movement and controls**

We modified a player controller from [Brackeys](https://brackeys.com/), adding an input for interacting with objects. We wanted it to be clear to players which object they were set up to interact with, so a raycast is shot from the center of the screen, and if the first object it hits is a `scannable_item`, that object is set as the highlighted object, and its regular material is replaced with an emissive red material. ChatGPT was helpful in writing up this script.

![[item_highlight.gif]]

Each `scannable_item` has its own [3D Audio Player](https://docs.godotengine.org/en/stable/classes/class_audiostreamplayer3d.html) attached, so the audio is spatial: if you start the audio on a box closer to you, it will play louder than from a box further away, and panning the camera right or left will adjust the stereo signal accordingly.

## Hypothetical Next Steps

There are a few directions this project could go.

1. A full-fledged game, with objectives (like shopping for a certain recipe), improved models and graphics, UI, etc.
2. A physical installation, where we set up a fake grocery store with boxes
    1. Each box would have a barcode, that when scanned with a custom barcode scanner, would cause a different sound to play (either from the scanner or from speakers)
3. An audio journey in an existing grocery store, where we use the barcodes on the existing products in the store to trigger different sounds
    1. Sounds would likely play from the participants’ phones through a web app
    2. The tricky part would be figuring out which products to include as part of the journey, and how to guide the participants to the correct products
