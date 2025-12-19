---
title: Software
subtitle: Software design process and justification
image: assets/img/portfolio/softwarethumbnail.png
alt: Keep Exploring

caption:
  title: Software
  subtitle: Software design process and justification
  thumbnail: assets/img/portfolio/softwarethumbnail.png
---

### Initial goals:
- System-level integration of input and output devices, including motors, servos, LED displays, and microphones
- Cute and in-character behavior
- Idle states that run when no interaction is happening
- Recognition and response to the breastling melody from Silksong
- Ability to follow a treat (a Minecraft berry prop made of painted plywood)
- Response to darkness

We chose to use Python because of its ease of use and many libraries, except for a C++ script that sends data to arduino that controls all the hardwares. 

#### Code Structure and Design:
##### Behaviors
The core module that controls the robot is the behaviors class, which:
- Keeps track of and updates robot behaviors
- Sets the values of motor speed, ear and tail servo positions, and eye display matrices
- Converts these values to bytes and return a packet with expected format by the Arduino  

***

self.left_speed = 0  # -128 - 127   
self.right_speed = 0  # -128 - 127   
self.ear = 90  # 0–180   
self.tail = 60 # 0–120   
self.eye_brightness = 1    
self.left_eye = eye_display.EyeDisplay()   
self.right_eye = eye_display.EyeDisplay()   

***

In a seperate EyeDisplay class, we define the eye of 8*8 pixels with a list of 0s and 1s and imports it here.   
We began with some basic idle functions like sleep() and chase_tail() that sets these parameters to a certain value or state according to time and phase of the behavior. For example, in chase_tail(), its tail would first move to one side, then it spin at its tail's direction, and finally it gets dizzy eyes.  

At early stage, since the hardwares are not in place yet, we tested our code on a simulation script that prints out all robot parameters and the raw bytes it was sending out, so that we could test and debug our code as we were writing it, which prevents the risks of large-scale code reconstruction or underlying logic bugs that couldn't have been found without running.  
<img src="assets/img/portfolio/simulation.png" alt="Simulation script" width="100%" /><br>
 In fact, this helped a lot with our integration. Our script almost worked instantly when we first tested it on hardwares despite some arduino reading issues that are easy to fix.

##### Behavior Manager
As we started to add more idle states, more complex behaviors, and behaviors that only triggers with certain input signals, we realized that one class that controls both the behavior actions and robot states can be really long and messy. To make our code more readable and extendable, we split it into two classes: **Behaviors** and **BehaviorManager**.    
BehaviorManager, as its name suggests, manages robot states and behavior signals based on inputs such as button press(petted), melody detection and darkness. It also handles prioritization of behaviors, idle behavior loop and time tracking.   
The Behavior class receives a string that indicates the state that the robot should currently be in, and update robot parameters accordingly.

***

def update_behavior(self):   
&nbsp;&nbsp;&nbsp;&nbsp;self.state = self.manager.state   
&nbsp;&nbsp;&nbsp;&nbsp;match self.state:   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;case "run_petted":   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.behavior = lambda:self.petted(self.manager.eye_state)   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;case "run_look_for_treat":   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;elapsed = self.manager.now - self.manager.look_for_treat_start   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.behavior = lambda:self.look_for_treat(elapsed)   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;case "run_sleep":   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.behavior = self.sleep   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;case "blink":   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;elapsed = self.manager.now - self.manager.idle_start   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.behavior = lambda: self.blink(elapsed)   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;case "chase_tail":   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;elapsed = self.manager.now - self.manager.idle_start   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.behavior = lambda:self.chase_tail(elapsed)   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;case "wag_tail":   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.behavior = self.wag_tail   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;case "look_around":   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;elapsed = self.manager.now - self.manager.idle_start   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.behavior = lambda: self.look_around(elapsed)   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;case _:   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.behavior = self.default   

***

##### Audio Processing
There are three main tasks for audio processing: 
- Detect the pitch of notes in the audio
- Real-time audio collection and processing
- (Stretch Goal) Recognize the words or identify the speaker.

**Pitch finder**  
We used librosa library for finding the frequency and pitch of notes in the audio. It worked pretty well on saved .wav audio files, so we decided that our main task is getting it to run on real-time collected audio data.

**Real-time audio**  
One challenge of real-time audio processing is that while collecting data is fast and easy, processing it and detecting notes takes longer and will cause a noticible pause of robot. To address this, we continuously streams sound from the microphone and store small chunks of audio in memory, which doesn't affect the normal robot operation. Every few seconds, we save the recorded audio to a temporary file, process it and analyze the frequencies, and save the notes detected. Then we can comparing the notes to the given melody and send that signal to other modules.

**Word & Speaker recognition**  
Our stretch goal was to make the robot react to specific word commands, such as "spin" or "come here", as well as train a model for audio data on different people's voices. We explored on both of them but didn't end up applying it to the robot.
- Speech recognition: we already got it to work on our computer with calling Google speech recognizer API and it was able to run in the background without interfering the robot behaviors (see word_detection.py in our archive). However, when running it on Rasberry Pi, it turned out that the API requires exclusive microphone access, so we couldn't run it along with the melody detection script. We decided to keep our MVP goal and discarded this function.
- Speaker Identification: we did research on current machine learning algorithms for sound feature detection and recognition and found some simple ones that could be used. However, considering the difficulty to collect and label enough data, uncertainty of the training result on the microphone audio, and time restrictions, we didn't end up implementing it.

##### Object Detection
We wanted the robot to come to us when it hears the specific melody. Our initial thought was to use three microphones on the side of the robot, convert the amplitude of each microphone's reading to distance, and calculate the position of the sound source based on the distances. However, the microphone quality was worse than we expected and the difference between the three microphones are not significant enough for accurate math calculation.   
To address that, we decided to instead use a camera so that the robot can see what's around it and follow a targeted thing -- in our case, a red berry. We used OpenCV to detect the color and when it sees a block of red it returns the position of its center. The robot will then move to it.
<img src="assets/img/portfolio/opencv.png" alt="OpenCV demo" width="100%" /><br>

#### Fine tune and test
At final stage, when all the hardwares were assembled, we tested the code on the robot and fine tuned the parameters. For example, the tail servo cannot move to its theoretically biggest angle because it would hit the mounting, and the motor speed for wiggling also needed to be adjusted carefully so that it didn't just go straight or turn to one side.  
<img src="assets/img/portfolio/testing.jpg" alt="testing and fine tuning demo" width="100%" /><br>
