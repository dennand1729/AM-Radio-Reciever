# AM Radio Reciever (In Progress)  

## Table of Contents

- [Summary](#summary)
- [Design Specifications](#design-specifications)
- [Project Diary](#project-diary)  
    - [June 12 2026](#June-12-2026)  


## Summary

Inspired by the role of amplifiers in communication systems and RF applications, I am designing and building an AM radio receiver as a personal project.

This project combines analog circuit design concepts from my semiconductor coursework at the University of Washington with signal analysis techniques from my Continuous-Time Signals class. Through the design process, I am applying principles such as resonance, frequency selectivity, amplification, and demodulation to create a functional receiver capable of tuning and reproducing broadcast AM signals.

By integrating theory with hands-on implementation, this project has strengthened my understanding of RF communication systems and reinforced key skills in analog electronics, circuit analysis, and signal processing.

## Design Specifications

- Wire Antennae
- LC Tank Circuit
- Envelope Detector
- NMOS Amplifier

## Project Diary

### June 12 2026

I aimed to pick up Northwest News Radio aka 1000 AM channel. 

I knew an antennae would capture all frequnecies available to it at once and then the circuit would need to isolate the desired frequency. I decided to design an LC Tank Circuit to utilize the resonany frequency of the tank to
isolate 1000 AM. 

Before building the circuit, I simulated the design on LTspice. Observe:

![LC Tank LTspice schematic](./LC%20Tank%20LTs%20schematic.png) ![LC Tank LTspice simulation](./LC%20Tank%20LTs%20simulation.png)

This confirmed the LC tank circuit would use resonance to isolate 1000 kHz aka 1 MHz.







