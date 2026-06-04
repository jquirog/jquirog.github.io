---
layout: page
title: Robust H∞ Pitch Control Applied to a Fixed-Wing Unmanned Aerial Vehicle
description: Robust H-infinity mixed-sensitivity control, uncertainty modeling, and MATLAB simulation for UAV pitch dynamics
img: assets/img/Project6/figure1.png
importance: 6
category: work
related_publications: false
---

This project presents my research paper, **Robust H∞ Pitch Control Applied to a Fixed-Wing Unmanned Aerial Vehicle**, completed for ECE 854: Robust Control at Michigan State University. The project studies the design and evaluation of a robust H∞ controller for the pitch-attitude dynamics of a fixed-wing UAV.

<div class="row justify-content-sm-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    <p>
      Access the full technical paper here:
      <a class="btn btn-sm btn-outline-primary" href="/assets/pdf/UnofficialPublication3/RobustHinfPitchControlApplied.pdf" target="_blank">
        📄 View Paper
      </a>
    </p>
  </div>
</div>

## Project Overview

This project is based on the paper **“Robust H∞ Control Applied on a Fixed Wing Unmanned Aerial Vehicle,”** where robust longitudinal and lateral controllers are developed for a quarter-scale Piper J3-Cub UAV. In my project, I focused specifically on the longitudinal elevator-to-pitch channel to reproduce the main robust control ideas in a clear and organized way.

The objective was to design a robust controller that could stabilize the nominal longitudinal plant, achieve fast and well-damped pitch-angle tracking, maintain acceptable closed-loop behavior under plant uncertainty, and reduce sensitivity to measurement noise and modeling errors.

## Modeling and Uncertainty Description

The longitudinal UAV model was implemented in MATLAB using a state-space representation. The input was the elevator deflection, and the controlled output was the pitch angle. The state vector included perturbations in body-axis velocity components, pitch rate, and pitch angle.

To account for model uncertainty, selected aerodynamic derivatives and control-effectiveness terms were modeled as uncertain real parameters. A 20% uncertainty level was applied to selected terms in the state-space matrices. This uncertainty model allowed the controller to be evaluated against a family of possible pitch-channel plants instead of only one nominal model.

## Robust H∞ Control Method

A mixed-sensitivity H∞ formulation was used to synthesize the controller. The controller was designed to balance three main objectives: tracking performance, control effort, and robustness to uncertainty and noise.

The weighting functions were selected based on the pitch-control case reported in the reference paper. In this formulation, the performance weight shaped low-frequency tracking behavior, the control-effort weight limited excessive elevator actuation, and the noise/complementary-sensitivity weight helped reduce high-frequency amplification.

The controller was synthesized in MATLAB using the `mixsyn` command from the Robust Control Toolbox.

## Nominal Closed-Loop Response

The nominal closed-loop system was stable and achieved fast pitch tracking. The step response was well damped, had no overshoot, and reached the desired pitch angle quickly. The main time-domain characteristics were a rise time of approximately 0.2398 seconds, a settling time of approximately 0.5776 seconds, zero overshoot, and a peak value of 0.9749.

<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project6/figure1.png" title="Nominal closed-loop step response" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Nominal closed-loop step response for the UAV pitch angle, showing fast tracking and no overshoot.
</div>

## Frequency-Domain Analysis

The frequency-domain behavior of the nominal closed-loop system was also evaluated. The nominal closed-loop transfer function showed strong attenuation at high frequencies, which is desirable for limiting the effect of high-frequency uncertainty and measurement noise.

The sensitivity and complementary sensitivity functions were analyzed to understand the tradeoff between disturbance rejection and bandwidth. The sensitivity function remained small at low frequencies, supporting good tracking and disturbance rejection, while the complementary sensitivity rolled off at high frequencies, helping attenuate noise and unmodeled dynamics.

<div class="row justify-content-sm-center">
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project6/figure2.png" title="Frequency response of nominal closed-loop system" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project6/figure3.png" title="Nominal sensitivity and complementary sensitivity" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Frequency response, sensitivity, and complementary sensitivity plots used to evaluate tracking, robustness, and noise attenuation.
</div>

## Uncertain Closed-Loop Simulations

To evaluate uncertain performance, twelve samples of the uncertain plant were generated and closed with the same H∞ controller. All twelve uncertain closed-loop systems remained stable, and their trajectories stayed close to the nominal response.

This result showed that the controller provided good robustness with respect to the modeled parameter variations, even though the formal robust-performance test later showed that the selected performance objectives were not guaranteed across the entire uncertainty set.

<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project6/figure4.png" title="Nominal and perturbed closed-loop step responses" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Nominal and perturbed closed-loop step responses for the pitch angle. The uncertain samples remain stable and close to the nominal response.
</div>

## Control Effort and Noise Transmission

The control effort for a unit step pitch reference was also analyzed. The elevator command showed a strong initial transient and then decayed smoothly toward its steady value. This confirmed that the controller achieved good tracking without requiring unbounded actuator action.

The relationship between reference tracking and measurement-noise transmission was also studied. At low frequencies, the closed loop preserved good reference tracking. At high frequencies, the noise-to-output transmission rolled off strongly, indicating strong attenuation of measurement noise.

<div class="row justify-content-sm-center">
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project6/figure5.png" title="Nominal control effort" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/Project6/figure6.png" title="Reference tracking versus measurement noise transmission" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
  Control effort and noise-transmission analysis for the robust H∞ pitch controller.
</div>

## Robust Stability and Robust Performance

A formal robust-stability analysis was performed using MATLAB’s `robuststab` function. The result showed that the system was robustly stable for the modeled uncertainty and could tolerate approximately 196% of the uncertainty set before losing stability. This indicated a strong stability margin.

However, robust-performance analysis using `robustperf` showed that the uncertain system did not satisfy the desired performance objectives over the full uncertainty set. The result indicated that performance robustness was lost at about 79% of the modeled uncertainty. In other words, the system remained stable, but the chosen tracking and gain bounds were not guaranteed for all uncertain plants.

This was one of the main lessons of the project: robust stability and robust performance are related but not the same. A system can remain stable under uncertainty even when it no longer satisfies every performance specification.

## What I Learned

This project strengthened my understanding of robust control, uncertainty modeling, mixed-sensitivity H∞ synthesis, and MATLAB robust-control tools. I learned how to model parametric uncertainty, design an H∞ controller, evaluate nominal and uncertain closed-loop responses, and compare robust stability against robust performance.

The project also showed me how frequency-domain design decisions affect time-domain behavior. The controller achieved strong nominal performance and robust stability, but the formal robust-performance result revealed the tradeoff between uncertainty size, conservatism, and achievable closed-loop performance.

## Conclusion

This project investigated robust H∞ pitch control for a fixed-wing UAV. A longitudinal elevator-to-pitch model was implemented, parametric uncertainty was introduced, and a mixed-sensitivity H∞ controller was synthesized in MATLAB.

The controller achieved nominal closed-loop stability, fast pitch tracking, no overshoot, and stable behavior across sampled uncertain plants. Robust-stability analysis confirmed that the system remained stable for the modeled uncertainty with a substantial stability margin. However, robust-performance analysis showed that performance objectives were not guaranteed over the entire uncertainty set.

Overall, the controller was successful in achieving robust stability and good nominal behavior, while also illustrating an important robust-control lesson: stability under uncertainty does not automatically guarantee full performance under uncertainty.
