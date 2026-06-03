---
layout: page
title: Inverted Pendulum
description: Modeling, simulation, control design, and embedded implementation of a self-balancing cart-pendulum system
img: assets/img/FreeBodyDiagramPenguin.png
importance: 1
category: work
related_publications: false
---

The inverted pendulum is one of the classic problems in control systems because it is naturally unstable: without control, the pendulum falls away from the upright position. For this project, my team and I designed, modeled, simulated, and experimentally tested an inverted pendulum system built on a mobile cart platform. The goal was to stabilize the pendulum around the upright equilibrium by deriving the system dynamics, designing a controller, validating the response in MATLAB, and translating the control strategy into embedded C for real-time implementation.

<div class="row justify-content-sm-center">
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/FreeBodyDiagram.png" title="Free-body diagram" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/FreeBodyDiagramPenguin.png" title="Project pendulum diagram" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Initial system model and project-specific diagram used to represent the cart-pendulum dynamics.
</div>

## Project Overview

This project began with the physical modeling of a cart-pendulum system. I derived the equations of motion using energy methods and Lagrange’s equations, starting from the cart position, pendulum angle, rod length, cart mass, pendulum mass, and gravity. From there, I linearized the nonlinear dynamics around the upright equilibrium so the system could be represented in state-space form and used for controller design. My handwritten derivation included the kinematic constraints, kinetic and potential energy expressions, Lagrangian formulation, equations of motion, linearization, and conversion into a state-space model. :contentReference[oaicite:0]{index=0}

The physical system was built as a cart with an upright pendulum attached to it. Instead of a traditional pendulum mass, our prototype used a small penguin figure at the top, which made the system more visually recognizable while preserving the same control challenge. The final system combined mechanical design, sensor feedback, motor actuation, embedded programming, and control theory.

<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/portada.png" title="Physical inverted pendulum prototype" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Physical prototype of the inverted pendulum system mounted on a mobile cart.
</div>

## Dynamic Modeling and State-Space Representation

After defining the system geometry, I modeled the motion of the pendulum mass relative to the moving cart. This involved expressing the pendulum mass position, differentiating it with respect to time, and using those results to compute kinetic and potential energy. With the Lagrangian approach, I obtained the coupled equations that describe how the cart motion and pendulum angle influence each other.

The nonlinear equations were then linearized around the upright position using small-angle assumptions. This allowed the system to be written in a state-space form suitable for modern control design. The final state-space model included the pendulum angle, angular motion, cart position, and cart velocity. The model also incorporated the actuator dynamics of the DC motor, including how input voltage produces force on the cart. :contentReference[oaicite:1]{index=1}

## Control Design and Simulation

Once the state-space model was derived, I implemented the system in MATLAB to analyze controllability, convert the continuous-time model into a discrete-time model, and design a digital controller. The system was discretized with a sampling time of 0.05 seconds, corresponding to a 20 Hz control loop. :contentReference[oaicite:2]{index=2}

I used an LQR-based control approach to place the closed-loop behavior in a stable and practical region. The controller was tested under different initial pendulum angles to evaluate how quickly the system could recover and how much control effort was required. The MATLAB simulations included root locus analysis, frequency-domain checks, closed-loop state response, control effort plots, and animated visualizations of the pendulum motion. :contentReference[oaicite:3]{index=3}

<div class="row justify-content-sm-center">
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Response Full State.png" title="Full-state feedback response" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Response estimator.png" title="Estimator response" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  MATLAB simulations showing pendulum angle stabilization, cart position response, and control effort over time.
</div>

## Embedded Implementation

After validating the control strategy in simulation, the next step was to move from MATLAB into embedded implementation. This required translating the controller logic into C and adapting it to run on a physical microcontroller. The experimental system introduced challenges that do not appear in an ideal simulation, such as sensor noise, actuator limits, timing constraints, calibration errors, and differences between the mathematical model and the real hardware.

This part of the project was especially valuable because it connected theory with real-world behavior. I learned how a controller that works well in simulation must be adjusted carefully before it can work on hardware. The project helped me better understand the importance of sampling time, sensor feedback, actuator saturation, and experimental debugging.

## Experimental Learning

One of the most important parts of this project was seeing how the system behaved outside the simulation environment. The physical prototype helped me understand the gap between mathematical models and real engineering systems. Small changes in friction, mass distribution, sensor readings, and motor response could significantly affect performance.

Through testing, I gained experience debugging both the control logic and the physical system. I also learned how to compare simulated response plots with experimental behavior, identify possible sources of error, and improve the implementation step by step. This made the project more than a control theory exercise; it became a complete mechatronics project involving modeling, simulation, embedded systems, and hands-on experimentation.

## What I Learned

This project strengthened my understanding of how to take a control system from theory to implementation. I learned how to derive equations of motion, linearize nonlinear dynamics, build a state-space model, design a digital controller, simulate closed-loop performance, and implement control logic on embedded hardware. It also gave me practical experience working with an unstable system, where small modeling or implementation errors can quickly affect performance.

More importantly, the project taught me that engineering design is iterative. The mathematical derivation, MATLAB simulation, embedded code, and physical testing all informed each other. Each stage revealed something new about the system and helped improve the final implementation.

## Team

This project was completed as part of the CircuitsDuSoleil inverted pendulum team with Marlon Brewer, Julio Quiroga Galan, Christopher Mcleod, and Dylan Ford. :contentReference[oaicite:4]{index=4}
