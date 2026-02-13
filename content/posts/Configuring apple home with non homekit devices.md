---
date: 2021-06-30T19:38:49+01:00
description: Setting up apple home for your non-homekit supported devices through HomeBridge
slug: apple home for non homekit devices
tags:
- apple
- IoT
- Homekit
- Siri
categories:
- tech
title:  Configuring Apple Home With Non-Homekit Devices
summary: Setting up apple home for your non-homekit supported devices through HomeBridge
draft: false
---



Thanks to the competition in IoT device market, it's now affordable to have some installed at home. Most devices are compatible with Google Assistant & Amazon Alexa - popular AI based virtual assistant which helps you to control them using voice commands. It's hard to find devices compatible with with Apple's Siri & Apple Home leaving Apple aficionados not much choice but to buy HomeKit supported products which are quite expensive.
### Homebridge to the rescue
Homebridge is a lightweight NodeJS server which can be deployed at home to mimic iOS HomeKit API. Numerous community contributed modules of HomeBridge exists which bridges the gap between HomeKit API and 3rd party APIs provided by vendors. So my now affordable smart devices, can be used with Apple Home which also allows me to interact with them via Siri.

### Deployment
Homebridge can be installed using a lot of native & non-native ways but the easiest/fastest can be using Docker. As it needs to be up always, installing on a raspberry pi can be a viable option. Once the container is up, open http://yourip:8521 in your browser and login with the default admin credentials.

![HomeBridge UI](/images/homebridge_ui.png)

The web UI is intuitive and will allow you to install modules and add your IoT devices.

### Experience

With the Home app, devices (aka accessories) can be added by scanning HomeKit Setup Code which is available in the UI of Homebridge. Once done, rooms can be created and the UI customized to add accessories.

{{< image src="/images/apple_my_home.png"  position="center" style="border-radius: 8px;width:40%;display:inline-block;padding:0.1em;margin:1em;" >}}
{{< image src="/images/apple_my_office.png"  position="center" style="border-radius: 8px;width:40%;display:inline-block;padding:0.1em;margin:1em;" >}}

Scenes are automated routines which can be configured in Apple Home. Initially I was a bit disappointed as scenes appeared to be very limited as compared to 3rd party software solutions. I did configure the **Leave Home** scene which triggers all devices to switch off. 
> Hey Siri! I'm leaving home

would switch all devices off. However, it's a very basic scene like that of IFTTT. I'm sure IFTTT has better modules. What I expected the least is - **If it's sunset, turn the backyard lights on** or **If I'm closer to my home, switch on the heater & perhaps the speaker with some welcome Home song** but I could just  set the scene & request verbally to Siri for using it.

Ability to share the configuration with other iOS users is what I found amazing. Excited try this one, I shared it with another friend who has an iPhone. She got an invite but was getting unavailable errors in her Home app. 

Within a few minutes of web search, it was clear that the other person, needs to be present on the same location & network to use the features. Yes, I hear you saying how about a VPN? Turns out Apple uses some proprietary networking technology which makes VPN ineffective here.

{{< image src="/images/remote_avail.png"  position="center" style="border-radius: 8px;width:40%;" >}}

The way to make it work remotely is using Apple TV (> 4th gen), HomePod or an iPad running iOS 10 or later. Once, this setup is present, configuring better automation scenes like mentioned above, can be possible.

Unfortunately I don't have any of these Apple devices lying around to make the automation & remote access work.

For now, I rely on the vendor's app to control these devices remotely, as well as setting up advanced scenes/routines.

{{< image src="/images/tp-1.jpg" alt="Hello Friend" position="center" style="border-radius: 8px;width:40%;display:inline-block;padding:0.1em;margin:1em;" >}}
{{< image src="/images/tp-2.jpg" alt="Hello Friend" position="center" style="border-radius: 8px;width:40%;display:inline-block;padding:0.1em;margin:1em;" >}}

### Result
For my TP-link smart sockets, their app works pretty neat. I am able to define routines like, turn on the backlight with sunset & turning off with sunrise.

Rest, I'm happy to invoke Siri when at Home :)


_Thanks to a post from [@Mrwhoistheboss](https://twitter.com/Mrwhoistheboss)'s which motivated me to try Homebridge._

