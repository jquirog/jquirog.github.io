---
layout: page
title: House of the Slug
description: Autonomous scooping robot with hierarchical state machines, system integration, and mechatronic design
img: assets/img/project7/FotoParaWeb1.png
importance: 1
category: work
related_publications: false
---

House of Slug was my team's final project for Mechatronics: an autonomous scooping robot designed to navigate a structured competition field, collect randomly dispensed balls, avoid obstacles, and strategically deposit the balls at a designated door. The project combined mechanical design, embedded software, power distribution, sensing, and real-time decision-making into one integrated robotic system.

As the Electrical Lead and Software Sub-lead, I focused on the systems that allowed the robot to operate autonomously: the control logic, state-machine structure, electrical integration, power distribution, and the software/hardware interfaces that connected the robot's mechanical behavior with its sensor feedback. I discuss the individual sensor circuits in more depth in a separate project page, so this page focuses on the full robot design and how the different subsystems worked together.

<div class="row justify-content-sm-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/project7/FotoParaWeb1.png" title="House of Slug robot during testing" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  House of Slug during testing on the competition field.
</div>

## Project Overview

The goal of the competition was to build a robot that could collect as many balls as possible within a limited time and deposit them at the correct door. The balls were released from towers at random intervals, and the robot had to collect, store, transport, and release them while staying inside the field boundaries and responding to obstacles.

This made the project much more than a simple driving robot. The final design had to behave as a complete autonomous system. It needed to make decisions based on field position, ball-collection status, obstacle interactions, timing, and drop-off detection. By the end of the project, our robot collected 37 out of 40 balls and deposited 33 out of 40 balls within the two-minute competition window.

## Mechanical and System Design

The mechanical design went through several iterations as we balanced ball capacity, maneuverability, robustness, and the 11-inch cube size constraint. Early versions explored different collection approaches, including a bristled roller, but we eventually moved toward a rubber-band-based intake system because it was more reliable for pulling balls into the robot and less likely to tangle.

The final design used a front intake for ball collection, internal storage for carrying balls, and a controlled dispensing mechanism for releasing them at the target door. We also considered how the internal geometry of the robot affected ball movement. A tilted floor and assisted internal motion helped guide balls toward the dispensing area and reduced the chance of clogging or jamming.

A key lesson from this part of the project was that the mechanical design and software logic could not be developed independently. The software depended on predictable mechanical behavior, while the mechanical system had to support the states and actions defined in the control logic.

## Electrical Integration and Power Distribution

Because the robot combined motors, sensors, actuators, and a microcontroller, reliable electrical integration was essential. My work included helping organize the electrical system so the robot could distribute power safely and consistently across the different subsystems.

The power distribution board and wiring layout were important because the robot had to support multiple loads while remaining compact and serviceable. Clean wiring, accessible connections, and reliable power delivery made debugging easier and reduced the chance of intermittent failures during testing.

Rather than treating the electronics as separate circuits, we designed the electrical system around the needs of the robot as a whole: sensing, movement, ball handling, and timed actuation all had to work together under one control structure.

## Software Architecture

The robot's autonomy was built around a hierarchical state-machine architecture. This allowed us to divide the robot's behavior into clear modes instead of writing one large, difficult-to-debug control loop.

At the highest level, the robot transitioned between major behaviors such as navigation, ball collection, obstacle response, door localization, and ball dispensing. Each behavior had its own logic, entry conditions, exit conditions, and motor or actuator commands.

This structure made the system easier to understand and improve. When the robot failed during testing, we could identify whether the issue came from a state transition, a timing condition, a motor command, or a sensor interpretation. This made debugging much more systematic.

## Autonomous Decision-Making

The robot had to respond to changing field conditions in real time. It could not simply follow a fixed path because balls were dispensed randomly, obstacles could affect its movement, and the robot had to determine when it was in the correct position to deposit balls.

To manage this, the state machine used sensor feedback to decide when to keep collecting, when to correct orientation, when to avoid boundaries, and when to transition into the dispensing sequence. Encoder-based motion correction helped the robot execute turns more consistently, while the higher-level state logic ensured that each action supported the overall competition objective.

This project taught me that autonomous software is not just about writing commands for motors. It is about designing a decision-making structure that can handle uncertainty, recover from imperfect behavior, and keep the robot progressing toward its goal.

## Embedded Software and Hardware Interaction

The embedded software connected the robot's decision-making logic to the physical system. We developed software routines for motor control, sensor reading, actuator timing, and hardware interaction. These routines made it easier to manage the robot's behavior from the state machine without rewriting low-level control code repeatedly.

This modular approach helped us test individual functions, then combine them into larger autonomous behaviors. It also made the code easier to modify during late-stage testing, when small timing or movement adjustments could make a major difference in competition performance.

## Testing and Iteration

A major part of this project was repeated testing. Each test revealed something new about the robot's behavior: how well it collected balls, how consistently it turned, whether the dispensing mechanism released balls cleanly, and whether the state transitions happened at the right moments.

Many improvements came from observing small failures and making targeted changes. Mechanical friction, ball jams, timing issues, sensor thresholds, and motor response all affected performance. The final robot was the result of many iterations across mechanical design, electrical integration, and software logic.

<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    <video class="img-fluid rounded z-depth-1" controls>
      <source src="/assets/img/project7/mechVideo.mp4" type="video/mp4">
      Your browser does not support the video tag.
    </video>
  </div>
</div>

<div class="caption">
  Testing the robot's autonomous behavior on the competition field.
</div>

## Leadership and Team Role

As Electrical Lead and Software Sub-lead, I helped guide the development of the robot's control and integration systems. My role involved coordinating how the electrical design, software structure, and mechanical behavior connected into one functioning platform.

This required both technical work and team communication. I helped organize the system-level design, debug interactions between hardware and software, and make sure that design changes supported the robot's overall objective. The project was a strong example of interdisciplinary engineering because every decision affected multiple parts of the robot.

## What I Learned

House of Slug strengthened my understanding of mechatronic system design. I gained experience developing embedded C state machines, integrating hardware with software, coordinating power and signal flow, and building an autonomous robot that had to operate reliably under competition constraints.

More importantly, the project taught me how important system integration is in robotics. A good mechanical design, a good circuit, or a good software module is not enough by itself. The robot only works when all of those pieces support each other. This project helped me understand how to move from individual subsystems to a complete autonomous machine.

## Related Work

A separate project page will explore the sensor development in more detail, including the beacon detector, track wire detector, filtering, amplification, and signal-conditioning circuits used to support the robot's autonomous behavior.

