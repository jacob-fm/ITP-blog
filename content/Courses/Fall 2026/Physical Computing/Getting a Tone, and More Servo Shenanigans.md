---
draft: false
tags:
  - physicalComputing
  - Arduino
---

## Servo

First, I wanted to test the input range of the force-sensing resistor:

![[IMG_7020.gif]]

Then I modified the script so that I could more easily see the maximum and minimum values:

```C
int minim = 900;
int maxim = 0;
void setup() {
	Serial.begin(9600); // initialize serial communications
}
void loop () {
	int analogValue = analogRead(AO); // read the analog input
	if (analogValue > maxim) {
		maxim = analogValue;
		Serial.println("max: " + maxim);
	} else if (analogValue < minim) {
		minim = analogValue;
		Serial.println("min: " + minim);
	}
}
```

From there, it was pretty easy to build on what I had learned before in [[First Explorations]] to map the input from the FSR to the servo's position:

![[IMG_7021.gif]]

## Tone

At first I thought I wasn't getting any output from the speaker, but then realized I was just using too high of a resistor.

After getting a tone, I realized it would be pretty easy to make an array of frequencies to loop through, which I did to get an arpeggiating Am chord.

Then, I had ChatGPT make a function to convert a wide range of note names to their associated frequencies. This then allows me to define an array of notes to play, which gives a full song:

```C
#include <math.h>
const int duration = 200;

int noteIndex = 0;

// can't help falling in love
char *song[] = {
  "D3",
  "A3",
  "D4",
  "F#4",
  "D4",
  "A3",
  "A2",
  "E3",
  "A3",
  "C#4",
  "A3",
  "E3",
  "D3",
  "A3",
  "D4",
  "F#4",
  "D4",
  "A3",
  "A2",
  "E3",
  "A3",
  "C#4",
  "A3",
  "E3",
  "D4",
  "A2",
  "D3",
  "F#3",
  "D3",
  "A2",
  "A4",
  "C#3",
  "F#3",
  "A3",
  "F#3",
  "C#3",
  "D4",
  "F#3",
  "B3",
  "D4",
  "B3",
  "F#3",
  "B2",
  "F#3",
  "B3",
  "E4",
  "B3",
  "F#4",
  "G4",
  "D3",
  "G3",
  "B3",
  "G3",
  "D3",
  "F#4",
  "A2",
  "D3",
  "F#3",
  "D3",
  "A2",
  "E4",
  "E3",
  "A3",
  "C#4",
  "A3",
  "E3",
  "A2",
  "E3",
  "A3",
  "C#4",
  "A3",
  "A3",
  "B3",
  "D3",
  "G3",
  "B3",
  "G3",
  "D3",
  "C#4",
  "E3",
  "A3",
  "C#4",
  "A3",
  "E3",
  "D4",
  "F#3",
  "B3",
  "D4",
  "B3",
  "F#3",
  "E4",
  "D3",
  "F#4",
  "B3",
  "G4",
  "F#4",
  "A2",
  "D3",
  "F#3",
  "D3",
  "A2",
  "E4",
  "E3",
  "A3",
  "C#4",
  "A3",
  "E3",
  "D4",
  "A2",
  "D3",
  "F#3",
  "D3",
  "A2",
  "D2"
};

// mary had a little lamb
// char *song[] = {
//   "E4",
//   "D4",
//   "C4",
//   "D4",
//   "E4",
//   "E4",
//   "E4",
//   "REST",
//   "D4",
//   "D4",
//   "D4",
//   "REST",
//   "E4",
//   "G4",
//   "G4",
//   "REST",
//   "E4",
//   "D4",
//   "C4",
//   "D4",
//   "E4",
//   "E4",
//   "E4",
//   "E4",
//   "D4",
//   "D4",
//   "E4",
//   "D4",
//   "C4",
//   "REST",
//   "REST",
//   "REST",
// };

int songLength = sizeof(song) / sizeof(song[0]);

double note_to_frequency(const char *note) {
  static const char *note_names[] = {
    "C", "C#", "D", "D#", "E", "F",
    "F#", "G", "G#", "A", "A#", "B"
  };

  int note_number = -1;
  int octave;
  char name[3];

  /* Extract note name and octave */
  if (note[1] == '#') {
    name[0] = note[0];
    name[1] = '#';
    name[2] = '\0';
    octave = note[2] - '0';
  } else {
    name[0] = note[0];
    name[1] = '\0';
    octave = note[1] - '0';
  }

  /* Find note within octave */
  for (int i = 0; i < 12; i++) {
    if (strcmp(name, note_names[i]) == 0) {
      note_number = i;
      break;
    }
  }

  if (note_number < 0)
    return -1.0;

  int midi = (octave + 1) * 12 + note_number;

  /* A4 = 440 Hz */
  return 440.0 * pow(2.0, (midi - 69) / 12.0);
}

void setup() {
}

void loop() {
  if (strcmp(song[noteIndex], "REST") == 0) {
    noTone(8);
  } else {
    tone(8, note_to_frequency(song[noteIndex]), duration);
  }
  incrementNote();
  delay(duration);
}

void incrementNote() {
  if (noteIndex < songLength - 1) {
    noteIndex++;
  } else {
    noteIndex = 0;
  }
}

```

Here it is in action, playing *Can't Help Falling in Love*:

![[arduino-tone.mp4]]
