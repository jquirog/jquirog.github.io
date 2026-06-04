---
layout: page
title: Temperature Sensor PCB
description: Custom LM35 temperature sensing circuit, PCB design in Cadence Allegro, and FSM-based thermal protection
img: assets/img/Project4/Top.png
importance: 3
category: work
related_publications: false
---

This project focuses on the development of a custom temperature sensor PCB designed to monitor environmental conditions and protect sensitive electronics from unsafe thermal conditions. The design was part of a larger embedded sensing system, but this page focuses only on the temperature sensor circuit, PCB design process, and thermal protection logic.

The goal was to build a reliable temperature sensing board from scratch: selecting the sensing approach, designing the analog circuit, creating the schematic, laying out the PCB in Cadence Allegro, validating mechanical constraints, and connecting the sensor outputs to an embedded finite state machine.

<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project4/TempSensorDiagram.png" title="Temperature sensor circuit schematic" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Temperature sensor schematic using two LM35 configurations, signal conditioning, and negative voltage generation for sub-zero temperature measurement.
</div>

## Project Overview

The temperature sensor was designed to monitor ambient conditions and protect temperature-sensitive components in the system. A major requirement was the ability to detect both positive and negative temperatures while still interfacing safely with a microcontroller ADC, which can only read positive voltages.

To accomplish this, I developed a dual-sensor LM35 circuit. One LM35 was configured for standard positive-temperature readings, while a second LM35 was configured for full-range temperature measurement, including sub-zero conditions. Since the full-range configuration can output negative voltages, the circuit also required power conditioning and signal conditioning so the microcontroller could interpret the measurement correctly.

This project gave me hands-on experience with analog circuit design, component selection, PCB layout, mechanical constraints, and embedded temperature-based decision logic.

## Circuit Design

The core of the circuit uses two LM35 temperature sensors. The first sensor measures positive ambient temperatures using the standard centigrade configuration. The second sensor is configured for full-range operation, allowing the circuit to detect temperatures below 0°C.

Because the full-range LM35 can generate a negative voltage, I included a TC1044S charge pump to create a negative voltage rail from the available +5V supply. This made it possible for the LM35 to operate correctly in the negative temperature range.

However, the microcontroller ADC could not directly read negative voltages. To solve this, I used a unity-gain inverting op-amp stage to convert the negative LM35 output into a positive signal that the ADC could measure. This allowed the embedded system to read positive temperatures from one sensor and infer sub-zero temperatures from the conditioned output of the second sensor.

The circuit also included decoupling and noise mitigation capacitors to improve signal stability and reduce measurement noise.

## PCB Design in Cadence Allegro

After validating the circuit concept, I designed the PCB using Cadence Allegro. This part of the project focused on translating the schematic into a manufacturable and testable board.

The PCB design process included:

- placing the LM35 sensors, op-amp, charge pump, capacitors, resistors, and connectors,
- checking component footprints against manufacturer specifications,
- routing analog signal paths carefully,
- separating power and signal traces where appropriate,
- designing accessible connector locations,
- and validating mechanical mounting holes and drill sizes.

This project helped me develop a stronger understanding of how electrical design decisions affect physical layout. Component placement was especially important because the board needed to be compact while still leaving enough space for routing, soldering, and testing.

<div class="row justify-content-sm-center">
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project4/Top.png" title="Top PCB layer" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project4/Bot.png" title="Bottom PCB layer" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Top and bottom PCB layers showing routing, component placement, connectors, and signal paths for the temperature sensing board.
</div>

## Mechanical Constraints and Fabrication Planning

In addition to the electrical layout, the board had to satisfy mechanical constraints. I designed the mechanical layer to define the board outline, mounting holes, and drill locations. This ensured that the PCB could be physically integrated into the larger system and mounted securely.

The mechanical layer was also useful for checking whether connectors and components would be accessible after installation. This step reinforced the importance of designing PCBs not only as electrical circuits, but also as physical parts that must fit inside a real system.

<div class="row justify-content-sm-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project4/Mech.png" title="Mechanical layer and drill chart" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Mechanical layer and drill chart used to verify board outline, mounting points, and fabrication requirements.
</div>

## Temperature Monitoring Logic

The sensor board was designed to interface with a microcontroller through ADC inputs. The embedded software reads both temperature channels and selects the appropriate signal depending on the measured range.

For positive temperatures, the system uses the standard LM35 output. For negative temperatures, the system uses the conditioned output from the full-range LM35 and inverting op-amp stage. This allows the embedded system to monitor a wider temperature range than a single basic LM35 configuration would provide.

The temperature readings are then evaluated by a finite state machine that determines whether the system should remain in normal operation, activate a fan, or trigger a shutdown.

## Finite State Machine

The temperature control logic was implemented as a finite state machine. The FSM begins by reading the current temperature and comparing it against defined thermal thresholds.

At normal temperatures, the system remains in a low-risk state and keeps the fan off. If the temperature enters an elevated-risk range, the system activates the fan and checks the temperature more frequently. If the temperature becomes critically high or critically low, the system transitions into a shutdown state to protect sensitive components.

This FSM-based structure made the thermal response predictable and easy to debug. It also helped avoid unnecessary fan activation while still responding quickly to dangerous temperature conditions.

<div class="row justify-content-sm-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project4/TempFSM.png" title="Temperature finite state machine" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Temperature finite state machine showing how readings trigger fan activation, normal monitoring, or system shutdown.
</div>

## Testing and Validation

After the PCB design was completed, the circuit was fabricated, populated, and tested. Testing focused on verifying that the board could correctly measure both positive and negative temperature conditions and that the conditioned output remained compatible with the ADC input range.

The testing process included checking power rails, verifying sensor outputs, confirming that the charge pump generated the required negative voltage, and validating that the op-amp stage produced a readable positive signal for sub-zero measurements.

This step was especially valuable because it connected the design work in Cadence Allegro to real hardware behavior. It showed how schematic design, footprint selection, routing, soldering, and embedded software all affect the final performance of a sensor board.

## What I Learned

This project strengthened my PCB design skills and gave me practical experience developing a custom sensing circuit from scratch. I learned how to design around sensor requirements, condition analog signals for microcontroller compatibility, create a manufacturable PCB layout, and consider mechanical integration during the design process.

More importantly, the project showed me that a successful sensor board is not just a circuit that produces a voltage. It must be accurate, reliable, physically manufacturable, easy to integrate, and useful to the embedded software that depends on it. Designing this temperature sensor helped me better understand the full path from physical measurement to electrical signal to embedded decision-making.

## Related Work

This temperature sensor PCB was part of a larger embedded sensing system for environmental monitoring. Other parts of the system included LiDAR data acquisition, file storage, and higher-level embedded control, but this page focuses specifically on the custom temperature sensor circuit and PCB design.
