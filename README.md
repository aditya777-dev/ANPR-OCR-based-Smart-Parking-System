# ANPR & OCR based Smart Parking System
This project includes a smart gate for parking lots using an IR sensor, esp32cam, servo motor and RFID reader. 
Additionally a webserver to authenticate a car using Automatic Number Plate Recognition and Optical Character Recognition technology.

# Step 1
refer to 

https://www.youtube.com/watch?v=0-4p_QgrdbE 

https://github.com/nicknochnack/TFODCourse

to setup ANPR on your device

# Step 2
setup hardware circuit

COMPONENTS: EM-18 RFID Reader, ESP32CAM, Servo Motor, IR Sensor, Arduino Uno (only for uploading esp32cam code)

connect power and ground of all components to a 5V 2A power source and ground respectively

ARDUINO UNO PINS:

RST -> ARDUINO UNO GND

TX -> ESP32CAM TXD

RX -> ESP32CAM RXD

ESP32CAM PINS:

GPIO0 -> ESP32CAM GND

TXD -> ARDUINO UNO TX

RXD -> ARDUINO UNO RX

GPIO12 -> IR sensor Digital Pin

GPIO13 -> EM-18 RFID reader TX Pin

GPIO14 -> Servo Motor Digital Pin

# Step 3
ensure Step 1 is completed

start your webserver by executing "ANPR-OCR-webserver.ipynb"

# Step 4
Load esp32cam config from "CameraWebServerServo.ino" file located in "CameraWebServerServo.zip" into esp32cam module

use ssid and password of the same wifi network connected to the webserver device in "CameraWebServerServo.ino"

use Arduino IDE and Arduino UNO to load esp32cam config into esp32cam module

use "Wrover Module" as board to upload code to esp32cam

on uploading code, to hard reset (only when mentioned in arduino ide console), disconnect GPIO0 and GND connection and press reset button on esp32cam

once upload is comlpete, disconnect TX, RX pins and seperate Arduino UNO from circuit

# Step 5
make sure your webserver device and esp32cam are connected to the same wifi network

test your entire curcuit for functionality

system flow:

1. car approaches gate, IR sensor placed in front of gate is triggered
2. this trigger signals esp32cam to send a /capture request to webserver
3. on receiving /capture request, webserver accesses the live stream provided by esp32cam on http://{esp32cam ip}:81/stream
4. ANPR is performed on the live stream to locate number plate
5. OCR is performed on located numberplate to extract text
6. extracted text is compared to existing authorized numberplates list
7. accordingly a /rotate90 or /unauthorized request is sent back to esp32cam to rotate servo 90 degrees or signal an unathorized vehicle respectively
