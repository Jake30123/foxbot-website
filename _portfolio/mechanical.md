---
caption: #what displays in the portfolio grid:
  title: Mechanical
  subtitle: subtitle
  thumbnail: assets/img/portfolio/Mechanical Design Icon.JPG
  
#what displays when the item is clicked:
title: Mechanical
subtitle: subtitle lorem ipsum dolor sit amet consectetur.
image: assets/img/portfolio/Original Drawing.JPG #main image, can be a link or a file in assets/img/portfolio
alt: Keep Exploring

---

# Mechanical Overview

The main goals for the mechanical design of the Fox-Bot were to be easy to take apart and put back together, and to be visually fox-like and cute. To achieve the first goal we made a modular skeleton out of lasercut plywood. All joints are pressfit or secured with bolts, to ease disassembly and access to the inside electronics. Plywood was also chosen to be cost effective and sustainable. To achieve the second goal we built moving ears and tail, hid the camera in the nose, and covered the entire Fox-Bot in faux-fur. We’ve broken our design process into six main aspects which we’ll cover here.


## Chassis

The chassis is designed to be easily taken apart in order to access the interior workings. The Fox-Bot is driven by two 12V DC motors mounted on the baseplate, with two castor wheels on the front and back. We originally were going to design and manufacture our own caster wheels, but decided against it because of how much time it would take. The caster wheels are embedded higher into the baseplate in order to match the height of the main wheels. We also had to sand the motor mounts a bit to reach the desired height. The design of the lasercut chassis stayed mostly the same throughout our design process, and we were able to add attachment points where necessary.

<img src="assets/img/portfolio/Overall Mech Design Flow.png" alt="Fox-Bot General Design Flow" width="100%" /><br>

## Face

The design of the face had to include mounting for the 2 displays we used for eyes, and eventually a mount for the camera. In continuing the goal of ease of assembly, the eyes are simply pressfit into holes cut into plywood. Once we pivoted to using a camera, the camera mount was added into the center of the face, and disguised as a nose/snout.

## Electrical Management

The management of the electrical component was played around with near the beginning of the project as a unified platform to house the arduino, motorshield and battery, but we quickly realized that would not work because the DC motors for the wheels cut right through the center of the body. This led to a two platform design, with a simple plywood housing for the battery on one side of the Fox-Bot and a simple 3D printed platform on the other. 

<img src="assets/img/portfolio/Electrical Management Full.png" alt="Fox-Bot PCB and Battery Housing" width="100%" /><br>

Two screws are tightened up through the baseplate and holes in the raspi to screw into the poles of the PCB platform. The Arduino is then secured with a separate set of bolts into the platform. This set up had the added benefit of balancing the chassis. 

We also implemented wiring notches in the top and bottom the chassis supports to help with the internal wire management of the Fox-Bot

<img src="assets/img/portfolio/Wire Notch on Support.png" alt="Fox-Bot Wire Notches on Support" width="100%" /><br>

## Ears

The ears were designed to be rotated above the head by two servo motors, and be simple enough to cover in fur. A structure to support the servos is press fit into the top. We used small servos provided by our class because they were free and we didn't need much tourque to move the ears, but were more constarined by space.

<img src="assets/img/portfolio/Ear CAD.png" alt="Fox-Bot Ear CAD" width="100%" /><br>

## Button

The goal of the button is to allow the user to directly interact with the Fox-Bot. It was designed with ease of activation and ease of integration in mind. Electrically the Fox-Bot was already doing a lot, so that is why there is not a pressure sensor or some other sensor to detect someone petting the head of the Fox-Bot. The solution is two buttons.

<img src="assets/img/portfolio/Button CAD.png" alt="Fox-Bot Button CAD" width="100%" /><br>
<img src="assets/img/portfolio/Button Live Image.jpg" alt="Fox-Bot Button Live Image" width="100%" /><br>

Above is the CAD rendering of the top plate and large button (both laser cut plywood) and the real life implementation. The small blue button is a real electrical component,  but doesn’t meet the requirement that a user can pet anywhere on the top of the Fox-Bot and queue a response. We chose to use this component because we found it for free within our classroom's excess materials. The large and the top plate that have suspended dowels running through both, and springs around those dowels. This guides the button directly down when it is pressed and keeps it from moving on the XY plane. The springs wood glued directly to the button and top plate keep the large button from dislodging. Whenever the large button is pressed, even slightly, it will apply pressure to the small button and queue the Fox-Bot to respond.

This design makes integration easier and relies on a very simple and robust electric component, versus a potentially finickier sensor.

## Tail

The tail of the Fox-Bot is and was 3D printed from PLA. This manufacturing process was chosen for its affordability, and because it was easy to prototype. The tail went through two major phases. The first one was an attempt to install a cable drive system that would use a gear to simultaneously move the tail and curl the tail. These were the first iterations of the tail with that design in mind (note the holes in the sides of each component). 

<img src="assets/img/portfolio/Tail V1 to V2.png" alt="Fox-Bot Tail version one and two" width="100%" /><br>

This idea was quickly abandoned as too complicated for such a small subsystem, and would require springs to be integrated into the cable system to keep it taut. We opted for a simpler modular design that would curl slightly when swung back and forth via the momentum of the tail. The components were also made cylindrical, and filling was added to remove the clanking noise created when the PLA collided with itself as the tail swung. This significantly reduced the tails ability to curl, but considering the size of the fur that would later cover it this trade off was worth it. 

<img src="assets/img/portfolio/Tail V3 to Final.png" alt="Fox-Bot Tail version 3 and final" width="100%" /><br>

The motor supports (which just press fit into the Fox-Bot for even more modularity) for the servo remained relatively the same through the two phases, although the whole platform was raised to reduce the dragging created with the ground.

## Aesthetics:

“Yap about fur”

<img src="assets/img/portfolio/All Fox Fur Parts.jpg" alt="Skinned Fox-Bot" width="100%" /><br>

The other purely aesthetic part we created was a 3D rendition of the Minecraft Sweet Berry. 

<img src="assets/img/portfolio2/Fox-Bot Sweet Berry Live Image.jpg" alt="Our Sweet Berry" width="100%" /><br>

We made it out of plywood and painted it. The color of the berries is a brighter and more consistent red than that of the actual videogame artifact, but that decision was made with the computer vision in mind. One, brighter red color makes tracking easier.

<img src="assets/img/portfolio/MC Sweet Berry.jpg" alt="Actual MC Sweet Berry" width="100%" /><br>