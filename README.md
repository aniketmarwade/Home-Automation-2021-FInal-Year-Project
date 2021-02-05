# WEB SERVER BASED HOME AUTOMATION SYSTEM USING INTERNET OF THINGS


# 1. What is Home Automation?

Wireless Home Automation system(WHAS) using IoT is a system that uses computers or mobile devices to control basic home functions and features automatically through internet from anywhere around the world.

With this you can control home appliances anywhere in the world from your phone as well as manually from switches with real-time feedback on your app with internet or without internet.


**Flow Chart Explaining Our Project**
<p align="center">
<img src="https://github.com/aniketmarwade/Home-Automation-2021-FInal-Year-Project/blob/main/IMAGES/diagrams/flow%20chart.jpg?raw=true"
  width="686" height="359">
</p>

# Components.

1. [ESP 32](https://www.amazon.in/SquadPixel-ESP-32-Bluetooth-Development-Board/dp/B071XP56LM/ref=sr_1_1?dchild=1&keywords=esp32&qid=1612376825&sr=8-1) / [Node MCU ESP8266](https://www.amazon.in/Lolin-NodeMCU-ESP8266-CP2102-Wireless/dp/B010O1G1ES/ref=sr_1_2?dchild=1&keywords=esp8266&qid=1612376986&sr=8-2)
2. [8 Channel Relay](https://www.amazon.in/JBTek-Channel-Control-Optocoupler-Arduino/dp/B00KTELP3I/ref=sr_1_1?crid=3BEE2K9I4CM3P&dchild=1&keywords=8+channel+relay+module&qid=1612377061&sprefix=8+channel+re%2Caps%2C194&sr=8-1)
3. [Breadboard Small](https://www.amazon.in/REES52-400-Point-Solderless-Breadboard/dp/B01IN0QGP6/ref=sr_1_2?crid=3J32PUARETXGC&dchild=1&keywords=small+breadboard&qid=1612377128&sprefix=small+brea%2Caps%2C381&sr=8-2)
4. [Breadboard Large](https://www.amazon.in/Generic-Elementz-Solderless-Piecesb-Circuit/dp/B00MC1CCZQ/ref=sr_1_4?crid=J9RLK5OJ4NKB&dchild=1&keywords=breadboard&qid=1612377224&sprefix=breadb%2Caps%2C643&sr=8-4)
5. [Jumper](https://www.amazon.in/Electrobot-Jumper-Wires-120-Pieces/dp/B071VQLQQQ/ref=sr_1_2?crid=OC4LYMDWO3JR&dchild=1&keywords=jumper+wires&qid=1612378597&sprefix=jumpr%2Caps%2C634&sr=8-2)
6. [Regulator Switch](https://www.amazon.in/pole-Wiring-Selector-Rotary-Switch/dp/B08PS32FCY/ref=sr_1_4?dchild=1&keywords=rotary+switch&pd_rd_r=a68bfd61-e901-4920-ac14-aa270e352c48&pd_rd_w=QPL4h&pd_rd_wg=FaYzp&pf_rd_p=1abe8808-d6bc-4840-858b-6acddb119a7a&pf_rd_r=YT4SZWE3WYXHPDEGZDRM&qid=1612378653&sr=8-4)
7. Prototype Board
8. 2.2ohm 1/2W Resistor
9. 220k ohm 1/4W Resistor
10. 3.3uF 250V Capacitor
11. 2.2uf 250V Capacitor
12. 10k ohm 1/4W Resistors



# CODE 
The code is divided into two main parts they are with internet and without internet with other parts that are used to complete the code.

- [Code](#code)
    - [Uploading Code](#uploading-code)
    - [ESP 32 Code](#esp32-code)
        - [Libraries Included](#libraries-included)
        - [Input-Output Pins Defined](#input-output-pins-defined)
        - [Mode](#mode)
        - [Authentication Token](#authentication-token)
        - [WiFi Credentials](#wifi-credentials)
        - [Flag Define](#flag-define)
        - [Assigning Incoming Values](#assigning-incoming-values-from-blynk-app-to-variables)
        - [Void Setup](#void-setup)
        - [WiFi Status](#wifi-status)
        - [With Internet Function](#with-internet-function)
        - [Without Internet Function](#without-internet-function)
        - [Fan Speed Control](#fan-speed-control)
        - [Check Blynk](#check-blynk)





#### Libraries Included

```
#include <WiFi.h>
#include <WiFiClient.h>
#include <BlynkSimpleEsp32.h>
BlynkTimer timer;
```
#### Input-Output Pins Defined

```
#define DEBUG_SW 1

// Pins for Fan Regulator Knob
#define s1  34 //IO34
#define s2  35 //IO35
#define s3  32 //IO32
#define s4  33 //IO33

// Pins for Switches
#define S5 18  //IO18
#define S6 17  //IO17
#define S7 16  //IO16
#define S8 13  //IO13
#define S9 14  //IO14

// Pins for Relay (Appliances Control)
#define R5 23  //IO23
#define R6 22  //IO22
#define R7 21  //IO21
#define R8 19  //IO19
#define R9 5   //IO5


// Pins for Relay (Fan Speed Control)
#define Speed1 25  //IO25
#define Speed2 26  //IO26
#define Speed4 27  //IO27
```


#### Mode 
(By default the mode is 0 with internet connected)
```
int MODE = 0;
```


#### Authentication Token
(You should get Auth Token in the Blynk App or in Gmail.
Else go to the Project Settings (nut icon))
```
char auth[] = "AUTH TOKEN";
```

#### WiFi Credentials
(Set password to "" for open networks.)
```
char ssid[] = "WIFI NAME";
char pass[] = "PASS";
```

#### Flag Define
```
bool speed1_flag = 1;
bool speed2_flag = 1;
bool speed3_flag = 1;
bool speed4_flag = 1;
bool speed0_flag = 1;

int switch_ON_Flag1_previous_I = 0;
int switch_ON_Flag2_previous_I = 0;
int switch_ON_Flag3_previous_I = 0;
int switch_ON_Flag4_previous_I = 0;
int switch_ON_Flag5_previous_I = 0;
```

#### Assigning Incoming Values from Blynk app to Variables
```
BLYNK_WRITE(V0)
{
  int fan_speed = param.asInt(); // assigning incoming value from pin V1 to a variable
  if (fan_speed == 0)
  {
    speed0();
  }
  if (fan_speed == 1)
  {
    speed1();
  }
  if (fan_speed == 2)
  {
    speed2();
  }
  if (fan_speed == 3)
  {
    speed3();
  }
  if (fan_speed == 4)
  {
    speed4();
  }
}

BLYNK_WRITE(V1)
{
  int pinValue = param.asInt(); // assigning incoming value from pin V1 to a variable
  digitalWrite(R5, pinValue);
  // process received value
}

BLYNK_WRITE(V2)
{
  int pinValue = param.asInt(); // assigning incoming value from pin V2 to a variable
  digitalWrite(R6, pinValue);
  // process received value
}

BLYNK_WRITE(V3)
{
  int pinValue = param.asInt(); // assigning incoming value from pin V3 to a variable
  digitalWrite(R7, pinValue);
  // process received value
}

BLYNK_WRITE(V4)
{
  int pinValue = param.asInt(); // assigning incoming value from pin V4 to a variable
  digitalWrite(R8, pinValue);
  // process received value
}
BLYNK_WRITE(V5)
{
  int pinValue = param.asInt(); // assigning incoming value from pin V4 to a variable
  digitalWrite(R9, pinValue);
  // process received value
}
```

#### Void Setup
```
void setup()
{
  // put your setup code here, to run once:
  pinMode(s1, INPUT);
  pinMode(s2, INPUT);
  pinMode(s3, INPUT);
  pinMode(s4, INPUT);
  pinMode(S5, INPUT);
  pinMode(S6, INPUT);
  pinMode(S7, INPUT);
  pinMode(S8, INPUT);
  pinMode(S9, INPUT);

  pinMode(R5, OUTPUT);
  pinMode(R6, OUTPUT);
  pinMode(R7, OUTPUT);
  pinMode(R8, OUTPUT);
  pinMode(R9, OUTPUT);
  pinMode(Speed1, OUTPUT);
  pinMode(Speed2, OUTPUT);
  pinMode(Speed4, OUTPUT);

  Serial.begin(9600);
  WiFi.begin(ssid, pass);

  timer.setInterval(3000L, checkBlynk); // check if connected to Blynk server every 3 seconds

  Blynk.config(auth);//, ssid, pass);
}
```

#### WiFi Status 

```
void loop()
{

  if (WiFi.status() != WL_CONNECTED)
  {
    if (DEBUG_SW) Serial.println("Not Connected");
  }
  else
  {
    if (DEBUG_SW) Serial.println(" Connected");
    Blynk.run();
  }
  timer.run(); // Initiates SimpleTimer

  if (MODE == 0)
    with_internet();
  else
    without_internet();
  // put your main code here, to run repeatedly:
}
```

#### With Internet Function
```
void with_internet()
{

   // FOR FAN
  if (digitalRead(s1) == HIGH && digitalRead(s2) == LOW && digitalRead(s3) == LOW && digitalRead(s4) == LOW && speed1_flag == 1)
  {
    speed1();
    Blynk.virtualWrite(V0, 1);
    speed1_flag = 0;
    speed2_flag = 1;
    speed3_flag = 1;
    speed4_flag = 1;
    speed0_flag = 1;


  }
  if (digitalRead(s1) == LOW && digitalRead(s2) == HIGH && digitalRead(s3) == LOW && digitalRead(s4) == LOW && speed2_flag == 1)
  {
    speed2();
    Blynk.virtualWrite(V0, 2);
    speed1_flag = 1;
    speed2_flag = 0;
    speed3_flag = 1;
    speed4_flag = 1;
    speed0_flag = 1;

  }
  if (digitalRead(s1) == LOW && digitalRead(s2) == LOW && digitalRead(s3) == HIGH && digitalRead(s4) == LOW && speed3_flag == 1)
  {
    speed3();
    Blynk.virtualWrite(V0, 3);
    speed1_flag = 1;
    speed2_flag = 1;
    speed3_flag = 0;
    speed4_flag = 1;
    speed0_flag = 1;
  }
  if (digitalRead(s1) == LOW && digitalRead(s2) == LOW && digitalRead(s3) == LOW && digitalRead(s4) == HIGH  && speed4_flag == 1)
  {
    speed4();
    Blynk.virtualWrite(V0, 4);
    speed1_flag = 1;
    speed2_flag = 1;
    speed3_flag = 1;
    speed4_flag = 0;
    speed0_flag = 1;
  }
  if (digitalRead(s1) == LOW && digitalRead(s2) == LOW && digitalRead(s3) == LOW && digitalRead(s4) == LOW  && speed0_flag == 1)
  {
    speed0();
    Blynk.virtualWrite(V0, 0);
    speed1_flag = 1;
    speed2_flag = 1;
    speed3_flag = 1;
    speed4_flag = 1;
    speed0_flag = 0;
  }
  
  
  // FOR SWITCH
  if (digitalRead(S5) == LOW)
  {
    if (switch_ON_Flag1_previous_I == 0 )
    {
      digitalWrite(R5, HIGH);
      if (DEBUG_SW) Serial.println("Relay1- ON");
      Blynk.virtualWrite(V1, 1);
      switch_ON_Flag1_previous_I = 1;
    }
    if (DEBUG_SW) Serial.println("Switch1 -ON");
    
  }
  if (digitalRead(S5) == HIGH )
  {
    if (switch_ON_Flag1_previous_I == 1)
    {
      digitalWrite(R5, LOW);
      if (DEBUG_SW) Serial.println("Relay1 OFF");
      Blynk.virtualWrite(V1, 0);
      switch_ON_Flag1_previous_I = 0;
    }
    if (DEBUG_SW)Serial.println("Switch1 OFF");
  }

  if (digitalRead(S6) == LOW)
  {
    if (switch_ON_Flag2_previous_I == 0 )
    {
      digitalWrite(R6, HIGH);
      if (DEBUG_SW)  Serial.println("Relay2- ON");
      Blynk.virtualWrite(V2, 1);
      switch_ON_Flag2_previous_I = 1;
    }
    if (DEBUG_SW) Serial.println("Switch2 -ON");

  }
  if (digitalRead(S6) == HIGH )
  {
    if (switch_ON_Flag2_previous_I == 1)
    {
      digitalWrite(R6, LOW);
      if (DEBUG_SW) Serial.println("Relay2 OFF");
      Blynk.virtualWrite(V2, 0);
      switch_ON_Flag2_previous_I = 0;
    }
    if (DEBUG_SW)Serial.println("Switch2 OFF");
    //delay(200);
  }

  if (digitalRead(S7) == LOW)
  {
    if (switch_ON_Flag3_previous_I == 0 )
    {
      digitalWrite(R7, HIGH);
      if (DEBUG_SW) Serial.println("Relay3- ON");
      Blynk.virtualWrite(V3, 1);
      switch_ON_Flag3_previous_I = 1;
    }
    if (DEBUG_SW) Serial.println("Switch3 -ON");

  }
  if (digitalRead(S7) == HIGH )
  {
    if (switch_ON_Flag3_previous_I == 1)
    {
      digitalWrite(R7, LOW);
      if (DEBUG_SW) Serial.println("Relay3 OFF");
      Blynk.virtualWrite(V3, 0);
      switch_ON_Flag3_previous_I = 0;
    }
    if (DEBUG_SW)Serial.println("Switch3 OFF");
    //delay(200);
  }

  if (digitalRead(S8) == LOW)
  {
    if (switch_ON_Flag4_previous_I == 0 )
    {
      digitalWrite(R8, HIGH);
      if (DEBUG_SW) Serial.println("Relay4- ON");
      Blynk.virtualWrite(V4, 1);
      switch_ON_Flag4_previous_I = 1;
    }
    if (DEBUG_SW) Serial.println("Switch4 -ON");

  }
  if (digitalRead(S8) == HIGH )
  {
    if (switch_ON_Flag4_previous_I == 1)
    {
      digitalWrite(R8, LOW);
      if (DEBUG_SW) Serial.println("Relay4 OFF");
      Blynk.virtualWrite(V4, 0);
      switch_ON_Flag4_previous_I = 0;
    }
    if (DEBUG_SW) Serial.println("Switch4 OFF");
    //delay(200);
  }

 if (digitalRead(S9) == LOW)
  {
    if (switch_ON_Flag5_previous_I == 0 )
    {
      digitalWrite(R9, HIGH);
      if (DEBUG_SW) Serial.println("Relay5- ON");
      Blynk.virtualWrite(V5, 1);
      switch_ON_Flag5_previous_I = 1;
    }
    if (DEBUG_SW) Serial.println("Switch5 -ON");

  }
  if (digitalRead(S9) == HIGH )
  {
    if (switch_ON_Flag5_previous_I == 1)
    {
      digitalWrite(R9, LOW);
      if (DEBUG_SW) Serial.println("Relay5 OFF");
      Blynk.virtualWrite(V5, 0);
      switch_ON_Flag5_previous_I = 0;
    }
    if (DEBUG_SW) Serial.println("Switch5 OFF");
    //delay(200);
  }

}
```


#### Without Internet Function
```
void without_internet()
{
// FOR FAN
  if (digitalRead(s1) == HIGH && digitalRead(s2) == LOW && digitalRead(s3) == LOW && digitalRead(s4) == LOW && speed1_flag == 1)
  {
    speed1();

    speed1_flag = 0;
    speed2_flag = 1;
    speed3_flag = 1;
    speed4_flag = 1;
    speed0_flag = 1;
  }
  
  if (digitalRead(s1) == LOW && digitalRead(s2) == HIGH && digitalRead(s3) == LOW && digitalRead(s4) == LOW && speed2_flag == 1)
  {
    speed2();

    speed1_flag = 1;
    speed2_flag = 0;
    speed3_flag = 1;
    speed4_flag = 1;
    speed0_flag = 1;
  }
  
  if (digitalRead(s1) == LOW && digitalRead(s2) == LOW && digitalRead(s3) == HIGH && digitalRead(s4) == LOW && speed3_flag == 1)
  {
    speed3();

    speed1_flag = 1;
    speed2_flag = 1;
    speed3_flag = 0;
    speed4_flag = 1;
    speed0_flag = 1;
  }
  
  if (digitalRead(s1) == LOW && digitalRead(s2) == LOW && digitalRead(s3) == LOW && digitalRead(s4) == HIGH  && speed4_flag == 1)
  {
    speed4();

    speed1_flag = 1;
    speed2_flag = 1;
    speed3_flag = 1;
    speed4_flag = 0;
    speed0_flag = 1;
  }
  
  if (digitalRead(s1) == LOW && digitalRead(s2) == LOW && digitalRead(s3) == LOW && digitalRead(s4) == LOW  && speed0_flag == 1)
  {
    speed0();

    speed1_flag = 1;
    speed2_flag = 1;
    speed3_flag = 1;
    speed4_flag = 1;
    speed0_flag = 0;
  }
  
   // FOR SWITCH
  digitalWrite(R5, !digitalRead(S5));
  digitalWrite(R6, !digitalRead(S6));
  digitalWrite(R7, !digitalRead(S7));
  digitalWrite(R8, !digitalRead(S8));
  digitalWrite(R9, !digitalRead(S9));
}
```

#### Fan Speed Control
```
void speed0()
{
  //All Relays Off - Fan at speed 0
  if (DEBUG_SW)Serial.println("SPEED 0");
  digitalWrite(Speed1, LOW);
  digitalWrite(Speed2, LOW);
  digitalWrite(Speed4, LOW);

}

void speed1()
{
  //Speed1 Relay On - Fan at speed 1
  if (DEBUG_SW)Serial.println("SPEED 1");
  digitalWrite(Speed1, LOW);
  digitalWrite(Speed2, LOW);
  digitalWrite(Speed4, LOW);
  delay(500);
  digitalWrite(Speed1, HIGH);
}

void speed2()
{
  //Speed2 Relay On - Fan at speed 2
  if (DEBUG_SW)Serial.println("SPEED 2");
  digitalWrite(Speed1, LOW);
  digitalWrite(Speed2, LOW);
  digitalWrite(Speed4, LOW);
  delay(500);
  digitalWrite(Speed2, HIGH);
}

void speed3()
{
  //Speed1 & Speed2 Relays On - Fan at speed 3
  if (DEBUG_SW)Serial.println("SPEED 3");
  digitalWrite(Speed1, LOW);
  digitalWrite(Speed2, LOW);
  digitalWrite(Speed4, LOW);
  delay(500);
  digitalWrite(Speed1, HIGH);
  digitalWrite(Speed2, HIGH);
}

void speed4()
{
  //Speed4 Relay On - Fan at speed 4
  if (DEBUG_SW)Serial.println("SPEED 4");
  digitalWrite(Speed1, LOW);
  digitalWrite(Speed2, LOW);
  digitalWrite(Speed4, LOW);
  delay(500);
  digitalWrite(Speed4, HIGH);
}
```

#### Check Blynk 
(To check Connectivity every 3 seconds)
```
void checkBlynk()
{
  bool isconnected = Blynk.connected();
  if (isconnected == false)
  {
    MODE = 1;
  }
  if (isconnected == true)
  {
    MODE = 0;
  }
}
```







