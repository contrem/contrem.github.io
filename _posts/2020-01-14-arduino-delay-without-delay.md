---
layout: post
title: Arduino Delay Without delay()
categories: [arduino]
---

Delaying actions in Arduino without using the [delay()](https://www.arduino.cc/reference/en/language/functions/time/delay/) function.

## Why not use delay()

Using [delay()](https://www.arduino.cc/reference/en/language/functions/time/delay/) stops the entire program. For simple programs that might be ok, but if you need to read sensors or buttons, using [delay()](https://www.arduino.cc/reference/en/language/functions/time/delay/) might cause you to miss events. Since [delay()](https://www.arduino.cc/reference/en/language/functions/time/delay/) stops the entire program, it is unable to react to any inputs.

## What's the alternative

Instead of using [delay()](https://www.arduino.cc/reference/en/language/functions/time/delay/) you should compute the desired pause duration using a time function like [millis()](https://www.arduino.cc/en/Reference/Millis). Record the time at the start point, and then continuously check to see if the desired duration has passed. [An example using this method is available on arduino.cc](https://www.arduino.cc/en/tutorial/BlinkWithoutDelay).

This method quickly gets ugly as soon as you need more than one delay duration. So instead, use the [arduino-timer library](https://github.com/contrem/arduino-timer).


## Blink without delay

The [blink example on arduino.cc](https://www.arduino.cc/en/tutorial/BlinkWithoutDelay) becomes trivial using the [arduino-timer library](https://github.com/contrem/arduino-timer/blob/master/README.md):

```c++
#include <timer.h>

auto timer = timer_create_default(); // create a timer with default settings

bool toggle_led(void *) {
  digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN)); // toggle the LED
  return true; // keep timer active? true
}

void setup() {
  pinMode(LED_BUILTIN, OUTPUT); // set LED pin to OUTPUT

  // call the toggle_led function every 1000 millis (1 second)
  timer.every(1000, toggle_led);
}

void loop() {
  timer.tick(); // tick the timer

  // other code goes here without worrying about being blocked by delay()
}
```

## More examples

There are [more examples](https://github.com/contrem/arduino-timer/tree/master/examples) available in the [arduino-timer github](https://github.com/contrem/arduino-timer), including blinking using microseconds and handling multiple delay durations.
