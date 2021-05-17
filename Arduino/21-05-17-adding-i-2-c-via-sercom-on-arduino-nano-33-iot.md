---
title: Adding I2C via SERCOM on Arduino Nano 33 IOT
date: 'Mon, 17 May 2021 15:03:34 +0200'
stacks:
    - Arduino
---

# Adding I2C via SERCOM on Arduino Nano 33 IOT

Arduino Nano 33 IOT is based on Atmel SAMD21G microcontroller.
This mcu manages some perpherials. Check the official pinout:
![Pinout-NANO33IoT_latest](../.geeks-diary/assets/Pinout-NANO33IoT_latest.png)

This board has an IMU and a cyptochip onboard attached to I2C on pins A4-A5. During a project development I found a sort of conflict, or simply a strange behaviour, by connecting a TCA9548a and 5 MPU6050.

It is possible to add more peripherials to SAMD21 of Arduino Nano 33 by using SERCOM, so as a possible solution you can add a custom hardware I2C using other pins.

In *variants.cpp* there are few tables about how it can be configured.

***SERCOM0*** and ***SERCOM1*** are general purpose and fully customizable by the user for Arduino Nano 33 IOT!!!

Thanks to @ubidefeo and @facchinm (their repos are awesome), I managed to use a secondary I2C over pins D6 and D5!


```cpp
#include <Wire.h>
#include "wiring_private.h"

TwoWire myWire(&sercom0, 6, 5);

void setup(){
	pinPeripheral(6, PIO_SERCOM_ALT); //SDA
	pinPeripheral(5, PIO_SERCOM_ALT); //SCL
    myWire.begin();
    
    //insert your code using myWire instead of Wire
}

void loop(){
    //insert your code
}

extern "C" {
	void SERCOM0_Handler(void) {
  		myWire.onService();
	}
}
```


**NOTE:** It is useful to put a 4k7 pull-up on the I2C pins. The pull-up must be on 3V3.

It is also possible to add other peripherials by checking sercoms and pads, please note the arduino official guide:
[https://www.arduino.cc/en/Tutorial/SamdSercom](https://www.arduino.cc/en/Tutorial/SamdSercom)

If you want to learn more or buy an Arduino Nano 33 IOT, check the official page:
[https://store.arduino.cc/arduino-nano-33-iot](https://store.arduino.cc/arduino-nano-33-iot)

Datasheet of the Arduino Nano 33 IOT mcu:
[https://www.microchip.com/wwwproducts/en/ATsamd21g18](https://www.microchip.com/wwwproducts/en/ATsamd21g18)






