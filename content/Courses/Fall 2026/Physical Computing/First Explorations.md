---
draft: false
---

#physicalComputing

Read some articles and watched some videos to try and get a grasp on electricity basics this week. I didn't really have a physics class in high school or college so this is all pretty new.

I think I was able to get a basic grasp of Ohm's law. The idea that voltage is a measure of potential was and still is a bit confusing, but as I've started to actually mess around with different circuits, I think I'm starting to get the picture.

## Hands on

Went through a bunch of random labs on the pComp website.

1. Get a switch working at all
![[IMG_6899 (1).gif]]

2. Attempt to get switches working in parallel (fail)
![[IMG_6900.gif]]

3. Attempt to get switches working in parallel, somehow accidentally get them in series
![[IMG_6901.gif]]

I did eventually get it to work in parallel, but forgot to video that.

4. Went a little ahead and got the servo motor to respond to Arduino code
![[IMG_6905 (1).gif]]

Code for above (ChatGPT helped me get started):

```C
#include <Servo.h>

  

Servo myServo;

  

const int DELAY_TIME = 500;

const int INTERVAL = 30;

int direction = 1;

  

void setup() {

myServo.attach(9); // Yellow wire connected to digital pin 9

myServo.write(0); // Move to approximately center

}

  

void loop() {

int currPos = myServo.read();

Serial.println(currPos);

  

if (currPos >= 180) {

direction = -1;

} else if (currPos <= 0) {

direction = 1;

}

myServo.write(currPos + (INTERVAL * direction));

delay(DELAY_TIME);

}
```
