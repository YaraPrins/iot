# Table of Content

* [Manual 1]()
  * [Adafruit IO]()
  * [Quickstart]()
    * [General Installation]()
    * [Installing Adafruit IO]()
  * [My installation process]()
    * [First thoughts]()
    * [Starting the installation]()
    * [Connecting the board]()
    * [Uploading the code]()

# Manual 2 - Arduino, NodeMCU & Telegram

## 📱 Telegram
Telegram is a globally accessible freemium, cross-platform, cloud-based instant messaging (IM) service. The service also provides optional end-to-end encrypted chats and video calling, VoIP, file sharing and several other features. (- [Wikipedia](https://en.wikipedia.org/wiki/Telegram_(software)))

## 📔 Quickstart

### ⚙ General Installation
To get started, you need to have done a couple of things.

1. You need to have Arduino installed, and connected to a board that can send out and receive WiFi. (i.e. NodeMCU, which I will be using today).
2. Your board should be correctly installed and adapted to the Arduino software. This includes selecting the right board and port in your Arduino.

Make sure you have installed the board by going to Tools > Board> Board Manager, and look for "esp". Select `esp8266` by ESP8266 Community and install any version above `3.**`.
To then select that board and the port you are using, go to Tools > Board `NodeMCU 1.0 (ESP-12E Module)`, and Tools > Port > `COM3` (Windows) or `/dev/cu.SLAB_USBtoUART` (macOS).
3. You need to have a working internet connection.
4. You need to have a Telegram account
5. I'm using a LED strip today to control the colors, but you can also just make a chatbot or something like that

### ✔ Telegram Bot
From there you need to follow a couple of steps to get started before you get to coding.

#### BotFather

1. We're now first off all going to set up your bot in Telegram. So, make sure you have a Telegram account, and either access through Telegram Web or through the IOS/Android App. 
2. When you're logged in, go to the searchbar at the top of the screen and tpye in "BotFather".
3. Select the BotFather user and tap the start button at the bottom of the screen, or write `/start` in the chat.
4. After the welcoming message, type in `/newbot`, to create a new bot for your own.
5. BotFather will ask you to come up with a name. Fill in your prevered name and continue.
6. Then you need to choose a username for your bot, ending with "bot" or "_bot". It can take a bit because some are already taken (in that case you will get notified by BotFather).
7. When you're finished up with the name and username, BotFather will generate a token for you. Note or copy the token, it will be used in the code! (also make sure to never upload the token to GitHub or something, for privacy reasons of course).
8. Tap the start button at the bottom of the screen (I did this on IOS / Android app, I couldn't get it to work on my desktop).

#### Arduino Libraries

Now we have made our Telegram Bot, but before we can do anything with it on Arduino, we need to install a couple of Arduino Libraries to make sure the code runs smoothly.

The libraries you need to have installed:

- ArduinoJSON by Benoit Blanchon (version 5.13.5 , v6 is still in beta and not yet compatible with Telegram)
- CTBot by Stefano Ledda

and in my case because I am working with Adafruit Neopixel for my LED strip
- Adafruit NeoPixel by Adafruit


## Connecting your Telegram Bot to Arduino

### The Code

We will use one of the example codes from the CTBot library called `lightBot`. Go to File > Examples > CTBOT > lightBot, and select it. This will open up a new window.

### Setting Up
In the string `ssid` you need to fill in your WiFi network name, and in the string `pass` please fill in your WiFi password (don't worry, as long as you dont upload your code online, no one can hack your WiFi ;) ). Then, next up is the string `token`, please paste your Telegram Bot token you received earlier.

After filling in your general info that is needed, I will in my case also be declaring some extra things in this code. I am using a LED strip and the library Adafruit Neopixel to control the lights of my LED with the Telegram bot. So, on the top of my code, just beneath the 
```
#include "CTBot.h"
CTBot myBot;
``` 
part, I will write this too, to include the Adafruit library:

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

After writing this, I also changed the original lightBot code a bit, under the strings for your WiFi and Telegram Token, instead of `uint8_t led = 2` or `uint8_t led = BUILTIN_LED`, I made sure to connect it with my LED strip by putting `uint8_t led = LED_PIN`, so basically that means that my `uint8_led` is connected to pin D5 of my NodeMCU board.


## 🙋 My installation process

### 💭 First thoughts
Well, I think it's amazing that it is even possible to do things like this, connecting to WiFi and making changes in an app or something and that it will change the light, but I heard it's not that simple to set up. So, I'm curious if everything will go smooth or if I'll get stuck and annoyed a lot haha!
This document is meant as a experiment and I will document every step I make and errors I encounter. 

### 💯 Starting the installation
The 

### 🔌 Connecting the board



### 💾 Uploading the code
I first made sure everything was working alright, by first hitting the checkmark to verify my code and afterwards I uploaded it to my NodeMCU board.

My code was uploaded, and I started up the Serial Monitor, and put it on 115200 baud. When I opened this up, I kept getting dots printed, which I do not understand why...



I could easily edit the color with the color picker, everything went smoothly!

<img src="https://user-images.githubusercontent.com/27287809/194368530-df1ba421-dbe4-4581-92c4-a739256ad168.png" width="500px"/>





