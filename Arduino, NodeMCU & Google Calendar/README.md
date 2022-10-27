Written by Yara Prins | 26 oct 2022

# Table of Content

* [Manual 3 - Arduino, NodeMCU & Google Calendar](https://github.com/YaraPrins/iot/tree/main/Arduino%2C%20NodeMCU%20%26%20Google%20Calendar#manual-3---arduino-nodemcu--google-calendar)
  * [📱 What are we going to do?](https://github.com/YaraPrins/iot/tree/main/Arduino%2C%20NodeMCU%20%26%20Google%20Calendar#-what-are-we-going-to-do)
  * [📔 Quickstart](https://github.com/YaraPrins/iot/tree/main/Arduino%2C%20NodeMCU%20%26%20Google%20Calendar#-quickstart)
  	 * [⚙ General Installation](https://github.com/YaraPrins/iot/tree/main/Arduino%2C%20NodeMCU%20%26%20Google%20Calendar#-general-installation)
  	 * [🏫 Arduino Libraries](https://github.com/YaraPrins/iot/tree/main/Arduino%2C%20NodeMCU%20%26%20Google%20Calendar#-arduino-libraries)
  	 * [✔ Google Calendar events](https://github.com/YaraPrins/iot/tree/main/Arduino%2C%20NodeMCU%20%26%20Google%20Calendar#-google-calendar-events) 	
  * [💾 The Code](https://github.com/YaraPrins/iot/tree/main/Arduino%2C%20NodeMCU%20%26%20Google%20Calendar#-the-code)
    * [💻 Setting Up](https://github.com/YaraPrins/iot/tree/main/Arduino%2C%20NodeMCU%20%26%20Google%20Calendar#-setting-up)
  * [🙋 My installation process](https://github.com/YaraPrins/iot/tree/main/Arduino%2C%20NodeMCU%20%26%20Google%20Calendar#-my-installation-process) 
    * [💭 First thoughts](https://github.com/YaraPrins/iot/tree/main/Arduino%2C%20NodeMCU%20%26%20Google%20Calendar#-first-thoughts)
    * [🔌 Trial and Error](https://github.com/YaraPrins/iot/tree/main/Arduino%2C%20NodeMCU%20%26%20Google%20Calendar#-trial-and-error)



# Manual 3 - Arduino, NodeMCU & Google Calendar

## 📱 What are we going to do?
We are going to try and connect a Google Calendar to your Arduino IDE + Adafruit IO feed, so that when something happens in the Google Calendar, you can set up an action that you wanted to achieve when that happened.
In my case, everytime your schedule for the day has a longer duration than 8 hours total, I wanted to change my LED strip (with Adafruit NeoPixel) to a certain color, and when it was below that number I wanted to see a different color.


## 📔 Quickstart

Before we begin, you need to have a couple of things to get started. You need to have:

- Laptop/pc with functioning internet connection
- Board that can connect to WiFi (in my case the NodeMCU ESP8266)
- Adapter for the board
- LED strip that can connect to the board
- Adafruit IO profile
- Zappier profile
- Google Calendar profile
- Basic understanding of C++ / Arduino code

### ⚙ General Installation
To get started, you need to have done a couple of things.

1. You need to have Arduino installed, and connected to a board that can send out and receive WiFi. (i.e. NodeMCU ESP8266, which I will be using today).
2. Your board should be correctly installed and adapted to the Arduino software. This includes selecting the right board and port in your Arduino.

Make sure you have installed the board by going to Tools > Board> Board Manager, and look for "esp". Select `esp8266` by ESP8266 Community and install any version above `3.**`.
To then select that board and the port you are using, go to Tools > Board `NodeMCU 1.0 (ESP-12E Module)`, and Tools > Port > `COM3` (Windows) or `/dev/cu.SLAB_USBtoUART` (macOS).
3. You need to have a working internet connection.
5. I'm using a LED strip today to control the colors, but you can always change the prefered action.

### 🏫 Arduino Libraries

Before we can do anything with it on Arduino, we need to install a couple of Arduino Libraries to make sure the code runs smoothly.

The libraries you need to have installed:

- Adafruit IO Arduino by Adafruit

and in my case because I am working with Adafruit Neopixel for my LED strip
- Adafruit NeoPixel by Adafruit

> To learn more about the Adafruit NeoPixel library, I would recommend checking out these sources
> 
> [learn.adafruit.com](https://learn.adafruit.com/adafruit-neopixel-uberguide/the-magic-of-neopixels),
> [adafruit.github.io](https://adafruit.github.io/Adafruit_NeoPixel/html/class_adafruit___neo_pixel.html#a310844b3e5580056edf52ce3268d8084),
> & [partsnotincluded.com](https://www.partsnotincluded.com/getting-started-with-the-adafruit-neopixels-library/)
> 

### ✔ Google Calendar events
From there you need to follow a couple of steps to get started before you get to coding.

> The steps I followed come from 2 sources:
> 
> Source 1, setting up Adafruit IO |
> https://docs.google.com/document/d/14jhaxaJUhuu0p6_u_oNj8_50Y6PAmtrvcePtKc0HDGs
>
> Source 2, Connecting with Zappier |
> https://www.instructables.com/Google-Calendar-Events-to-ESP8266/
>
> (from source 2 I used steps 1 & 2, and customized step 3 to my own project)
>


## 💾 The Code

I used an example code from [this GitHub Repo](https://github.com/SummerDanoe/ReadGoogleCalFeed), which was linked in the source I used previously (source 2). 
I downloaded the code as a ZIP, unpacked the ZIP on my desktop and I opened it up on my Arduino IDE software.
After opening it up, I saw 2 tabs ( `config.h` & `readfeedtutorial.ino`). 


### 💻 Setting Up
Having the `config.h` tab open, we first need to set up some connection to your WiFi and Adafruit IO account.

First of all, in the string `IO_USERNAME`, please replace what is in the quotation marks with your Adafruit IO Username, and in the string from `IO_KEY`, please enter your IO API key (you can find it at the big yellow button top right on the Adafruit IO website).

After that you need to fill in some things for your WiFi connection. 

>
> PLEASE NOTE!
> 
> The NodeMCU ESP8266 board is not able to receive 5GHz WiFi, so please make sure your WiFi is set to 2.4Ghz.
>

In the string `ssid` (line 34) you need to fill in your WiFi network name, and in the string `pass` (line 35) please fill in your WiFi password (don't worry, as long as you dont upload your code online, no one can hack your WiFi ;) ).

After filling in your general info that is needed, I will in my case also be declaring some extra things in this code. I am using a LED strip and the library Adafruit Neopixel to control the lights of my LED after the trigger from Google Calendar. So, on the top of my code (tab `readfeedtutorial`, just beneath the 
```
#include "config.h"
``` 
part), I will write this too, to include the Adafruit library:

```
// LED STRIP ADAFRUIT
#include <Adafruit_NeoPixel.h>
#ifdef __AVR__
 #include <avr/power.h> // Required for 16 MHz Adafruit Trinket
#endif

#define LED_PIN D5
#define LED_COUNT 15

// Declare our NeoPixel strip object:
Adafruit_NeoPixel strip(LED_COUNT, LED_PIN, NEO_GRB + NEO_KHZ800);
// Argument 1 = Number of pixels in NeoPixel strip
// Argument 2 = Arduino pin number (most are valid)
// Argument 3 = Pixel type flags, add together as needed:
//   NEO_KHZ800  800 KHz bitstream (most NeoPixel products w/WS2812 LEDs)
//   NEO_KHZ400  400 KHz (classic 'v1' (not v2) FLORA pixels, WS2811 drivers)
//   NEO_GRB     Pixels are wired for GRB bitstream (most NeoPixel products)
//   NEO_RGB     Pixels are wired for RGB bitstream (v1 FLORA pixels, not v2)
//   NEO_RGBW    Pixels are wired for RGBW bitstream (NeoPixel RGBW products)

```

This part of the code is based off on the example code `strandtest` from the library Adafruit Neopixel, with slight changes (my LED_PIN is D5 instead of 6, because my ledstrip is connected to the pin D5, and my LED_COUNT is 15, the amount of LEDs I have on my LED strip).


## 🙋 My installation process

### 💭 First thoughts
It wasn't that hard luckily to connect Zappier to my Adafruit IO feed and my Google Calendar, but it was very hard to read out the data values from the different actions set up in Zappier, which I wanted to use in my code.


### 🔌 Trial and Error

First of all, I made some silly mistakes that I quickly solved. 

<img src="https://user-images.githubusercontent.com/27287809/198257952-e83b749e-0b9a-450f-a5f8-b65ce1f04d85.png" width="600"/>

>
> Not selecting the board and port in my Arduino IDE software
>

<img src="https://user-images.githubusercontent.com/27287809/198258135-f8e3bb3e-efbc-46ae-a253-a2510814721c.png" width="600"/>

>
> Trying to connect with my WiFi but realizing I had not changed the WiFi (was at a different place). After that, connection did work.
>

After those kind of silly mistakes, I tried to see if my code and the connection was working, so I did some testing with Zappier and made a few testing events on my own Google Calendar. This worked perfectly! The example code had a certain way to read out the values, which worked, but I wanted to do things a little different.

<img src="https://user-images.githubusercontent.com/27287809/198259091-fa478acd-accb-491d-b488-1be5b0c3c681.jpeg" width="300" align="right"/>
<img src="https://user-images.githubusercontent.com/27287809/198259288-c1aa84a3-d6f3-451c-aff7-1e5797c1518e.png" width="600"/>
<img src="https://user-images.githubusercontent.com/27287809/198259315-2abef3cc-2863-4997-a8c6-e74322cb3caa.png" width="600"/>

After I managed to get the data using the example file, I wanted to customize the code a bit. So, I started off with adding some other values in the Zappier action fields, but apparently this did not work with the original example code.

<img src="https://user-images.githubusercontent.com/27287809/198259586-13ea56aa-f6ef-4021-b86e-20f7bee112c3.png" width="600"/>

It messed up the whole structure that was created, but that came from this piece of code:

```

    // Turn the feed message into a string
 String answer = data;
 Serial.println(answer);
 Serial.println("Raw answer " + answer);
 
//  Turn the string into usable data
   date
   String date = answer.substring(0,10);
   //starting time
   String startHourString = answer.substring(11,13);
   int startHour = startHourString.toInt();
 
   String startMinuteString = answer.substring(14,16);
   int startMinute = startMinuteString.toInt();

   //ending time
   String endHourString = answer.substring(36,38);
   int endHour = endHourString.toInt();
  
   String endMinuteString = answer.substring(39,41);
   int endMinute = endMinuteString.toInt();

   String nameEvent = answer.substring(50);
  
  
//  Print results in serial monitor
 Serial.println("Raw asnwer " + answer);

 Serial.println("");
 Serial.println(nameEvent);
 Serial.println("***********************");
 Serial.println("Training on " + date );
 Serial.println("***********************");
 Serial.println("Training starts at " + startHourString + ":" + startMinuteString);
 Serial.println("Training ends at " + endHourString + ":" + endMinuteString);
  


//  You can use the data to calculate all sorts of things
  
 Calculate duration (doesn't work at midnight)
 int startTime = startHour*60 + startMinute; //starting time in minutes of the day
 int endTime = endHour*60 + endMinute; //ending time in minutes of the day
 int durationTotal = endTime - startTime; //duration in minutes
 int durationHours = durationTotal/60; //duration whole hours
 int durationMin = durationTotal%60; // rest of duration in minutes
 Serial.print("Training lasts ");
 Serial.print(durationHours);
 Serial.print(" hours and ");
 Serial.print(durationMin);
 Serial.println(" minutes");

}

```

This made sure to get the value data, but because it was all in one long string, it needed to be seperated into pieces and needed the declarations from where to where it needed to be cut. This also meant that I could not use the 'pretty' format, because that would have messed up the order as well.

I did not like this, so I tried something different. I added multiple actions for my Zappier trigger, each action with a different Google Calendar value (one event start (pretty), the other event end (pretty), and also the duration, and the event description (name)).

<img src="https://user-images.githubusercontent.com/27287809/198261077-972e6559-5e43-4281-b366-acb2f29d9314.png" width="600"/>

After I put that in, I tried to see if I could read out the data, but this was very difficult to do. Because I was not very familiar with the documentation of Adafruit IO and how that would work with reading out Feed data and such, I struggled a lot with the right way to collect my data. I tried a couple of ways to `Serial.println();` the incomming data values, but I kept getting the wrong values, or it didn't work at all.

<img src="https://user-images.githubusercontent.com/27287809/198260756-f3a2a96f-2c92-4e72-9753-49591f3fe7a4.png" width="600"/>
<img src="https://user-images.githubusercontent.com/27287809/198261194-2b83b919-4d60-46e3-af8d-e7434f73da42.png" width="600"/>
<img src="https://user-images.githubusercontent.com/27287809/198261253-293d64b9-8dde-43f6-a9b6-b9b1047735f7.png" width="600"/>

This eventually worked, by putting in this:

```
void handleMessage(AdafruitIO_Data *data) {

String dataStr = data->value();
Serial.println(dataStr);

}
```


Next up, I wanted to try and see if I could read out the duration, and if the duration would be > 8 hours for that day, I wanted to change the color of my LED to a certain color. I couldn't get this to work sadly...

