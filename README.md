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
-  Web Interface - Node-Red built in dashboard interface which is hosted on your local network by default at http://10.0.0.81:1880/ui.    An easy means of remote access is a secure VPN. Currently setup for best viewability on a Galaxy S8 using a 7 tile width for the        dashboard nodes. You can play around with width size for smaller phones or larger tablets. 
-  Setpoint control - Used to change various device setpoints for day and night. Day and night is determined by the lights being on/off.  In auto mode the setpoints are used for control and in manually the user has control via the on/off buttons. All setpoints have          default values that are loaded when the program first starts.

Dashboard
--------

  -  Temperature gauge. Graphs in fahrenheit and celcius.
  -  Humidity gauge/graph.
  -  Leaf VPD and air VPD gauge/graphs. Leaf VPD is calculated using a manually entered leaf temperature usually taken with an IR thermometer. VPD is a target value and not used for control at this time, it is based off the following: https://bit.ly/2AN74g4.
  -  LED indication lights on each of the devices which show if Node-Red is sending a signal.
  
Setpoints
--------

  -  Heater - Typical 1500W space heater. Be careful because I found out the hard way you need a decent relay to power a 1500W heater, I ended up using a 30A solid state after melting the first 15A mechanical. The TP-Link HS103 is rated for 12A and so far I have had        no issues.
  -  Dehumidifier/Humidifer - Works in the same way as Heater.
  -  Exhaust temperature/humidity - Controls an ehxaust and intake fan. You cannot use both control methods at the same time and        the buttons are interlocked. Humidity control can be used effectively to maintain a target VPD if temperature loads in the room are      under control. Exhaust maintains humidity, and set points on the heater are kept tight to maintain ideal temperature if required. The following chart is used for reference: https://pulsegrow.com/blogs/learn/vpd
  -  Exhaust interval - Hard-coded to only be applicable during night time if the button is active, and will trigger the fans every 30 minutes for the user specified timer. You can use this concept for any type of interval control and easily change it to your needs. Enter the set-point as "timer XXX" in seconds, ie. "timer 120" for 2 minutes.
  -  Lighting - Basic on/off control for lights and a display showing current time. The "timer disable" toggle is used to stop any scheduled based timer functionality. Manually on/off for timers does not rely on the timer disable button. Timer entries use a 24hr clock 00:00 format. 
  -  Fertigation - Scheduled feed pump control for up to 3 feeds. Timer disable, manual controls, and entry requirements are the same as described above. The feed will trigger once at the specified time for the duration and then be reset at midnight. Caution if you enter a new value and the current time has exceeded it, a feed will be triggered.
  
Future Plans
------------
  
  -  E-mail Alerts - Notification via email when measurements reach or exceed user-specified thresholds.
  -  Raspberi Pi Camera - A browser based remote live stream, image capture and time-lapse photography window.
  -  MySQL Database - Better historical data trending with Grafana integration: https://grafana.com/
  -  Ultrasonic Level - Simple display and alarming with an SRF04 sensor.
  -  PWM control - Variable speed fan control to maintain temperature/humidity. Potential use of an ESP32 as a remote fan control module.
  -  CO monitoring - MH-Z19B sensor integration.
  
Notes
--------
  -  DHT22s are unreliable at best, humidity typically needs a +/- adjustment of 10 against a known source to be accurate. Temperature is usually 1-2c off. Ruuvitags are ideal but require a Pi 3 or greater to work properly, for that reason I've provided links on how to implement a RuuviTag, but the code is currently using a DHT22. Do note exceed recommended wire length for the DHT. The function blocks prior to passing the data contain adjustments for both temperature and humidity that you will need to adjust for your application.
  -  If you plan on using a RuuviTag, make sure to do a full Node-Red install as per the following: https://nodered.org/docs/getting-started/raspberrypi, this is because pre-packaged Node-Red install on the pi is limited. It will work just fine for simpler DHT22/relay code, but i had problems when adding BLE scanning.

Instructions on how to use RuuviTags in Node-Red:
  -  https://lab.ruuvi.com/node-red/
  -  https://github.com/ojousima/node-red

Pictures
-----------

https://ledgardener.com/forum/viewtopic.php?f=35&t=5698

Using GardenBro
-----------------

  -  Once you have the Raspberry Pi booted into the Raspbian OS and you are connected to your WiFi network, perform your typical "sudo apt update" and "sudo apt full-upgrade" procedures in the terminal. Raspberry Pi comes with Node-Red preinstalled and it's perfectly fine if you plan on only using relays and a DHT, even WiFi plugs work, but for RuuviTags a full Node-Red install is required. Run the following command "bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)", for more details visit https://nodered.org/docs/getting-started/raspberrypi, it will:

    -  remove the pre-packaged version of Node-RED and Node.js if they are present
    -  install the current Node.js LTS release using the NodeSource. If it detects Node.js is already installed from NodeSource, it will ensure it is at least Node 8, but otherwise leave it alone
    -  install the latest version of Node-RED using npm
    -  optionally install a collection of useful Pi-specific nodes
    -  setup Node-RED to run as a service and provide a set of commands to work with the service

  -  Start up Node-Red and a terminal will be displayed, once NR is running have it automatically start on boot by entering "sudo systemctl enable nodered.service". Navigate to http://10.0.0.81:1880/ on any computer connected to your network and you should see the NR IDE. Simply import the code by copying the text file and get started, NR automatically broadcasts the dashboard at http://10.0.0.81:1880/ui.
  -  Adjust the Raspberry pi nodes for your connection points, don't forget the DHT22. All the pi nodes make this straightforward regardless of what pinout convention you use.
  -  To connect TP-Link HS103 WiFi plugs, first power on the device and go through the setup procedures using the Kasa app https://play.google.com/store/apps/details?id=com.tplink.kasa_android&hl=en_CA. Once setup on the plug is complete, turn the plug off on the app, and find the devices IP address however you see fit. Enter the IP address on the corresponding plugs node and it should immediately say "connected" once deployed.
  -  If you want to try out the RuuviTag simply follow the instructions in the provided links. They can explain it far better than me and example code is provided, import it just like you did with GardenBro.
  
Required Node-Red Modules
-----------------

The follwing node-red modules are required for the program to function. Use the palette manager and install the following:
  -  node-red-contrib-bigtimer
  -  node-red-contrib-dht-sensor
  -  node-red-contrib-tplink
  -  node-red-dashboard
  -  node-red-node-pi-gpio

RuuviTag related:
  -  node-red-contrib-noble
  -  node-red-contrib-ruuvitag

License
-------

GardenBro is a simple Node-Red program: you can redistribute it and/or modify it under
the terms of the GNU General Public License as published by the Free
Software Foundation, either version 3 of the License, or (at your
option) any later version.

GardenBro is distributed in the hope that it will be useful, but WITHOUT
ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public
License for more details.

A full copy of the GNU General Public License can be found at
http://www.gnu.org/licenses/gpl-3.0.en.html

This program uses third party open source software components.
Please see individual files for license information, if applicable.
