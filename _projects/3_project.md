---
layout: page
title: UART Communication Protocol
description: Interrupt-driven UART device driver, circular buffers, and packet-based serial communication on a PIC32
img: assets/img/Project3/diagram.png
importance: 2
category: work
related_publications: false
---

This project focused on building a UART-based serial communication system between a PIC32 microcontroller and a computer interface. The goal was to move beyond simple character transmission and develop a more complete embedded communication stack using device-driver initialization, interrupt-driven data transfer, circular buffers, packet parsing, checksum validation, and command handling.

The final system allowed the computer-side Python interface to communicate with the PIC32, set and retrieve LED states, and exchange Ping/Pong messages through a structured packet protocol. This project strengthened my understanding of how low-level hardware communication connects to higher-level software protocols.

<div class="row justify-content-sm-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project3/diagram.png" title="UART protocol stream flow diagram" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  High-level stream-flow diagram showing how packets move between the application layer, protocol layer, UART driver, and PIC32 hardware.
</div>

## Project Overview

The project was developed as part of a microcontroller systems design lab focused on serial device drivers and protocol communication. The PIC32 implemented an interrupt-driven, full-duplex UART link to a remote computer. Through this link, the system could receive commands from a Python interface, process structured packets, and send responses back to the host computer.

The work was divided into three main stages: configuring UART communication on the PIC32, implementing interrupt-driven circular buffers for transmit and receive data, and building a packet protocol for application-level commands.

This structure made the project a strong introduction to how embedded systems separate low-level byte movement from higher-level message processing.

## UART Device Driver

The first step was configuring the UART peripheral on the PIC32. This involved setting the baud rate, enabling transmission and reception, and verifying that the board could communicate with a computer terminal. The UART configuration used a standard serial setup with 8 data bits, no parity, one stop bit, and a baud rate of 115200.

At this stage, the goal was to prove that the hardware interface worked correctly. A simple loop checked whether new data had arrived, read the received byte, and echoed it back through the UART. This provided a basic but important foundation before adding interrupts, buffers, and packet handling.

## Interrupt-Driven Communication

After validating basic UART operation, the next step was to make communication interrupt-driven. Instead of constantly polling for every character, the system used interrupt service routines to respond when UART events occurred.

When a byte arrived, the receive interrupt moved it into a software buffer before it could be lost. When the transmitter was ready, the transmit interrupt pulled data from the outgoing buffer and placed it into the UART transmit register. This approach made the system more responsive and allowed the application code to work with buffered data instead of directly managing every individual hardware event.

## Circular Buffer Design

Circular buffers were used to manage the incoming and outgoing byte streams. This was important because UART data can arrive asynchronously, and the software needs a safe place to store bytes until the application is ready to process them.

The buffer design used a head pointer, a tail pointer, and status flags to determine whether the buffer was empty or full. New bytes were enqueued at the tail, while processed bytes were dequeued from the head. When either pointer reached the end of the buffer, it wrapped around to the beginning, allowing the same fixed-size memory region to be reused continuously.

<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project3/Buffer.png" title="Circular buffer diagram" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Circular buffer structure used to manage asynchronous UART data without losing incoming or outgoing bytes.
</div>

## Packet Protocol

Once the byte-level UART driver and buffers were working, the project moved to packet-based communication. Instead of treating every byte as an isolated character, the protocol grouped bytes into structured packets.

Each packet contained fields such as a header, length, message ID, payload, tail, checksum, and end characters. This structure made it possible to send meaningful commands rather than raw characters. For example, one packet could request the current LED state, another could set LEDs, and another could carry a Ping message that required a Pong response.

The packet format also made the system more robust because the receiver could check whether a complete and valid message had arrived before acting on it.

## State Machine Packet Parser

To build valid packets from the UART byte stream, I implemented the packet parser as a state machine. The parser moved through states such as header detection, length reading, message ID detection, payload collection, tail validation, and checksum verification.

This design made the parser easier to reason about and debug. If a malformed packet arrived, the state machine could reject it and return to the initial state without corrupting the rest of the communication flow. This was an important lesson in reliable embedded protocol design: serial data may arrive one byte at a time, but the software must still reconstruct and validate complete messages.

<div class="row justify-content-sm-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project3/StateMachine.png" title="Packet parsing state machine" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Packet-building state machine used to identify headers, collect payloads, validate packet structure, and confirm checksums.
</div>

<div class="row justify-content-sm-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project3/StateC.png" title="BuildRxPacket state machine implementation" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  C implementation of the packet parser state machine inside BuildRxPacket(), where incoming UART bytes are processed through the head, length, ID, payload, tail, and checksum states.
</div>

## Checksum and Data Integrity

A checksum was used to verify that packet data had not been corrupted during transmission. The sender calculated a checksum from the packet contents and included it in the message. The receiver then recalculated the checksum after receiving the packet and compared the two values.

If the calculated checksum matched the transmitted checksum, the packet was considered valid and could be processed. If not, the packet was rejected. This added a layer of reliability to the protocol and helped prevent incorrect commands from being executed due to malformed data.

## Application Commands

At the application level, the protocol supported commands such as setting LEDs, reading LED states, and performing Ping/Pong communication. These commands demonstrated how a structured serial protocol can connect a computer application to hardware behavior on a microcontroller.

The Ping/Pong functionality was especially useful because it confirmed two-way communication. The computer sent a Ping packet, the PIC32 processed the message, calculated the required response, and sent back a Pong packet. If the response was correct, the Python interface confirmed that communication was working properly.

## What I Learned

This project helped me understand embedded communication as a layered system. At the lowest level, the UART peripheral moves bytes between the PIC32 and the computer. Above that, circular buffers protect against data loss and allow asynchronous communication. Above the buffers, the packet parser reconstructs meaningful messages. Finally, the application layer interprets those messages and performs actions such as controlling LEDs or responding to Ping requests.

The project also strengthened my understanding of interrupt service routines, state machines, buffer management, packetization, and data validation. More importantly, it showed me that reliable embedded communication depends on both hardware configuration and careful software architecture.

## Related Work

This project connects to my broader embedded systems and robotics work because reliable communication is essential in robotic platforms, sensor systems, and distributed control. The same ideas used here — buffering, packet parsing, state machines, and data validation — are useful whenever microcontrollers need to communicate with computers, sensors, or other embedded devices.
