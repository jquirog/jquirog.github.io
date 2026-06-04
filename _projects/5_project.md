---
layout: page
title: Convergence of IoT and Industrial Automation Research Paper
description: Technical literature review on IIoT, PLC networks, industrial protocols, and unified communication frameworks
img: assets/img/Project5/FigureIV.png
importance: 3
category: work
related_publications: false
---

This page presents a brief overview of my research paper, **Convergence of IoT and Industrial Automation Protocols: Toward Unified Communication Frameworks**, completed for ECE 842: Performance Modeling of Communication Networks at Michigan State University.

The paper studies how industrial systems are evolving as the Industrial Internet of Things is integrated with existing automation technologies. Modern factories, warehouses, and vehicles increasingly combine PLC networks, industrial Ethernet, wireless sensor networks, cloud-based services, and low-latency communication protocols. Because each technology has different assumptions about latency, reliability, scalability, and integration with higher-level control, building unified communication frameworks remains an important engineering challenge.

<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project5/FigureIV.png" title="Web-server-based centralized HMI architecture" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Web-server-based centralized HMI architecture for IoT-ready PLC systems, used in the paper as an example of industrial automation moving toward networked and unified communication.
</div>

## Research Paper Overview

In this paper, I reviewed five recent works that study the convergence of industrial automation and IoT from different perspectives: PLC-based automation in warehousing, centralized HMI for heterogeneous PLCs, embedded sensor networks in industrial settings, IoT and cloud-based extensions to automation systems, and low-latency communication schemes for time-sensitive industrial applications.

The goal was not only to summarize each paper, but also to identify recurring technical problems and solution patterns. Across the reviewed works, several themes appear repeatedly: legacy PLC integration, real-time communication requirements, web-server or gateway-based architectures, cloud connectivity, sensor-network scalability, and the need for reliable low-latency protocols such as TSN, EtherCAT, PROFINET IRT, and related industrial Ethernet technologies.

The paper concludes that future unified industrial communication frameworks will likely combine legacy PLC and fieldbus networks, deterministic Ethernet on the factory floor, IoT gateways, and application-layer standards that move data toward cloud platforms while still preserving the timing, reliability, and safety requirements of industrial systems.

<div class="row justify-content-sm-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    <p>
      Access the full research paper here:
      <a class="btn btn-sm btn-outline-primary" href="/assets/pdf/UnofficialPublication2/ConvergenceofIoTandIndustrialAutomation.pdf" target="_blank">
        📄 View Paper
      </a>
    </p>
  </div>
</div>