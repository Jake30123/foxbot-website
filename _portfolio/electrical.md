---
title: Electrical
subtitle: Lorem ipsum dolor sit amet consectetur.
image: assets/img/portfolio/electrical.jpg
alt: Keep Exploring

caption:
  title: Electrical
  subtitle: Electrical Design Justification
  thumbnail: assets/img/portfolio/electrical.jpg
---

#### Electrical Overview
The electrical system of the Fox-Bot was designed to be both functional and clean. The electrical system needs to facilitate all necessary software tasks and power all of the systems while not interfering with the cute looks of the bot. It should also be well organized and easy to follow for debugging. 

#### Initial Design Choices
Going into the team set the following electrical goals:
1. Clean and tidy
2. Integrated with the mechanical system from the start
3. Contained (does not interfere with cuteness)
4. Sufficiently powerful to power all components without dropouts 
5. Powerful enough to run machine learning

The first big choices on the table were how to do computing, how to control motors and servos, and how to power the system. 

##### Computing Demands
Running both machine learning processes and computer vision at once requires a lot of computing power and powerful software available. Many Single Board Computers (SBCs) exist on the market but many are either overpriced or underperforming for our budget and needs. To compare our options we made the following decision matrix:

| Criteria    | Weight (%) | Raspberry Pi 5 | NVIDIA Jetson | Raspberry Pi 4 | Orange Pi 5 |
| ----------- | ---------- | -------------- | ------------- | -------------- | ----------- |
| Cost        | 20         | 7              | 4             | 8              | 4           |
| Performance | 20         | 8              | 10            | 4              | 10          |
| Support     | 5          | 6              | 6             | 6              | 3           |
| Community   | 15         | 10             | 8             | 10             | 6           |
| Size        | 5          | 10             | 5             | 10             | 8           |
| Power Draw  | 15         | 7              | 4             | 7              | 6           |
| Ease of Use | 20         | 8              | 7             | 8              | 7           |
| Final Score | 100        | 7.95           | 6.55          | 7.35           | 6.55        |

*Note: The Arduino Uno Q would have been an absolutely perfect option but it would not have arrived in time.* 

##### Motor/Servo Control
The Raspberry Pi 5 cannot directly control and power servos or motors so we needed an intermediate to facilitate this control. In earlier PIE projects we learned how to use Arduino's and Adafruit Motor Shields, and since fancy motor control was not in the scope of the project or in our learning goals we opted to continue with this method of control. PIE also has a spare motor shield we could borrow which made it an easy choice given that our team also had multiple Arduino R4s are our disposal at the beggining (*foreshadowing*). 

##### System Power
All the components we selected have to now be powered. All together, the *max* power draw of the system is as follows:

Raspberry Pi 5 = 15W (3A @ 5V) <br>
Arduino Uno R4 Minima = 0.5W (90mA @ 5V) <br>
Three Servos = 3W (600mA @ 5V) <br>
Two 8x8 Matrix Displays = 3W (600mA @ 5V) <br>
Two 12V Drive Motors = 72W (6A @ 12V) <br>

**Total 5V = 4.29A *Needs to be stepped down from 12v*** <br>
**Total 12V = 2.4A *Including 12V converter theoretical draw***

We had a 12V battery easily at our disposal from PIE stock. It was a 12V 2.2AH Li-Po battery that has seen better days (*foreshadowing #2*). Also, we needed a 12V system anyway to power the motors.

We found buck converters in the electrical stock room that could only supply 3A max, meaning that at full draw our robot would be power limited. Also, the Raspberry Pi 5 is incredibly sensitive to power fluctuations (*foreshadowing #3*) so having a dedicated buck converter makes the system more reliable and cleaner. So we ended up with a 12V battery and 2 buck converters to power the entire robot.

#### Initial Work/Integration




