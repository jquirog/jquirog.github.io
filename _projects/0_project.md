---
layout: page
title: Hybrid Robotic Arm Contact Control
description: Hybrid modeling, contact detection, force regulation, Lyapunov analysis, and MATLAB simulation of a robotic arm
img: assets/img/Project0/phase_portrait.png
importance: 0
category: work
related_publications: false
---
<div class="row justify-content-sm-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    <p>
      Access the full technical paper here:
      <a class="btn btn-sm btn-outline-primary" href="/assets/pdf/UnofficialPublication1/HybridModelingandControlofaRoboticArm.pdf" target="_blank">
        📄 View Paper
      </a>
    </p>
  </div>
</div>

This project presents the hybrid modeling and control of a robotic arm operating under multiple control modes depending on contact interaction. The system was modeled using state-triggered hybrid logic, simulated in MATLAB with the HyEQ Toolbox, and analyzed for solution behavior, Lyapunov stability, and repeatable task execution.

The goal was to model and control a robotic arm performing a sequence of tasks that includes approaching an object, establishing contact, regulating contact force, releasing the object through a throwing motion, and resetting the arm to its initial configuration. Modeling this system using hybrid dynamics enabled the explicit representation of both continuous evolution and discrete transitions.

## Project Overview

In many robotic manipulation tasks, physical interaction with the environment plays a critical role. Robotic arms often operate in settings that require alternating between free motion and constrained motion, such as assembly, packaging, or object manipulation. These interactions naturally introduce abrupt transitions in the system dynamics when the arm makes or breaks contact with a surface.

Traditional continuous-time models are not well suited to handle these discrete, event-driven changes, which motivates the use of hybrid dynamical modeling. In this project, I developed a hybrid control framework for a one-degree-of-freedom robotic arm interacting with a compliant environment.

The robotic arm was modeled as a hybrid system with four discrete modes:

- position control before contact,
- force control during contact,
- throwing motion after force regulation,
- and ballistic reset back to the initial state.

## System Description

The robotic arm was constrained to motion along a horizontal axis and interacted with a compliant environment modeled using a Kelvin-Voigt contact model. The task consisted of four phases: free motion until contact detection, contact-phase force regulation, throwing motion using a specific velocity profile, and reset to the original arm position.

The continuous state vector was defined as position and velocity, while the full hybrid state included a discrete mode variable representing the current control phase. Contact detection was based on force thresholds, and the system used hysteresis-based switching logic to avoid undesirable chattering caused by measurement noise or threshold crossings.

## Hybrid System Modeling

The hybrid behavior of the robotic arm was modeled through a state-triggered system with four discrete modes. Each mode had its own continuous dynamics and control objective.

In position control mode, the arm was not in contact with the environment. A PD controller drove the end-effector toward a desired position. This allowed the arm to approach the contact region smoothly while damping velocity to reduce overshoot.

In force control mode, contact had been established. The objective was to regulate the contact force to a desired value. The controller used feedback to reduce the error between the actual contact force and the desired force.

In throwing mode, the system applied a sinusoidal forcing input. Unlike the stabilizing modes, this phase intentionally injected energy into the system to generate a release motion.

In reset mode, the system entered a passive ballistic phase governed by gravity. The arm returned to a known reset region, preparing the system to begin the next manipulation cycle.

## Mode Transitions and Jump Logic

The discrete transitions between modes were determined by contact force, velocity, and position thresholds. The jump map switched the system from position control to force control when contact force exceeded the contact threshold. Once the desired force was reached within a tolerance band, the system transitioned into the throwing phase. When the release velocity threshold was reached, the system moved into the reset phase. Finally, once the arm returned to the reset position, the state was reset and the cycle began again.

This structure allowed the system to repeat a complete pick-throw-reset sequence while maintaining clear separation between each control objective.

<div class="row justify-content-sm-center">
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project0/position_plot.png" title="Position vs hybrid time" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project0/velocity_plot.png" title="Velocity vs hybrid time" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Position and velocity trajectories over hybrid time. The plots show the repeated cycle of approach, contact regulation, throwing, and reset.
</div>

## Phase Portrait and Discrete Mode Behavior

The phase portrait shows the system trajectory in the position-velocity plane. The trajectory curvature reflects the control mode transitions, and the start of the trajectory is marked to show the initial condition.

The discrete mode plot confirms that all four modes were reached during the simulation and that the system correctly reset to begin a new cycle. These results validate the hybrid control structure's ability to manage physical contact, force regulation, dynamic transitions, and repeatable reset behavior.

<div class="row justify-content-sm-center">
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project0/phase_portrait.png" title="Phase portrait" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project0/mode_plot.png" title="Discrete mode vs hybrid time" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Phase portrait and discrete mode evolution showing the hybrid trajectory and repeated switching between position control, force control, throwing, and reset.
</div>

## Lyapunov Stability Analysis

To analyze stability, I defined mode-specific Lyapunov candidate functions and studied their evolution along system trajectories.

For position control mode, the Lyapunov candidate was based on kinetic energy and position error. Its derivative was non-positive during flows, confirming that the function decreased while the system approached the desired pre-contact configuration.

For force control mode, the Lyapunov candidate was based on the force tracking error. The objective was to show that the actual contact force converged toward the desired contact force. Simulation confirmed that the force-control Lyapunov function decreased and that its derivative remained negative or zero during the relevant flow intervals.

For throwing mode, no Lyapunov function was proposed because the purpose of that mode was to inject energy into the system. Since the system was not intended to converge to an equilibrium during this phase, a standard Lyapunov decrease condition would not be meaningful.

For reset mode, a Lyapunov-like function was defined around the reset target. Although this function was not strictly decreasing at all times, it reflected the system's passive return toward the pre-contact configuration.

<div class="row justify-content-sm-center">
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project0/V_plot.png" title="Lyapunov function in mode q = 0" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project0/Vdot_plot.png" title="Lyapunov derivative in mode q = 0" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Lyapunov function and derivative during position control mode q = 0.
</div>

<div class="row justify-content-sm-center">
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project0/V1_plot.png" title="Lyapunov function in mode q = 1" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project0/V1dot_plot.png" title="Derivative of V1 in mode q = 1" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Force-control Lyapunov function and derivative during mode q = 1.
</div>

<div class="row justify-content-sm-center">
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project0/V3_plot.png" title="Lyapunov function in mode q = 3" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project0/V3dot_plot.png" title="Derivative of V3 in mode q = 3" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Reset-mode Lyapunov-like function and derivative during mode q = 3.
</div>

## Decrease Across Jump Transitions

In hybrid systems, stability must be evaluated not only during continuous flows but also across discrete jumps. To verify this behavior, I implemented MATLAB-based post-processing of the hybrid simulation data. The analysis evaluated the Lyapunov value immediately before and after each jump transition.

For the transition from position control to force control, the analysis confirmed a strict decrease in Lyapunov value across jumps. This supported the hybrid Lyapunov condition for the contact transition.

The reset transition from mode q = 3 to q = 0 was also analyzed. The results showed that the Lyapunov value typically decreased or remained approximately constant across the transition. This behavior aligned with the physical interpretation of the system resetting its state after ballistic return and supported repeatability of the hybrid task cycle.

<div class="row justify-content-sm-center">
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project0/jump_V_before_after.png" title="V before and after q = 0 to q = 1 jumps" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project0/jump_delta_V.png" title="Change in Lyapunov across q = 0 to q = 1 jumps" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Lyapunov value before and after jumps from q = 0 to q = 1, showing consistent decrease across the contact transition.
</div>

<div class="row justify-content-sm-center">
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project0/jump_30_V_before_after.png" title="V before and after q = 3 to q = 0 jumps" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project0/jump_30_delta_V.png" title="Change in Lyapunov across q = 3 to q = 0 jumps" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Lyapunov value before and after reset jumps from q = 3 to q = 0, supporting bounded and repeatable hybrid trajectories.
</div>

## Simulation

To analyze the behavior of the proposed hybrid control scheme, I implemented the model in MATLAB using the HyEQ Toolbox. The system was simulated over a hybrid time domain with a maximum of 25 flow seconds and 20 jumps. The initial condition was set so the arm began in free motion mode, far from the contact surface.

The simulation demonstrated that the full cycle from approach to contact, throw, and reset was completed robustly and repeatably. The system moved through all four modes, regulated contact force, executed the throwing phase, and returned to the initial condition.

## What I Learned

This project strengthened my understanding of hybrid dynamical systems, especially how continuous dynamics and discrete logic can be combined to model robotic systems with contact. I learned how to define flow maps, jump maps, flow sets, and jump sets, and how to use MATLAB simulation to analyze hybrid trajectories.

It also helped me understand why hybrid systems are important in robotics. Contact, impacts, resets, and mode-dependent control laws are difficult to represent with purely continuous models. Hybrid modeling provides a structured way to describe these behaviors and verify that the system remains well-posed, repeatable, and practically stable.

## Conclusion

This project presented a hybrid control framework for a robotic manipulator executing a pick-throw-reset cycle. By modeling the system as a hybrid dynamical system with four discrete modes, I designed and analyzed mode-specific control laws using Lyapunov-based techniques.

For the position regulation and force control phases, Lyapunov candidate functions were proposed and their decrease during flows was confirmed through numerical simulations. Decrease across discrete transitions was also verified, satisfying hybrid Lyapunov stability conditions for the analyzed jumps. The simulation results demonstrated that hybrid systems theory enables formal verification of complex robotic behaviors involving continuous dynamics, discrete mode transitions, contact regulation, impacts, and resets.
