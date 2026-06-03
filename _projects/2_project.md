---
layout: page
title: Sensor Development
description: Signal conditioning, filtering, and digital sensor interfaces for an autonomous mechatronics robot
img: assets/img/Project2/allsensors.png
importance: 2
category: work
related_publications: false
---

This project focuses on the sensor development work behind our autonomous mechatronics robot, House of Slug. While the main robot project describes the full mechanical design, software architecture, and competition strategy, this page focuses on the circuits that allowed the robot to understand its environment and make real-time decisions.

The goal was to transform weak, noisy, or analog physical signals into reliable digital information that the robot's embedded software could use. These sensors helped the robot detect beacon towers, localize the drop-off area, stay inside the field boundaries, and respond when it hit an obstacle.

<div class="row justify-content-sm-center">
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project2/BeaconDet.jpg" title="Beacon detector testing" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project2/BeaconDet1ft.png" title="Close-distance beacon testing" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Beacon detector testing at different distances to verify signal strength, filtering, and rejection of unwanted frequencies.
</div>

## Project Overview

The robot needed multiple sensing systems to compete effectively. Each sensor had a specific purpose, but they all shared the same main challenge: the raw physical signal was not directly useful to the microcontroller. It had to be conditioned, filtered, amplified, and converted into a clean signal that the software could interpret.

The main sensor systems included:

- a beacon detector for identifying the ball-dispensing towers,
- a track wire detector for finding the drop-off door,
- black tape sensors for boundary detection and line following,
- and bumper sensors for obstacle detection.

Together, these circuits formed the connection between the robot's physical environment and its autonomous decision-making logic.

## Beacon Detector

The beacon detector was designed to identify the infrared signal emitted by the ball-dispensing towers. The target signal was centered around 2 kHz, while nearby unwanted signals at 1.5 kHz and 2.5 kHz had to be rejected.

To accomplish this, the circuit used a phototransistor, amplification, filtering, peak detection, and comparator-based digital conversion. The phototransistor converted the incoming light signal into an electrical signal. The signal was then amplified and passed through a bandpass filter designed to emphasize the desired beacon frequency while reducing nearby noise.

Once the signal was filtered, a peak detector converted the oscillating waveform into a more stable voltage level. A comparator then converted that voltage into a digital high or low signal that the robot could use in software. This allowed the robot to determine whether the beacon was present and use that information for orientation and navigation.

<div class="row justify-content-sm-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project2/IMG_0107.PNG" title="Beacon detector schematic" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Beacon detector schematic showing the signal-conditioning chain used to isolate and detect the desired beacon signal.
</div>

## Track Wire Detector

The track wire detector was used to help the robot locate the designated drop-off area. The field included a wire carrying an oscillating signal, which generated a magnetic field near the door. The robot used a coil-based sensor to detect this magnetic field.

The raw signal from the coil was weak, so it required multiple stages of conditioning. A resonant tank circuit provided passive amplification around the target frequency, and a non-inverting amplifier further increased the signal amplitude. After that, the signal passed through a peak detector and comparator so that the final output could be read as a digital signal by the robot's control software.

This circuit was important because it allowed the robot to transition from general navigation into the ball-depositing state when it detected that it was close to the correct door.

<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project2/TrackWireDEt.jpg" title="Track wire detector circuit" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Track wire detector prototype used to identify the drop-off region through magnetic-field sensing.
</div>

## Black Tape Sensor

The robot also needed to avoid leaving the valid field area. To help with this, we used black tape sensors to detect the boundary lines on the field. These sensors allowed the robot to recognize when it was approaching the edge and react before crossing out of bounds.

The sensor worked by detecting the contrast between the white field surface and the black tape. When the robot detected the tape, the software could trigger a boundary-handling behavior, such as stopping, reversing, turning, or correcting its heading.

This sensor was simple compared to the beacon and track wire circuits, but it was critical for reliable autonomous behavior. A robot that collects balls efficiently is not useful if it repeatedly leaves the field, so boundary detection became an important part of the robot's state machine.

<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project2/blktapeSensor.jpg" title="Black tape sensor" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Black tape sensor used for boundary detection and field-awareness during autonomous navigation.
</div>

## Bumper Sensors

The bumper sensors were used to detect physical contact with obstacles. Mechanically, the bumpers acted like switches. Electrically, they were designed so that a voltage would pass through only when a bumper was activated. This created a clean signal indicating that the robot had made contact with an obstacle.

This information was useful for collision handling. When a bumper was triggered, the robot could leave its current behavior and enter an obstacle-response state. For example, it could stop, reverse, turn, and then return to its previous task.

The bumper sensors were a good example of how simple hardware can become powerful when integrated properly into software. The circuit itself provided a digital contact signal, but the state machine determined how the robot should respond.

<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project2/BumperSensCKT.jpg" title="Bumper sensor circuit" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Bumper sensor circuit used to detect contact with obstacles and trigger collision-handling behavior.
</div>

## Signal Conditioning and Digital Interfaces

A major theme across all of these sensors was signal conditioning. The robot could not directly use the raw signals from the environment. The beacon, track wire, black tape, and bumper systems all required some form of conversion before the microcontroller could make decisions from them.

For the beacon and track wire detectors, the main challenge was extracting useful information from weak or frequency-dependent signals. This required filtering, amplification, peak detection, and comparator thresholds. For the black tape and bumper sensors, the goal was to create reliable digital behavior that could support fast decision-making.

The final result was a set of sensors that converted real-world events into software-readable signals.

## Integration With the Robot State Machine

The sensors were not isolated circuits; they were directly tied to the robot's autonomous behavior. Each sensor influenced a different part of the state machine.

The beacon detector helped the robot orient toward the ball towers. The track wire detector helped identify the door for depositing balls. The black tape sensor helped prevent boundary violations. The bumper sensors helped the robot respond to collisions and obstacles.

This integration was one of the most important parts of the project. A sensor is only useful if its output leads to the correct robotic behavior. Because of that, the circuit design and software design had to be tested together.

## Testing and Iteration

Testing was a major part of the development process. The beacon detector required careful tuning because it had to detect the correct 2 kHz signal while rejecting nearby frequencies. The track wire detector required testing at different distances and orientations to make sure the coil and amplification stages produced a usable signal. The black tape and bumper sensors required repeated field testing to make sure their digital outputs triggered the correct software response.

These tests helped me understand that sensor development is not just about building a circuit that works once. It is about building a circuit that works reliably when installed on a moving robot, under changing conditions, and as part of a larger autonomous system.

<div class="row justify-content-sm-center">
  <div class="col-sm-4 mt-3 mt-md-0">
    <video class="img-fluid rounded z-depth-1" controls>
      <source src="/assets/img/Project2/CloseDistanceBeacon.mp4" type="video/mp4">
      Your browser does not support the video tag.
    </video>
  </div>

  <div class="col-sm-4 mt-3 mt-md-0">
    <video class="img-fluid rounded z-depth-1" controls>
      <source src="/assets/img/Project2/BeaconDetectorFar.mp4" type="video/mp4">
      Your browser does not support the video tag.
    </video>
  </div>

  <div class="col-sm-4 mt-3 mt-md-0">
    <video class="img-fluid rounded z-depth-1" controls>
      <source src="/assets/img/Project2/SolderingVideo.mp4" type="video/mp4">
      Your browser does not support the video tag.
    </video>
  </div>
</div>

<div class="caption">
  Close-distance testing, far-distance beacon detection, and soldering work during sensor development.
</div>

<div class="caption">
  Sensor testing and soldering work during the development of the robot's sensing systems.
</div>

## What I Learned

This project strengthened my understanding of how sensing, analog circuits, and embedded software come together in robotics. I learned how to design and test signal-conditioning circuits, tune filters, convert analog behavior into digital signals, and integrate sensor outputs into autonomous decision-making logic.

More importantly, I learned that reliable sensing is one of the foundations of autonomous robotics. The robot's software can only make good decisions when the sensor signals are clean, consistent, and meaningful. This project helped me understand the importance of designing sensors not only as circuits, but as part of a complete robotic system.