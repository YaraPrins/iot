Written by Yara Prins | 26 oct 2022

# Table of Content

* [Manual 3 - Arduino, NodeMCU & Google Calendar](https://github.com/YaraPrins/iot/tree/main/Arduino%2C%20NodeMCU%20%26%20Telegram#manual-2---arduino-nodemcu--telegram)
  * [📱 Telegram]()
  * [📔 Quickstart]()
  	* [⚙ General Installation]()
  	* [✔ Telegram Bot]()
  		* [🤖 BotFather]()
  		* [🏫 Arduino Libraries]()
  	* [🔌 Connecting your Telegram bot to Arduino]()
  		* [💾 The Code]()
  		* [💻 Setting Up]()
  * [🙋 My Installation Process]()
  	* [💭 First Thoughts]()
  	* [🔌 Bot with LED strip]()
  	* [👨‍💻 Writing the Code]()

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

After writing this, I also changed the original lightBot code a bit, under the strings for your WiFi and Telegram Token, instead of `uint8_t led = 2` or `uint8_t led = BUILTIN_LED`, I made sure to connect it with my LED strip by putting `uint8_t led = LED_PIN`, so basically that means that my `uint8_led` is connected to pin D5 of my NodeMCU board.


## 🙋 My installation process

### 💭 First thoughts
It wasn't that hard luckily to make it work with the build in LED light from my NodeMCU board, but it was relatively tricky to get it working with my LED strip. Although I did get it to work, I think if I have a bit more understanding of how C++ and Arduino works, I would have made faster and more progress than relatively simple code that I have used here.

### 🔌 Bot with LED strip
After the Telegram Bot has been made, and I had it connected it to Arduino, I started by checking out the example code and tried to understand what they meant and wrote. It was pretty straight forward and soon I got what they wrote. I tested it on my phone with the Telegram app, but at first the bot wouldn't respond, it didn't turn the light on or off and didn't send out a message back...

This was quite odd, because I was certain that I connected it correctly, so I tried again, pasted the token again, but yet with no success. Then I tried to use my actual WiFi instead of my mobile WiFi hotspot, and then it suddenly worked!

### 👨‍💻 Writing the code
First I only worked with the build in light from the board, and it eventually worked, but now I wanted to control my LED strip.

When I made sure that my Telegram Bot worked with the initial commands, I started to change it around a bit by adding a third `else if` statement, stating that if the incoming text to the bot was `color to red`, it needed to put the color of the light to red.

Here is where it started to get really tricky. Because I wasn't sure how to connect my LED strip to this chatbot code. But, then I remembered that I have a library on my arduino called `Arduino NeoPixel`, which uses the LED strip. So, I opened up an example code from that, and added some of that code into my original lightBot code (see above for set up). After setting this up correctly, I also put this into my `void setup(){}` code

```
  // These lines are specifically to support the Adafruit Trinket 5V 16 MHz.
  // Any other board, you can remove this part (but no harm leaving it):
  // #if defined(__AVR_ATtiny85__) && (F_CPU == 16000000)
    // clock_prescale_set(clock_div_1);
  // #endif
    // END of Trinket-specific code.

    strip.begin();           // INITIALIZE NeoPixel strip object (REQUIRED)
    strip.show();            // Turn OFF all pixels ASAP
    strip.setBrightness(50); // Set BRIGHTNESS to about 1/5 (max = 255)
```

This makes sure to set up the LED strip once when you start up your code.

Then, in my loop code, I put some code into my `if` statements, where I first of all put in (in my `if` statement for `CTBotMessageText == myBot.getNewMessage(msg)) {}`)
```
		if (msg.text.equalsIgnoreCase("LIGHT ON")) {              
			strip.fill((255,255,255), 0, 0);                              
			myBot.sendMessage(msg.sender.id, "Light is now ON");
		}
		}
```

which worked, but the lights turned blue instead of my intended white.
I also tried to add a 'turn color to red' `if` statement, but this wouldnt work for some reason. I did the same as above, but instead of `strip.fill((255,255,255), 0, 0)` (first the color, then the starting point and lastly how many lights to be affected), I put in `strip.fill((255,0,0), 0, 0)` for it to be filled in with red. But, this code didn't work. 

Note: it did send a message afterwards that it turned to red, but the color of the LED strip turned off instead of to red.

I couldn't understand why this was happening, and got really frustrated. After a bit of looking on the web, I tried this instead of `string.fill`:

```
if (msg.text.equalsIgnoreCase("LIGHT ON")) {
      for (int i = 0; i<strip.numPixels(); i++) {
          strip.setPixelColor(i, 50, 50, 50);
      }              // if the received message is "LIGHT ON"..
      strip.show();                                // turn on the LED (inverted logic!)
			myBot.sendMessage(msg.sender.id, "Light is now ON");  // notify the sender
		}
```

So instead of the fill property, I used `setPixelColor`, with a `for` statement with an `int i` to say i is the number of the pixel on the ledstrip. In this piece of code above I'm stating that as long as i is below the total number of pixels on the strip, increase the i, and run this next code.

>
> Sources
> https://adafruit.github.io/Adafruit_NeoPixel/html/class_adafruit___neo_pixel.html#a310844b3e5580056edf52ce3268d8084
> &
> https://www.partsnotincluded.com/getting-started-with-the-adafruit-neopixels-library/
> 

With this change, my code did work! And also in the correct colors! As last part of this journey I also added an `if` statement for the colors green and blue, and these worked perfectly fine!

<!-- <img src="https://user-images.githubusercontent.com/27287809/194368530-df1ba421-dbe4-4581-92c4-a739256ad168.png" width="500px"/> -->

