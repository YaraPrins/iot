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
2. Your board should be correctly installed and adapted to the Arduino software.
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

## 🙋 My installation process

### 💭 First thoughts
Well, I think it's amazing that it is even possible to do things like this, connecting to WiFi and making changes in an app or something and that it will change the light, but I heard it's not that simple to set up. So, I'm curious if everything will go smooth or if I'll get stuck and annoyed a lot haha!
This document is meant as a experiment and I will document every step I make and errors I encounter. 

### 💯 Starting the installation
The first thing I encountered was that there are indeed multiple versions when you search for `Adafruit IO Arduino`, and was a bit confused. Then I remembered that for now, I need the 'Arduino IO Arduino' library (installed version 4.2.3), but appearantly I missed some dependencies that I did not have installed. So, of course I installed them too. This took a few seconds, but overall no problems.

<img src="https://user-images.githubusercontent.com/27287809/194360253-2961a816-d477-4cdf-abbb-8b74e4ae0910.png" width="500px"/>

Then, what I needed to do was making an account and a dashboard for Adafruit IO, through [the Adafruit IO website](https://io.adafruit.com/).

### 🔌 Connecting the board

These first few steps were relatively easy, just needed to put in some general info, username and password, and tada! I got my account. Afterwards I headed to the IO heading, and clicked on the yellow key icon to get the information for my username and password that had been generated. 

Then, in the Arduino software, I headed to `File -> Examples -> Adafruit IO Arduino -> adafruitio_14_neopixel`. This would open up a new Arduino file, and I put in my username and password in the `config.h` tab.
After I put this in, the software started loading something, but this took a very long time. It seemed like it got stuck in a loading loop (see image below)...

<img src="https://user-images.githubusercontent.com/27287809/194363778-35f60e22-57d1-4182-8580-c1f1443206ec.png" width="500px"/>

After a few minutes, I was certain it got stuck, so I closed off Arduino and started it back up again which indeed solved the loading state.

Then I also put in my WiFi name and WiFi password in the `config.h` tab, and I remembered that my board, the NodeMCU, does not work on a 5Ghz WiFi network, so I made sure I was on the 2.4Ghz one.
I also changed something in the tab for `adafruit_14_Neopixel.ino`, my LED strip would not have been connected to `#define PIXEL_PIN 5`, but to `#define PIXEL_PIN D5`. So, I corrected that code and I continued the steps.

### 💾 Uploading the code
I first made sure everything was working alright, by first hitting the checkmark to verify my code and afterwards I uploaded it to my NodeMCU board.

My code was uploaded, and I started up the Serial Monitor, and put it on 115200 baud. When I opened this up, I kept getting dots printed, which I do not understand why...

<img src="https://user-images.githubusercontent.com/27287809/194366731-4829c426-ca40-4de0-85c8-999f56688f31.png" width="500px"/>

This kept on going, and eventually I got why. My WiFi hotspot connection was turned off for some reason (I didn't think I put this off), so it couldn't connect. I turned my hotspot back on and it connected immediatly!

<img src="https://user-images.githubusercontent.com/27287809/194367331-174ec7d2-29da-4704-acb8-48d5f6d55070.png" width="500px"/>

This also turned the LED strip I had connected to the color of what I set the color picker to be on the Arduino IO website ( In my dashboard, the little settings icon on the top right, create new block, select color picker).

I could easily edit the color with the color picker, everything went smoothly!

<img src="https://user-images.githubusercontent.com/27287809/194368530-df1ba421-dbe4-4581-92c4-a739256ad168.png" width="500px"/>

<img src="https://user-images.githubusercontent.com/27287809/194368410-c5b7251a-a5ab-4e8a-a092-c58fc611382d.png" width="500px"/>




