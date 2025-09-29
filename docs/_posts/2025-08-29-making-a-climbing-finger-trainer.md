---
layout: post
author: Zak Crane-Whatmore
comments: false
---

One challenge that I set myself over the summer was to design a climbing finger training board. Large versions of such boards are a common sight at climbing gyms everywhere and are used by climbers to increase their grip strength. I wanted to have a miniature, portable version which could be used at home and outside.

In the summer of 2022 I sourced the parts for and built an MPCNC primo CNC router which I used for a few projects such as designing gifts and creating some new house signs to make it a bit simpler for delivery drivers to find my house. Since then it had been used a couple of times, mainly to make parts for itself, such as some clamps to hold wood to the spoilboard without having to resort to using wood screws.

I thought that this would be the perfect opportunity to get the CNC router back out and use it on a new and slightly different project. I started the project by having a look at some existing training boards and saw what their features were. They typically had a few slots of depth ranging from 8mm to 30mm so my design should feature slots in this range. Sizing also varied widely between models, with some models featuring about 20 different points to hold onto and smaller versions having as low as 2 different points to hold onto. Mounting options also varied between different types of board with the larger models designed to be mounted to a solid beam and the smaller models designed to have rope threaded through them and can then be pulled on using a foot. 
<p align="center">
  <img src="https://climbinghouse.com/wp-content/uploads/2023/03/using_trango_rock_prodigy_training_center-1024x642.jpg" alt="SolidWorks drawing of finger board" title="SolidWorks drawing of finger board" width = "60%"/>
  <figcaption>Wall mounted finger board, source - https://climbinghouse.com/wp-content/uploads/2023/03/using_trango_rock_prodigy_training_center-1024x642.jpg</figcaption>
</p>

## Designing

After considering the existing designs I wanted to design a really small trainer that could feasibly be attached to a rucksack so that it can't be left behind. From one of my previous projects I had some 18mm cheap softwood board that I had picked up from a local DIY shop. I decided to use this to prototype the design, so something that I could suspend my weight off of was out of the question. My original idea, which didn't make it out of my head, was to have a polygon (most likely a pentagon) with a central hole and then a slot of varying depth on each of the sides. Unfortunately this would have been very large so wouldn't satisfy the requirement of an ultra portable trainer. I then iterated on this design, resulting in a triangular trainer, with a hole nearer the top for the rope and a single 12 mm deep slot which is the width of two fingers. I then modeled this up in SolidWorks and was ready to test the design.
<p align="center">
  <img src="/files/photos/2025-08-29/swdraw.png" alt="SolidWorks drawing of finger board" title="SolidWorks drawing of finger board" width = "60%"/>
  <figcaption>SolidWorks drawing of finger board</figcaption>
</p>

## Printing 

I began by 3D printing the design in PETG in order to have a feel for the design. This worked really well and I was pleased with the design, especially the filleted edges which made the design feel a bit more polished. Since I was satisfied with this test print I then moved on to working out how to do the CAM for the wooden version. For this I decided I would do a simple 2.5D version, with no filleted edges but with the pocket and hole to have a feel for the design in wood. To do this I exported an SVG file from SolidWorks of the design's outline and then imported this into EstlCAM and then manually defined the cutting depth for each section of the design. I was then ready to start up the CNC and cut this design out of wood.
<p align="center">
  <img align="top" src="/files/photos/2025-08-29/PETG.jpg" alt="PETG finger board" title="PETG finger board" width = "45%"/>
  <img align="top" src="/files/photos/2025-08-29/wooden25d.jpg" alt="2.5D wooden finger board" title="2.5D wooden finger board" width = "45%"/> 
  <figcaption>My PETG 3D printed and wooden milled finger boards</figcaption>
</p>

## Milling
The next stage of the project was to make a wooden trainer with filleted edges. In order to do this I required three things, a way to make a workpiece of a known size, a way to turn over this workpiece whilst ensuring that it is placed in the same position and a ball nosed end mill to better carve 3d surfaces. The first requirement was quite simple to achieve, I could simply cut rectangular pieces out of my larger block of wood and use these as the workpieces. The second required a little more thought, one method I had seen and quite liked was using a 90 degree angled cut out to hold one corner in place. This was compatible with the mounting system of wooden screw down clamps that I was previously using on the CNC router. I then purchased a ball nosed end mill on Amazon as I wanted something fairly cheap and quick to arrive. 

Whilst I was waiting for the new end mill to arrive I began work on the mounting corner. To make this I mounted two pieces of wood on the origin of the router and programmed the CNC to cut out two lines, one horizontal and one vertical starting at a distance of 20 mm from the origin, to give enough room to avoid collisions. Whilst the machine was cutting away the material I noticed that it would deflect when it began cutting away material. Unfortunately this meant that when it had finished the nice straight lines that I had planned on cutting out had not ended up so straight. Believing that this was down to some bolts loosening over time I tightened everything up but there was still a lot of play in the machine. Some research online revealed that the cause of this was that the recommended plastic for printing the MPCNC, PLA, is prone to creeping over time and deforming which lead to these loose parts. Since the only remedy to this would be to reprint most of the moving parts on the machine which would take a lot of time and material I decided to carry on as well as I could and try and rebuild the machine out of some stronger materials in the future.
<p align="center">
  <img src="/files/photos/2025-08-29/cornermill.jpg" alt="Milling the corner" title="Milling the corner" width = "60%"/>
  <figcaption>Milling out the mounting corner on the CNC</figcaption>
</p>
The Ball nosed end mill had arrived so my next step was to investigate true 3D milling using EstlCAM. For this I tried carving out of some foam which I had previously used for testing out carving house signs on the MPCNC. I wanted to start out by milling something simple so I quickly made a hemispheroid in OpenSCAD and opened it in EstlCAM, after watching and reading a couple of tutorials I had my gcode ready to go. Swapping out the end mill on the router head and mounting the foam meant that the CNC router was ready to go. I set the machine off and half an hour later had a pleasing result, a perfect hemispheroid carved out of some foam.
<p align="center">
  <img src="/files/photos/2025-08-29/hemispheroid.jpg" alt="Carved hemispheroid" title="Carved hemispheroid" width = "60%"/>
  <figcaption>Hemispheroid carved out of foam on the CNC</figcaption>
</p>
Happy with this result I then decided to move on to two sided milling. For this I decided to try a similar model but with some dimples cut out on each side to tell which side was which and hence tell if I had flipped the workpiece the correct way. Importing the model into EstlCAM generated a gcode file for each side and gave me the dimensions for the workpiece. Using the workpiece dimensions I was able to generate a third gcode file to carve out the workpiece. Running these programmes gave me a roughly carved shape.
<p align="center">
  <img align="top" src="/files/photos/2025-08-29/CNConeside.jpg" alt="One side carved" title="One side carved" width = "45%"/>
  <img align="top" src="/files/photos/2025-08-29/FinalHemispheroid.jpg" alt="Hemispheroid with dimples" title="Hemispheroid with dimples" width = "45%"/> 
  <figcaption>Second test object milled with the ball nosed end mill</figcaption>
</p>
With the two sided and 3D milling working correctly the final step of the project was to create a wooden finger board with filleted edges. I used the same procedure in EstlCAM as the test part to generate three gcode files for making the board. Whilst inspecting the CNC Router before milling, I noticed that the ball nosed end mill had blunted slightly. Despite this I decided to set the machine off anyway since I knew the finish wouldn't be perfect due to the other problems that I had encountered along the way. Three gcodes later and I had my result, unfortunately it was quite low quality due to the deflection of the machine, blunting of the end mill and difficulty in aligning the two sides due to the imperfect corner that I had milled out.
<p align="center">
  <img align="top" src="/files/photos/2025-08-29/twosideboard.jpg" alt="Two side CNC milled finger board" title="Two side CNC milled finger board" width = "45%"/>
  <figcaption>Two side CNC milled finger board</figcaption>
</p>

## Conclusion

Regardless of the difficulties that I encountered in this project and the disappointing final result I was pleased with the project overall, this is because I managed to make two mini finger training boards for myself, one out of PETG and one out of wood. I was also able to learn a lot about 3D milling and the difficulties involved with carrying it out. In addition to this I also have a list of improvements to make to my setup including but not limited to: 3D printing new more rigid parts, creating and mounting a better right angle solution, finding some higher quality end mills to use and eventually milling a final version out of some harder wood such as birch or poplar.