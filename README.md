# Table Of Contents
* [Table of Contents](#TableOfContents)
* [Hello_CircuitPython](#Hello_CircuitPython)
* [CircuitPython_Servo](#CircuitPython_Servo)
* [CircuitPython_LCD](#CircuitPython_LCD)
## Hello_CircuitPython:
### Description
This assignment started me on Circuit Python and was designed to teach me about changing an on board LED
### Code
```python
import board
import neopixel
import time 
import math

Kaz = neopixel.NeoPixel(board.NEOPIXEL, 1)
Kaz.brightness = 0.1 
print("Make it red!")
while True:
    Kaz.fill((255,0,0))
```
### Evidence
#### Functioning Video
![Kaz's Video of The LED blink Assignment](https://github.com/kshinoz98/CircuitPython/raw/master/Untitled_%20Sep%2027,%202022%203_18%20PM.gif?raw=true)
#### Wiring Diagram
![Mr. Helmstetter's Wiring Diagram Of LED blink assignment](https://user-images.githubusercontent.com/54641488/192549584-18285130-2e3b-4631-8005-0792c2942f73.gif)
## CircuitPython_LED_Fade
### Description
This assignment teaches you how to use the on board LED light, it also teaches the basics of the map function
### Code
```python
# SPDX-FileCopyrightText: 2021 ladyada for Adafruit Industries
# SPDX-License-Identifier: MIT
import digitalio
import simpleio
import time
import board
import adafruit_hcsr04
import neopixel                       
from board import *

sonar = adafruit_hcsr04.HCSR04(trigger_pin=board.D3, echo_pin=board.D2)
Kaz = neopixel.NeoPixel(board.NEOPIXEL, 1)#connecting the neopixel on the board to the code
Kaz.brightness = 0.1 #setting the brightness of the light, from 0-1 brightness
KazOutput = 0
Red = 0
Green = 0
Blue = 0

while True:
    try:
        cm = sonar.distance
        print((sonar.distance, Red, Green, Blue))
        time.sleep(0.01)
        if cm < 5:
            Blue = 0
            Green = 0
            Kaz.fill((255, 0, 0))#setting the color with RGB values
        elif cm > 5 and cm < 20:
            Green = 0
            Red = simpleio.map_range(cm, 5.1, 20, 255, 0)
            Blue = simpleio.map_range(Red, 0, 255, 255, 0)
            Kaz.fill((Red, Green, Blue))
        else:
            Blue = simpleio.map_range(cm, 20.1, 50, 255, 0)
            Green = simpleio.map_range(Blue, 0, 255, 255, 0)
            Kaz.fill((0, Green, Blue))#setting the color with RGB values
    except RuntimeError:
        print("Retrying!")
    time.sleep(0.01)
```
### Evidence
#### Functioning Video
![Kaz's LED Fade Video](https://github.com/kshinoz98/CircuitPython/raw/master/ezgifgif.gif?raw=true)
#### Wiring Diagram
![Kaz's Wiring Diagram of LED Fade](https://raw.githubusercontent.com/kshinoz98/CircuitPython/f4be6df7eb8828500e94754d2ccb5b5c8cd2b276/Screenshot%202022-09-19%20154243.png)
### Reflection
When making the ranges, my basic knowlege of colors didn't help me figure out values, what did was an RGB calculator on the internet. Use an RGB calculator to help you visuaize your map fuction. In the beginning I did not use very efficient names for my variables. My "blue" value was called "green". Be better than me.
## CircuitPython_Servo:
### Description
The assignment is teaching the student the basics of how to use a servo with interval degrees
### Code
```python
# SPDX-FileCopyrightText: 2018 Kattni Rembor for Adafruit Industries
#
# SPDX-License-Identifier: MIT

"""CircuitPython Essentials Servo standard servo example"""
from digitalio import DigitalInOut, Direction, Pull
import time
import neopixel
import board
import pwmio
from adafruit_motor import servo
btn = DigitalInOut(board.D3)
btn2 = DigitalInOut(board.D2)
btn.direction = Direction.INPUT
btn.pull = Pull.UP
btn2.direction = Direction.INPUT
btn2.pull = Pull.UP
# create a PWMOut object on Pin A2.
pwm = pwmio.PWMOut(board.D7, duty_cycle=2 ** 15, frequency=50)
# Create a servo object, my_servo.
my_servo = servo.Servo(pwm)

while True:
    if btn == True:
        time.sleep(1)
        for angle in range(0, 180, 90):  # 0 - 180 degrees, 100 degrees at a time.
            my_servo.angle = angle
    elif btn2 == True:
        time.sleep(1)
        for angle in range(180, 0, -90): # 180 - 0 degrees, 100 degrees at a time.
            my_servo.angle = angle
    else:
        print("click a button man")
```
### Evidence:

#### Funcioning Video
![Sero_Spinning](https://github.com/kshinoz98/CircuitPython/raw/master/Untitled_%20Sep%2029,%202022%203_40%20PM.gif?raw=true)
#### Wiring Diagram
![Kaz's Servo Wiring](https://github.com/kshinoz98/CircuitPython/blob/master/Screen%20of%20servo%20wiring.png?raw=true)
### Reflection
Dont forget to add delay to your servo. I forgot and a servo sparked out. Dont forgot to make sure your intervol is within the max degrees. It doesnt have to multiply into 180, but it does have to be within 180.
## CircuitPython_LCD
### Description 
Tripping one of the inputs will cause your Metro to count and that count will be displayed on the LCD. Touching the other input should toggle whether your Metro is counting up or down. The count direction is also be displayed on the LCD.
### Code
```python
import board
import time
from lcd.lcd import LCD
from lcd.i2c_pcf8574_interface import I2CPCF8574Interface
from digitalio import DigitalInOut, Direction, Pull

# get and i2c object
i2c = board.I2C()
btn = DigitalInOut(board.D3)
btn2 = DigitalInOut(board.D2)
btn.direction = Direction.INPUT
btn.pull = Pull.UP
btn2.direction = Direction.INPUT
btn2.pull = Pull.UP
# some LCDs are 0x3f... some are 0x27.
lcd = LCD(I2CPCF8574Interface(i2c, 0x3f), num_rows=2, num_cols=16)
cur_state = True
prev_state = True
cur_state2 = True
prev_state2 = True
cur_state3 = True
prev_state3 = True
buttonPress = 0
buttonToggle = True

while True:
        cur_state3 = btn2.value
        prev_state3 = cur_state3
        if cur_state3 == prev_state3:
            cur_state = btn.value
            if cur_state != prev_state:
                if not cur_state:
                    buttonPress = buttonPress + 1
                    lcd.clear()
                    lcd.set_cursor_pos(0,0)
                    lcd.print(str(buttonPress))
                else:
                    lcd.clear()
                    lcd.set_cursor_pos(0,0)
                    lcd.print(str(buttonPress))
            prev_state = cur_state
        else:
            cur_state2 = btn.value
            if cur_state2 != prev_state2:
                if not cur_state2:
                    buttonPress = buttonPress - 1
                    lcd.clear()
                    lcd.set_cursor_pos(0,0)
                    lcd.print(str(buttonPress))
                else:
                    lcd.clear()
                    lcd.set_cursor_pos(0,0)
                    lcd.print(str(buttonPress))
            prev_state2 = cur_state2
```
### Evidence
#### Functioning Video
![Kaz's Functioning Video for LCD](https://github.com/kshinoz98/CircuitPython/blob/master/ezgif-2.gif?raw=true)
#### Wiring Diagram
![Kaz's Wiring Diagram For LCD](https://raw.githubusercontent.com/kshinoz98/CircuitPython/b45fed4ddee888d03481fca24c670a8d5ac0b01c/Screenshot%202022-09-27%20144318.png)
### Reflection
When I was doing this assignment my second input, the one that toggles the counting, was set as a integer and not a boolean. Set it as a boolean and that will help it toggle. Don't spend your time on debounce, its not worth it.
