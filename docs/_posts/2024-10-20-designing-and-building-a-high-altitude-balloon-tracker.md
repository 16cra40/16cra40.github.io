---
layout: post
author: Zak Crane-Whatmore
comments: false
---

Over Trinity term I was involved in re starting the Oxford Aerospace and Rocketry Society. The plan for the first term was to launch, track and recover a helium filled high altitude balloon before heading onto more advanced projects. The group was split into different sub groups covering different areas such as avionics, payload, balloon, parachute and recovery. I was on the avionics team whose role was to track the balloon from launch to recovery. 

## Planning

We started out by researching all of the existing options on the market for tracking a HAB (high altitude balloon). The different options which we considered were a pre built SSB radio tracker, a kit LoRa radio tracker and a GSM tracker (designed for tracking stolen cars). The SSB radio tracker was discarded, since none of us had the valid radio licence for operating an amateur radio. Research into the GSM tracker revealed that although a cheap option for tracking the HAB they would often fail in flight and would not work at altitude due to their reliance on the mobile phone network. This left the LoRa tracker which could be purchased as an assembled unit or built from parts. Since our funding was low we opted for the cheaper option and purchased the parts and assembled the system ourselves.

The recommended parts list for the tracker was: a Raspberry Pi Zero, Case, Raspberry Pi Camera, UBlox GPS, LoRa module and an SD card. Additionally at least one receiver was also required. For the receiver we required a microcontroller and another LoRa module. Since the guide for building the tracker and receiver was a few years old we decided to use the newer Raspberry Pi Zero 2 W. The recommended microcontroller for the reciever was an arduino however, we decided to use a Raspberry Pi Pico since some of us had more experience with it.

## Initial Testing 

After assembling the trackers and receivers the next step was to test them. This was where the issues began. The tracker and receiver refused to talk to each other which was a challenging issue to debug since we didn't have a display cable for the Raspberry Pi Zero 2 W (HDMI to Mini HDMI). This meant that we couldn't get find the source of the problem. As a first step for debugging, I disconnected the GPS module and connected it up to one of the Pi Picos which I used as a serial repeater to print the GPS module's output to a terminal. This revealed that the GPS module was working and getting a fix on some satellites. At this point it became clear that the project would not be launching before the end of term giving me the opportunity to design my own HAB tracker using the Raspberry Pi Picos. 

The next step was to test out the LoRa modules, I purchased some breakout boards for the LoRa modules, allowing for them to be plugged into a breadboard, along with the picos and GPS module. Using a [LoRa Library](https://github.com/sandeepmistry/arduino-LoRa) and the Arduino IDE I wrote a simple transmitter and receiver Hello World programme to test out the LoRa modules. After soldering a quarter wave antenna (17cm for 434 MHz) to the LoRa module I was able to verify that they both worked. Now that I had verified all of the components worked the next step was to write the tracking code. 

## Designing

The tracking code had to achieve two objectives, read the incoming GPS messages and transmit position and other data over LoRa. For the LoRa transmission there is a standard message, called the [UKHAS standard sentence](https://ukhas.org.uk/doku.php?id=communication:protocol). This has the format: 

`$$CALLSIGN,sentence_id,time,latitude,longitude,altitude,optional speed,optional bearing,optional internal temperature*CHECKSUM\n`

The GPS module outputs data using the NMEA specification, this includes information such as the position, velocity and time. Fortunately there is a library available for Arduino which is able to parse NMEA messages,[TinyGPSPlus](https://github.com/mikalhart/TinyGPSPlus/), returning the information contained within them. The GPS also had to be set to flight mode which allows it to operate at higher altitudes than the standard settings. Once the GPS data has been received, it was converted to a UKHAS sentence and the checksum of this is then calculated using [CRC](https://github.com/RobTillaart/CRC). This checksum can then be appended to the end of the sentence before it is transmitted over LoRa at the UK maximum power of 10mW.

The receiver was much simpler, since the UKHAS messages are human readable, for the first iteration of the program simply listened on the LoRa module for any incoming messages and printed them out, along with their signal strength. I tested this out on my desk with the tracker and receiver placed either side of my laptop and was pleased to see that it worked. 

<p align="center">
  <img src="/files/photos/2024-10-20/FirstMessage.jpeg" alt="Successful first message" title="Successful first message" width = "60%"/>
  <figcaption>Successful first message</figcaption>
</p>

## Range testing 

My next objective was to test the range of this setup. I began by testing in the garden and found that I could still receive messages from the bottom of the garden. A trip to the coast followed and revealed that the bottom of the garden was about the limit for this setup as I only managed to achieve about 30m of range. Researching designs for LoRa antennae showed that attaching two quarter wavelength wires to ground and positioning them perpendicularly to the antenna creates a fairly good ground plane, increasing range. 

Now that I had a working prototype circuit I then moved onto designing a PCB for the tracker. I used KiCad to draw the schematic for the tracker circuit, from this I was able to design a PCB for the tracker, with a large ground plane to hopefully help with transmission range. Since the receiver used the same circuit as the tracker minus the GPS I was able to get a single design manufactured, reducing cost.

<p align="center">
  <img src="/files/photos/2024-10-20/PCB.jpeg" alt="Manufactured PCBs" title="Manufactured PCBs" width = "60%"/>
  <figcaption>Manufactured PCBs</figcaption>
</p>

Once the tracker and receiver had been soldered, assembled, and the antennae mounted on cardboard, I was able to test the range further. For this I opened up the map, identified some nearby hills and set off into the South Downs with some help for field testing. We started on Old Winchester Hill and started by walking down the hill until the connection was lost, unfortunately we hit the bottom of the hill before we lost connection so we needed another plan. I then chose some nearby hills with Ordnance Survey Trig Points on them: Beacon Hill, Riversdown and Old Winchester Hill. Since they were trig points they could all be seen from each other, meaning that the tracker and receiver would have line of sight. We then tested the range using various combinations of these trig points and found the maximum range of the tracker to be about 4.7 Km. From what we had read online this should translate to being able to track an entire balloon flight.

<p align="center">
  <img src="/files/photos/2024-10-20/Field.jpeg" alt="First range test" title="First range test" width = "60%"/>
  <figcaption>First range test</figcaption>
</p>

## Uploading
[SondeHub](https://sondehub.org) is a website for tracking high altitude balloon flights and has a site for tracking amateur balloon flights. Since it would be useful for people without laptops and receivers to be able to track the balloon I decided to try and upload the data logged by the receiver to the website. Fortunately SondeHub provide a simple Python API for uploading data to their amateur website. They also have a developer mode so any testing doesn't show up on the actual map.

Since the receiver was already set up to just print the UKHAS sentence over serial, it was simple enough to read this from a Python script using [pySerial](https://pythonhosted.org/pyserial/) so no modification was necessary to the receiver. The next stage was splitting the sentence back up into its constituent parts. Once the sentence was split up it could then be verified using the checksum. Then, using the SondeHub API can be used to upload all of the flight information. I also quickly created a Python GUI with Tkinter and [TkinterMapView](https://github.com/TomSchimansky/TkinterMapView) which displays the information received from the tracker and plots the position of the HAB on a map. With this complete I was able to test the full set up and was able to verify that the tracker and receiver could work together to upload the position of a HAB to SondeHub.

<p align="center">
  <img align="top" src="/files/photos/2024-10-20/SondeHub.jpg" alt="SondeHub view" title="SondeHub view" width = "22%"/>
  <img align="top" src="/files/photos/2024-10-20/HABTrackAPP.png" alt="HABTrack Python GUI" title="HABTrack Python GUI" width = "45%"/> 
  <figcaption>Tracker on SondeHub and Python GUI</figcaption>
</p>

## Conclusion

I then shared my final version of the project with other members of the society who were able to build and test some of these devices for range and energy consumption. This meant that we were now ready to launch a HAB. Unfortunately due to delays in other areas of the project, logistical issues and Autumn / Winter fast approaching the launch had to be shelved. Despite not launching, the project formed a foundation for the avionics on further projects within the society so the effort was not wasted. I was really pleased with the project as I was able to develop my skills in Arduino programming, PCB design, Python programming and wireless communications to build a solution from the ground up for a real world problem. All code and PCB files can be found on the [GitHub repository](https://github.com/OxfordAerospaceAndRocketrySociety/HighAltitudeBalloon).

<p align="center">
  <img src="/files/photos/2024-10-20/Sunset.jpeg" alt="Range testing at sunset" title="Range testing at sunset" width = "60%"/>
  <figcaption>Range testing at sunset</figcaption>
</p>
