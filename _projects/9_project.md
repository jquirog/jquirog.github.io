---
layout: page
title: FPGA Timer Game
description: Verilog-based Basys 3 FPGA timing game using counters, state machines, seven-segment display control, and button input logic
img: assets/img/Project9/DiligentGame.png
importance: 9
category: work
related_publications: false
---

This project presents a Verilog-based timing game implemented on a Digilent Basys 3 FPGA board. The design was developed in Vivado for CSE 100 and focused on digital logic design, finite state machines, counters, button input handling, arithmetic modules, and seven-segment display control.

The goal of the game was to create an interactive hardware system where the player uses the Basys 3 buttons and switches to control the game behavior while the FPGA updates the display in real time. Unlike a software game running on a processor, this project was built from digital hardware modules written in Verilog.

<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project9/DiligentGame.png" title="Basys 3 FPGA timing game" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Basys 3 FPGA implementation showing the game output on the seven-segment display and user interaction through the board controls.
</div>

## Project Overview

This project was created as **Lab 5** in Vivado. The archived project included the synthesis and implementation runs, meaning the design was taken beyond only Verilog writing and was prepared for FPGA deployment. The project used a Basys 3 constraints file and included a full set of source modules for counters, arithmetic, state-machine logic, display selection, and top-level integration.

The design brought together several hardware-building blocks, including:

- a top-level Verilog module,
- a finite state machine,
- four-bit and multi-bit counters,
- arithmetic modules,
- shifters,
- edge detection,
- ring-counter display multiplexing,
- selector logic,
- and hexadecimal seven-segment decoding.

Together, these modules created a complete interactive FPGA system.

## Top-Level Design

The `TopLevel` module connected the user inputs, internal game logic, and physical board outputs. It handled the Basys 3 buttons and switches, routed signals between the state machine and counters, and controlled the seven-segment display outputs.

This top-level integration was one of the most important parts of the project. Each smaller module performed a specific task, but the full game only worked when the modules were connected correctly and synchronized through the FPGA clock.

## State Machine Logic

The project included a dedicated `StateMachine.v` module. This module controlled the main game behavior by determining which state the game was currently in and how the system should respond to player input.

Using a finite state machine made the game behavior more organized because the system could move through defined phases rather than relying only on independent combinational logic. This helped manage transitions, timing behavior, button events, and display updates.

The state machine was especially important because FPGA designs are naturally parallel. Without clear state-based control, it becomes difficult to coordinate when counters should run, when values should update, and when the display should change.

## Counters and Timing

The project used several counter modules, including `FourBitCounter.v`, `TurkeyCounter.v`, and timing support from `qsec_clks.v`. These counters were used to track values, generate timing behavior, and update the game state.

A central part of FPGA game design is controlling time directly in hardware. Instead of using software delays, the design used clocked counters that advanced based on FPGA clock signals. This gave the system deterministic timing and allowed the display and game logic to update predictably.

## Arithmetic and Data Path Modules

The project also included arithmetic modules such as `Adder.v`, `Add8.v`, and `sub8.v`. These modules were used to perform simple arithmetic operations inside the hardware design.

Although the arithmetic operations were small, they were important because they showed how higher-level behavior can be constructed from low-level digital circuits. The game logic could update values, compare conditions, and change outputs based on arithmetic results generated directly in hardware.

The `Shifter.v` module added another data-path component by allowing values to be shifted, which is a common operation in digital systems.

## Button Input and Edge Detection

The Basys 3 board buttons were used as player inputs, but physical button signals can remain high for multiple clock cycles. If the design responded directly to the raw button signal, one press could accidentally be counted multiple times.

To solve this, the project used an `Edge_Detector.v` module. The edge detector converted a button transition into a clean single-cycle pulse. This allowed the game to respond once per button press instead of continuously while the button was held down.

This was a key lesson in hardware design: physical inputs need to be synchronized and conditioned before they are used by the rest of the system.

## Seven-Segment Display System

The game output was displayed using the Basys 3 seven-segment display. The display system used several modules working together:

- `RingCounter.v` selected which digit was active,
- `Selector.v` chose the correct four-bit value for that digit,
- `Hex7.v` converted the value into seven-segment signals.

Because the Basys 3 display is multiplexed, the FPGA rapidly cycles through the digits so that they appear continuously lit to the user. This project helped me understand how display multiplexing works and how timing logic can control multiple physical outputs with shared signal lines.

## Demonstration

<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    <video class="img-fluid rounded z-depth-1" controls>
      <source src="/assets/img/Project9/TimerDiligentGame.mp4" type="video/mp4">
      Your browser does not support the video tag.
    </video>
  </div>
</div>

<div class="caption">
  Demonstration of the FPGA timing game running on the Basys 3 board.
</div>

## Vivado Implementation

The project was built in Xilinx Vivado and included both synthesis and implementation runs. This means the design was not only simulated conceptually, but also prepared for actual FPGA hardware deployment.

The archived project included the source files, simulation file, and Basys 3 constraint file. The constraint file mapped the Verilog signals to the physical FPGA pins, allowing the design to interact with the board's buttons, switches, LEDs, and seven-segment display.

## What I Learned

This project strengthened my understanding of FPGA design and hardware-based game logic. I learned how to break a system into reusable Verilog modules, connect them through a top-level design, manage player inputs, build state-machine behavior, use counters for timing, and display information through multiplexed seven-segment outputs.

More importantly, this project helped me understand the difference between software thinking and hardware thinking. In Verilog, modules operate concurrently, timing is controlled by clock edges, and every signal connection matters. Building this game helped me become more comfortable designing systems that run directly on digital hardware.

## Conclusion

This FPGA timer game was a complete digital design project involving Verilog, Vivado, Basys 3 hardware, counters, finite state machines, arithmetic modules, button handling, and seven-segment display control.

The project gave me practical experience developing an interactive hardware system from modular Verilog source files and deploying it on an FPGA board. It also served as a foundation for later VGA-based and embedded digital systems projects.
