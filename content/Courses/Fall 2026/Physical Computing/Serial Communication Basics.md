---
draft: false
tags:
  - Arduino
  - physicalComputing
type: lab
---

## Intro Serial Communications Lab

https://itp.nyu.edu/physcomp/lab-intro-to-serial-communications/

Double-checking I still know how to do the basics:

![[IMG_7241.jpg]]

```C
void setup() {
   // start serial port at 9600 bps:
   Serial.begin(9600);
}
 
void loop() {
   int analogValue = analogRead(A0);
   Serial.println(analogValue);
}
```

![[IMG_7242.jpg]]

Then tried to run this code to send data in multiple formats:

```C
void setup() {
   // start serial port at 9600 bps:
   Serial.begin(9600);
}
 
void loop() {
// read analog input, map it to make the range 0-255:
int analogValue = analogRead(A0);
int mappedValue = map(analogValue, 0, 1023, 0, 255);
Serial.println(mappedValue);
 
// print different formats:
Serial.write(mappedValue);  // Print the raw binary value
Serial.print('\t');             // print a tab
// print ASCII-encoded values:
Serial.print(mappedValue, BIN); // print ASCII-encoded binary value
Serial.print('\t');             // print a tab
Serial.print(mappedValue);      // print decimal value
Serial.print('\t');             // print a tab
Serial.print(mappedValue, HEX); // print hexadecimal value
Serial.print('\t');             // print a tab
Serial.print(mappedValue, OCT); // print octal value
Serial.println();               // print linefeed and carriage return
}
```

but ran into an issue where the port was busy:

![[SCR-20261109-jsxb.png]]

After talking to Claude, I learned that when following the lab's directions to run the `screen` command, and exit with ctrl-A -> ctrl-D, I had actually only detached from the process without killing it. In actuality, you need to run ctrl-A -> k, then press y to confirm. To reopen the detached process, I ran `screen -r`.

![[SCR-20261109-jutj.png]]

### Formatting Multiple Serial Data

Had an issue where the switch wasn't giving me the results I wanted. It was reading 0 constantly despite me pressing the button, though occasionally would briefly output 1 as I was fussing with the wires. Was having trouble figuring out the cause.

First, I realized that I had the switch plugged into A2 instead of D2. But fixing that still didn't fix the issue, though it did change the output from a constant 0 to a constant 1. I then asked Cody for help, and he helped me use a multimeter to test the connection between the different pins on the pushbutton, to find out I just had my wires connected on pins where the connection was constant and not affected by the button. Switching that fixed the issue.

## My code at the end of the lab for future reference

```C
const int switchPin = 2;
void setup() {
  // start serial port at 9600 bps:
  Serial.begin(9600);
  pinMode(switchPin, INPUT);
  while (Serial.available() <= 0) {
    Serial.println("hello");  // send a starting message
    delay(300);               // wait 1/3 second
  }
}

// void loop() {
//   if (Serial.available()) {
//     int sensor1Value = analogRead(A0);
//     Serial.print(sensor1Value);
//     Serial.print(",");
//     int sensor2Value = analogRead(A1);
//     Serial.print(sensor2Value);
//     Serial.print(",");
//     int switchValue = digitalRead(switchPin);
//     Serial.println(switchValue);
//   }
// }

void loop() {
  if (Serial.available()) {
    // read the incoming byte:
    int inByte = Serial.read();
    // read the sensor:
    int sensorValue = analogRead(A0);
    // print the results:
    Serial.print(sensorValue);
    Serial.print(",");

    // read the sensor:
    sensorValue = analogRead(A1);
    // print the results:
    Serial.print(sensorValue);
    Serial.print(",");

    // read the sensor:
    sensorValue = digitalRead(switchPin);
    // print the results:
    Serial.println(sensorValue);
  }
}

```
