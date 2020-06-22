# gardenbro

The follwing node-red modules are required:

#node-red-contrib-bigtimer
#node-red-contrib-dht-sensor
#node-red-contrib-tplink
#node-red-dashboard
#node-red-node-pi-gpio

RuuviTag related:
#node-red-contrib-noble
#node-red-contrib-ruuvitag

Ultrasonic sensor (SRF04):
#node-red-node-pisrf

For further information check out the thread on ledgardener:
https://ledgardener.com/forum/viewtopic.php?f=27&t=5698&p=22721#p22721

GardenBro
======

DIY Grow Room Controller

Latest version: 1.1

GardenBro is an open source Node-Red project for the Raspberry Pi that controls AC receptacles using either an 8-Channel hardwired relay board or TP-Link HS103 WiFi modules. DHT22 or RuuviTag BLE (Bluetooth Low Engergy) temperature/humidity sensors are used to trigger whatever type of devices are plugged in. The intent of the code is to be configurable for any setup, from small grow tent to multiple rooms. It is intented as a templated for others to start from and requires customization before use. Always consider relay contact closures for your important devices and have an electrician check your wiring if you are using hard-wired relays. For larger power loads never use the cheap 8-channel relay from amazon directly, use it to trigger a robust secondary relay that fits your needs.

Come join the grow room automation community at www.ledgardener.com and check out the forums. Alot of this was inspired by the awesome people over there and the projects they are working on.

Raspberry Pi Model B+, Model 3 B+ or better recommended for RuuviTag use.
Relay Board: https://amzn.to/37N7SNK
WiFi Receptacble: https://bit.ly/2BvfFUh
RuuviTag: https://ruuvi.com/

Features
--------

-  Inputs - RHT22, RuuviTag
-  Outputs - 8 Channel Relay Board, TP-Link HS103 WiFi Receptacle 
-  Web Interface - Node-Red built in dashboard interface which is hosted on your local network by default at http://10.0.0.81:1880/ui.      An easy means of remote access is a secure VPN.
-  Dashboard - Currently setup for best viewability on a Galaxy S8 using a 7 tile width for the dashboard nodes. You can play around        with width size for smaller phones or larger tablets. 
  -  Temperature gauge. Graphs in fahrenheit and celcius.
  -  Humidity gauge/graph.
  -  Leaf VPD and air VPD gauge/graphs. Leaf VPD is calculated using a manually entered leaf temperature usually taken with an IR            thermometer. VPD is a target value and not used for control at this time, it is based off the following: https://bit.ly/2AN74g4.
-  Setpoints - Used to change various device setpoints for day and night. Day and night is determined by the lights being on/off. In        auto mode the setpoints are used for control and in manually the user has control via the on/off buttons. All setpoints have default    values that are loaded when the program first starts.
  -  Heater - Typical 1500W space heater. Be careful because I found out the hard way you need a decent relay to power a 1500W heater, I      ended up using a 30A solid state after melting the first 15A mechanical. The TP-Link HS103 is rated for 12A and so far I have had        no issues.
  -  Dehumidifier/Humidifer - Works in the same way as Heater.
  -  Exhaust temperature/humidity - Controls an ehxaust and intake fan relay. You cannot use both control methods at the same time and        the buttons are interlocked. Humidity control can be used effectively to maintain a target VPD if temperature loads in the room are      not too high. Exhaust maintains humidity, and set points on the heater are kept tight to maintain ideal temperature. The following      chart is used for reference: https://pulsegrow.com/blogs/learn/vpd

-  `Alerts <https://github.com/kizniche/Mycodo/blob/master/mycodo-manual.rst#alerts>`__ to notify via email when measurements reach or exceed user-specified thresholds.
-  `Camera Feed <https://github.com/kizniche/Mycodo/blob/master/mycodo-manual.rst#camera>`__ for remote live stream, image capture, or time-lapse photography.

Odd Uses
--------

Originally developed to cultivate edible mushrooms, Mycodo has evolved to do much more. Here are a few things that have been done with Mycodo:

My projects

-  `Hydroponic System Automation <https://kylegabriel.com/projects/2020/06/automated-hydroponic-system-build.html>`__
-  `Mushroom cultivation <https://kylegabriel.com/projects/2015/04/mushroom-cultivation-revisited.html>`__
-  `Ground-based plant cultivation <https://www.youtube.com/watch?v=QNCx_VE7D-8>`__
-  `Maintaining honey bee apiary homeostasis <https://kylegabriel.com/projects/2015/12/environmentally-controlled-apiary.html>`__
-  `Maintaining humidity in an underground artificial bat cave <https://kylegabriel.com/projects/2015/10/artificial-bat-cave.html>`__
-  `Remote radiation monitoring and mapping <https://kylegabriel.com/projects/2019/08/remote-radiation-monitoring.html>`__
-  `Cooking sous-vide <https://hackaday.io/project/11997-mycodo-environmental-regulation-system/log/45733-sous-vide-pid-tuning-and-the-unexpected-electrical-fire>`__

Screenshots
-----------

.. image:: https://ledgardener.com/forum/download/file.php?id=4305&mode=view
   :target: https://ledgardener.com/forum/viewtopic.php?f=35&t=5698


Install GardenBro
-----------------

Prerequisites
~~~~~~~~~~~~~

-  `Raspberry Pi <https://www.raspberrypi.org>`__ single-board computer (any version: Zero, 1, 2, 3, or 4)
-  `Raspberry Pi Operating System <https://www.raspberrypi.org/downloads/raspberry-pi-os/>`__ flashed to a micro SD card
-  An active internet connection

Mycodo has been tested to work with Raspberry Pi OS Lite (2020-05-27).

Install
~~~~~~~

Once you have the Raspberry Pi booted into the Raspberry Pi OS with an internet
connection, run the following command in a terminal to initiate the
Mycodo install:
