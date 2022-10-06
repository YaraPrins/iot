# Adafruit IO
Adafruit IO is a library for Arduino, used with Internet of Things. With this library you can put sensordata online (with help of the MQTT protocol) in a "feed", where you can use your actuators through the web.

# Quickstart

## General Installation
To use Adafruit IO, you need to have done a couple of things.

1. You need to have Arduino installed, and connected to a board that can send out and receive WiFi. (i.e. NodeMCU, which I will be using today).
2. Your board should be correctly installed and adapted to the Arduino software.
3. You need to have a working internet connection.
4. You need to have some device or piece that is considered 'smart', for example a Philips Hue lightbulb.

## Installing Adafruit IO
From there, you need to install the Adafruit IO library in your Arduino software by going to your Library Manager, typing in Adafruit IO Arduino and install your prefered version (there are multiple, each with their own perks, so look for your own preferences).

# My installation process

## First thoughts
Well, I think it's amazing that it is even possible to do things like this, connecting to WiFi and making changes in an app or something and that it will change the light, but I heard it's not that simple to set up. So, I'm curious if everything will go smooth or if I'll get stuck and annoyed a lot haha!
This document is meant as a experiment and I will document every step I make and errors I encounter. 

## Starting the installation
The first thing I encountered was that there are indeed multiple versions when you search for `Adafruit IO Arduino`, and was a bit confused. Then I remembered that for now, I need the 'Arduino IO Arduino' library (installed version 4.2.3), but appearantly I missed some dependencies that I did not have installed. So, of course I installed them too. This took a few seconds, but overall no problems.

![image](https://user-images.githubusercontent.com/27287809/194360253-2961a816-d477-4cdf-abbb-8b74e4ae0910.png)

Then, what I needed to do was making an account and a dashboard for Adafruit IO, through (the Adafruit IO website)[https://io.adafruit.com/].

## Connecting the board

These first few steps were relatively easy, just needed to put in some general info, username and password, and tada! I got my account. Afterwards I headed to the IO heading, and clicked on the yellow key icon to get the information for my username and password that had been generated. 

Then, in the Arduino software, I headed to `File -> Examples -> Adafruit IO Arduino -> adafruitio_14_neopixel`. This would open up a new Arduino file, and I put in my username and password in the `config.h` tab.
After I put this in, the software started loading something, but this took a very long time. It seemed like it got stuck in a loading loop (see image below)...

![image](https://user-images.githubusercontent.com/27287809/194363778-35f60e22-57d1-4182-8580-c1f1443206ec.png)




