---
layout: post
title: 4 weeks left
date: 2014-04-07 23:43:44.000000000 +01:00
---

I am now half way through my dissertation (Major Project, may also be known by other names), it has been a long slog so far but the end is almost in sight. A couple of weeks ago I gave my second marker a demo of what I had so far, and got some good feedback about cromlech and how to over come some of the technical issues I was facing. However now is not the time to focus on that, those issues are for another post when I’ve finished my dissertation :).


## Messaging

For now lets focus on some of the technologies I’ve been learning and integrated into cromlech.

Firstly lets focus on the messaging protocol and libraries used to implement it. I chose MQTT,  it is ideal for realtime monitoring of data, because it is not a polling protocol, it is a pub sub protocol, I’m not going to go into the details of MQTT there are far better placed people than me for this.

The libraries I picked up to use to integrate MQTT into cromlech are from the [Eclipse Paho project](http://eclipse.org/paho). Their MQTT C++ wrapper library and their C library (C library is required for the C++ library). I chose Paho because I know of the project and the C library is mature after many years of development at IBM and  I worked with the guys behind it, so I find the idea of asking them for help more inviting than asking for help from a git hub repo. The eclipse libraries are used solely in the cromlech mesh monitoring nodes, because I found myself lacking the skill needed to be able to write an Objective-C wrapper for it to use as part of the Monitoring UI I am building using Cocoa.

For the cocoa application, I found a different library with an Objective-C wrapper already implemented, this project is found on GitHub and is called [MQTTKit](https://github.com/mobile-web-messaging/MQTTKit). MQTTKit uses the [Mosquitto](http://mosquitto.org) library to implement MQTT.  (Side Note: Mosquitto has become and Eclipse project, so I do not know if the client library will still be developed since eclipse also has the Paho.)

Now the MQTT server I am using for test purposes is the [Mosquitto](http://mosquitto.org) MQTT Broker, there was no real decision-making involved around this choice other than it was available in the repos and I had previously heard of Mosquitto while working at IBM.


## Graph Plotting

The biggest part of the monitoring UI is the ability to graph in realtime the data that is being received for a monitoring node in real-time. My first plan was to use QT to develop my user interface since it would be cross-platform and is written in C++ making it easier to develop the entire system. However the library and found and appeared to most recommended was  a QT library that was released under the GPL. Although I have no objection to GPL, I’d prefer knowing what I can do with my source code once I graduate from Aberystwyth and the LGPL version would need me to pay the developer €430 for a developer licence, so that was out.

I then did some research on graphing libraries for Cocoa and found [Core Plot](https://github.com/core-plot/core-plot), a plotting library that works on both OS X and iOS, I decided to use this library as it appeared to have an active community, through their mailing list and through stackoverflow.


## JSON Configuration Files

Cromlech uses a series of JSON configuration files to define and mesh and pass config to the monitoring nodes (this can be improved I know but to overcome a technical issues I was having I settled for a separate program for the monitoring nodes. The library I choose for this was Jansson, Jansson is a C library rather than a C++ library, however my reason for choosing it was that it had a an active community developing it, I had some experience using it at IBM as part of a test framework I was involved in creating and it was more active than the C++ libraries I had been pointed at from internet searches.


## What’s left to do

Well quite a lot is left to do if I am honest, still lots of GUI work that needs to be done, need to add more configuration options, (although these can be put in last-minute), the report that goes with the program and a lot of documentation such as user and installation manuals and code documentation (yes I know  should be doing it as I go along but with developing my project in an agile manner the class and member function documentation has gotten a bit out of step with the actual code). With around 5 weeks left until hand in, it is going to be tight to get all this done, however with easter coming I should be able to get it finished, bound and handed in before the deadline. Time for a big push for the few weeks left.


