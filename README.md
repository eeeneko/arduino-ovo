# arduino-ovo
ovo.h aims to make uninterrupted main loop possible for your program on any arduino board.

## Functions Spec
---------------
### setTimeout()
#### Params:
 - **auto function** Your function. `Lambda Expression is allowed!`
 - **int delay** Time to delay in milliseconds.
 ---------------
### setInterval()
#### Params:
 - **auto function** Your function. `Lambda Expression is allowed!`
 - **int delay** Time for every interval in milliseconds.
 -----------------------
#### Arduino Official Blinker Example
````C++

// the setup function runs once when you press reset or power the board
void setup() {
  // initialize digital pin LED_BUILTIN as an output.
  pinMode(LED_BUILTIN, OUTPUT);
}

// the loop function runs over and over again forever
void loop() {
  digitalWrite(LED_BUILTIN, HIGH);   // turn the LED on (HIGH is the voltage level)
  delay(1000);                       // wait for a second
  digitalWrite(LED_BUILTIN, LOW);    // turn the LED off by making the voltage LOW
  delay(1000);                       // wait for a second
}
````
#### OvO Example
````C++
#include "ovo.h"

// the setup function runs once when you press reset or power the board
void setup() {
    // initialize digital pin LED_BUILTIN as an output.
    pinMode(LED_BUILTIN, OUTPUT);
}

// the loop function runs over and over again forever
void loop() {

    setTimeout([]{setInterval([]{digitalWrite(LED_BUILTIN,HIGH);}, 2000);},1000);
    setInterval([]{digitalWrite(LED_BUILTIN,LOW);}, 2000);
}
````
-------------------
### setSwitch()
This function help you switch function as planned in main loop.
#### Params:
 - **auto function1** Your first function. `Lambda Expression is allowed!`
 - **auto function2** Your second function. `Lambda Expression is allowed!`
 - **int delay1** Time for every interval in milliseconds for function1.
 - **int delay2** Time for every interval in milliseconds for function2.
#### Example for Blinker
````C++
#include "ovo.h"

// the setup function runs once when you press reset or power the board
void setup() {
    // initialize digital pin LED_BUILTIN as an output.
    pinMode(LED_BUILTIN, OUTPUT);
}

// the loop function runs over and over again forever
void loop() {

    setSwitch([]{digitalWrite(LED_BUILTIN,HIGH);}, []{digitalWrite(LED_BUILTIN,LOW);}, 1000, 1000);
}
````
-------------------
## Class slowWrite
Use this class to make a rough voltage change slower.
### slowWrite.set()
Use this function to set the pin and delay info.
#### Params:
 - **int pin** Here is your pin number.
 - **int delay** How slow you want a voltage change from 0 to 1 in milliseconds.
 -------------------------------------------------------
### slowWrite.run()
 - Put this in your main loop.
 -------------------------------------------------------
### slowWrite.high()
 - To change the pin voltage from 0 to 1.
----------------------
### slowWrite.low()
 - To change the pin voltage from 1 to 0.
----------------------
#### Example
````C++
#include "ovo.h"

slowWrite m; //Must before setup()

// the setup function runs once when you press reset or power the board
void setup() {
    // initialize digital pin LED_BUILTIN as an output.
    pinMode(LED_BUILTIN, OUTPUT);
}

// the loop function runs over and over again forever
void loop() {
    m.set(LED_BUILTIN, 2000); //The objected pin is LED_BUILTIN, every voltage change takes 2 sec

    //The led will on and off for every 3 sec
    setTimeout([]{setInterval([]{m.high();}, 6000);},3000);
    setInterval([]{m.low();}, 6000);
    
    m.run(); //This must be put here
}
````
--------------------------------
