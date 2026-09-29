---
title: Autonomous Stereo Bluetooth Speaker
publishDate: 2025-05-01
img: /assets/enceinte-bluetooth.jpg
img_alt: Bluetooth speaker 3D model
description: |
  Hardware design and creation of a 2 × 10 W portable stereo speaker, including PCB routing, analog audio amplification, and 3D modeling of the enclosure.
tags:
  - Electronics
  - PCB Design
  - Audio
  - 3D Printing
---

## Project Overview

This project was conducted during the 6th semester of the electrical engineering program at INSA. The main objective was to design and manufacture a fully autonomous connected stereo speaker, developing the entire electronic and mechanical chain.

### Technical Specifications

The specifications required validating several strict criteria:

- **Sound power:** Deliver an output power of 2 × 10 W through two speakers with a 4-ohm impedance.
- **Hybrid connectivity:** Integrate a Bluetooth module (BK8000L model) and a 3.5 mm Jack wired input, with a switch to toggle between sources.
- **Autonomous power supply:** Use two 4S LiPo batteries to create a bipolar power source.
- **Human-Machine Interface (HMI):** Provide volume control via a dual-channel potentiometer, track selection buttons, an On/Off switch, and indicator LEDs for power and battery discharge status.

## Electronic Design and PCB Routing

The implementation relied on designing several separate Printed Circuit Boards (PCBs), routed using Proteus software.

- **Class AB Amplifier:** The amplification was achieved exclusively with discrete components, using transistors (MJD122G/MJD127G SMD components with heatsinks). The initial schematic was optimized by adding current sources (Darlington configuration) and a junction multiplier to reduce crossover distortion and increase the common-mode rejection ratio.
- **Battery Management System (BMS):** A monitoring circuit was created to protect the LiPo batteries against deep discharges. It relies on TL074 operational amplifiers acting as comparators, using a 6.2 V Zener diode to set a stable reference voltage. If the voltage of one of the batteries drops below the 12 V critical threshold, a DPDT relay automatically cuts off the power distribution to the speaker.
- **Bluetooth Signal Processing:** The differential audio signals provided by the Bluetooth module are processed by a subtractor circuit based on operational amplifiers to eliminate differential noise before amplification.

## Mechanical Modeling

The enclosure design aimed to combine originality and aesthetics.

- **Hexagonal structure:** The speaker adopts an atypical hexagonal shape made from wooden boards cut with 30° bevels.
- **Mixed materials:** The enclosure combines wood for the main casing, a partial plexiglass front panel to reveal the internal components, and 3D-printed plastic elements.
- **Custom 3D printing:** The 120° angles of the hexagonal design required custom modeling (using OnShape software) and printing of brackets with 100% infill to ensure structural stability. A recessed receptacle was also printed to house the HMI front panel.

## Project Review and Feedback

The simulations of the final amplifier on Proteus were promising, predicting a 25.61 dB gain and a reduced total harmonic distortion. However, the physical assembly presented a major voltage imbalance on the differential stage (5 V on one side versus 0.5 V on the other), preventing its practical operation and connection to the speakers. The BMS circuit also suffered from switching thresholds that slightly differed from the expected theoretical values.

Despite these technical difficulties related to component selection or theoretical gaps during debugging, this project was an extremely formative experience. It allowed for the mastery of good PCB routing practices (functional grouping, ground planes, placement of decoupling capacitors), the handling of frequency analysis tools (Bode, FFT), and the execution of a complete project from theoretical design to final mechanical assembly.
