# WEB SERVER BASED HOME AUTOMATION SYSTEM USING INTERNET OF THINGS


# 1. What is Home Automation?



Wireless Home Automation system(WHAS) using IoT is a system that uses computers or mobile devices to control basic home functions and features automatically through internet from anywhere around the world.

**Flow Chart Explaining Our Project**
<p align="center">
<img src="https://github.com/aniketmarwade/Home-Automation-2021-FInal-Year-Project/blob/main/IMAGES/diagrams/flow%20chart.jpg?raw=true"
  width="686" height="359">
</p>

**The Main Components Used in our Project**

* **Esp 32 Wroom model**
* **2 Bread Boards**
* **Capacitor 2.2uf 250v**
* **Capacitor 3.3uf 250v**
* **Resistor 200k ohms 1/4w**
* **Resistor 330 ohms**
* **8 Channel Relay**
* **Jumpers**
* **other electrical components.....**
* 


# CODE 
The code is divided into two parts they are with internet and without internet.

The Code Starts Here.

**Libraries Included**
```
#include <WiFi.h>
#include <WiFiClient.h>
#include <BlynkSimpleEsp32.h>
BlynkTimer timer;
```
**Input Pins Defined**
```
#define DEBUG_SW 1


// Pins of Fan Regulator Knob
#define s1  34
#define s2  35
#define s3  32
#define s4  33

// Pins of Switches
#define S5 18
#define S6 17
#define S7 16
#define S8 13
#define S9 14

// Pins of Relay (Appliances Control)
#define R5 23
#define R6 22
#define R7 21
#define R8 19
#define R9 5


// Pins of Relay (Fan Speed Control)
#define Speed1 25
#define Speed2 26
#define Speed4 27
```
**By default the mode is with_internet**
```
int MODE = 0;
```

**You should get Auth Token in the Blynk App.**
**Go to the Project Settings (nut icon).**
```
char auth[] = "1KI9pZktz5A9kTT3d7c0QslmIjowKmW0";
```







