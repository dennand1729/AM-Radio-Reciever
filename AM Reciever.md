# AM Radio Reciever (In Progress)  

## Table of Contents

- [Summary](#summary)
- [Design Specifications](#design-specifications)
- [Project Diary](#project-diary)  
    - [Designing an LC Tank Circuit](#Designing-an-LC-Tank-Circuit)
    - [Learning to Demodulate a Signal](#Learning-to-Demodulate-a-Signal)
    - [Testing the Envelope Detector Using Analog Discovery](#Testing-the-Envelope-Detector-Using-Analog-Discovery)
    - [Picking Up Real Broadcasts via Antennae](#Picking-Up-Real-Broadcasts-via-Antennae)
    - [Developing an Amplification Stage Using Modular Design](#Developing-an-Amplification-Stage-Using-Modular-Design)
        - [Using an IC to Amplify and Drive a Speaker](#Using-an-IC-to-Amplify-and-Drive-a-Speaker)
        - [Creating a Cascaded Amplifier Using NMOS](#Creating-a-Cascaded-Amplifier-Using-NMOS)


## Summary

Inspired by the role of amplifiers in communication systems and RF applications, I am designing and building an AM radio receiver as a personal project.

This project combines analog circuit design concepts from my semiconductor coursework at the University of Washington with signal analysis techniques from my Continuous-Time Signals class. Through the design process, I am applying principles such as resonance, frequency selectivity, amplification, and demodulation to create a functional receiver capable of tuning and reproducing broadcast AM signals.

By integrating theory with hands-on implementation, this project has strengthened my understanding of RF communication systems and reinforced key skills in analog electronics, circuit analysis, and signal processing.

## Design Specifications

- 22 AWG solid wire
- LC Tank Circuit
- Envelope Detector
- Low Pass Filter
- NMOS Amplifier

## Project Diary

## Designing an LC Tank Circuit

I aimed to pick up and demodulate waves transmitted by KNWN Northwest News Radio aka 1000 AM channel. The transmitter is on Vashon Island but is relatively powerful making it an ideal canidate for the project. 

I knew an antennae would capture all frequnecies available to it at once and then the subsequent circuit would need to isolate the desired frequency to the end of amplifying it. I decided to design an LC Tank Circuit to utilize the resonany frequency of the tank to isolate 1000 AM. 

Before building the circuit, I simulated the design on LTspice with a 100uH inductor and a 220pF capacitor. Observe:

![LC Tank LTspice schematic](./LC%20Tank%20LTs%20schematic.png) ![LC Tank LTspice simulation](./LC%20Tank%20LTs%20simulation.png)

This confirmed the designed LC tank circuit would use resonance to isolate and select the signal at 1000 kHz. I double-checked this simulation result by using the standard equation for resonance in an LC circuit. Observe:

$$f_0 \text{(in Hertz)}= \displaystyle \frac{1}{2 \pi \cdot \sqrt{LC}} = \displaystyle \frac{1}{2 \pi \cdot \sqrt{100 \cdot 10^{-6} \cdot 220 \cdot 10^{-12}}} = 1073022.47 \approx 1 \text{MHz}$$

I verified using the Analog Discovery's Network Analyzer feature that the LC circuit was working as intended at 1000 kHz.
[insert pic of the network analyzer screen on waveforms and the breadboard setu...p..]

## Learning to Demodulate a Signal

The next step was using an envelope detector to extract the high frequency carrier signal component from the output of the tank circuit, ultimately, leaving the end-user with a clean, demodulated message signal.

- To avoid increased ripple, I made the RC time constant much greater than the carrier period. This made it so that the demodulation did not follow the high frequency cycle of the carrier wave which would introduce unwanted ripple into the output waveform.

- To more accurately craft the envelope, I made the RC time constant much less than the message period (assumed < 5kHz). This means the output waveform would follow changes in the envelope of the modulated signal-- failing to follow this constraint would result in the detector's capacitor discharging too slowly and diagnoally clipping the silhouette of the modulated message.

I did not know how to use an Analog Discovery to send an ampltude modulated wave through the envelope detector circuit so I used LTspice to capture data on changing the value of the RC time constant.

After using GPT AI to generate an appropriate spice model for the 1N5819 Schottky diode, I ran a transient analysis for different resistances. I plotted the data collected in Excel, observe:  

![Schematic of Envelope Detector](Envelope_Detector_LTs.png) ![Excel Graph of Output vs Varying R](./Envelope%20Detector.png) 

Recall, the envelope of the non-modulated sinusidal input waveform should be a straight line. Hence, the 10k resistance created a discharge pattern which most accurately reflected the input's envelope.

Based on the simulated results above, I verified the validity of the standard criterion for a well conditioned envelope detector time constant using the R and C values I selected for the sub-circuit:

$$\displaystyle \frac{1}{f_c} << RC << \displaystyle \frac{1}{f_m}$$

$$\displaystyle \frac{1}{1 MHz} << (10k\Omega) \cdot (1 nF) << \displaystyle \frac{1}{5kHz} $$

$$ \implies 1 \mu s << 10 \mu s << 200 \mu s$$

 I then used the Analog Discovery Module and Waveforms software to send in a nonmodulated wave at carrier frequency and measure the response on the oscilliscope. Observe:
 ![Analog Discovery Scope Screenshot](./WaveForms_Envelope_Detector_Scope.png)

In the screenshot of the oscilloscope above, channel 1 depicted the 1MHz wave being sent in to the circuit, and channel 2 represented the output of the cascaded LC tank circuit and envelope detector. This inspred confidence as the demodulated envelope was a straight line as predicted by the Excel plot of the LTspice simulation.

## Testing the Envelope Detector Using Analog Discovery

Realizing there would be multiple more hurdles to jump through, I realized I could not continue to build upon the system I had before testing if it could demodulate an AM signal. I researched the Analog Discovery's
features and found that an inputted signal could be modulated. Hence, success would mean measuring an output that could take a 2kHz wave modulated at 1MHz and filter out the high-frequency carrier. To my delight, I found that the envelope detector produced the expected output:

<div>
  <img src="Envelope_Detector_Demodulation.png" width="750">
  <img src="cascaded_system_p1.jpg" width="250">
</div>

This phase marked a turning point in the project. After being unsure if the envelope detector was working properly (due to not being able to test with a modulated input waveform), I felt reinvigorated seeing the cursors on the scope display a period that matched the 2 kHz input before pre-modulation. 

I was also proud to have gained familiarity with using amplitude modulation in the waveform generator along with learning to use different modes of the scope and its cursors for measurement. I knew these would be valuable skills for troubleshooting and testing circuits in the furture.

## Picking Up Real Broadcasts via Antennae

Finally, it was time to begin picking signals up out of the air. I did not know much about how to do this and thought back to the radio/alarm-clock I had as a kid. I remember a simple wire antennae being able to pick up signals all across the FM and AM bands. 

I proceeded to use the only wire I had on hand-- soldering wire-- and hooked one side of an ~8ft segment to the head of the LC tank and the other side to ground. When I tested the signal at the output of the envelope detector I saw a flatline. I knew this indicated a problem and upon doing some research on the internet I realized that the soldering wire (which likely contained some sort of tin alloy) was far less conductive than copper wire. I proceeded to find 22 AWG solid copper wire and hook it up similarly and was still getting an almost negligible signal. 

After consulting with AI, I tried two things. I changed the scope's axis to pick up smaller signals; after doing this, I could see the electromagnetic noise the antennae was picking up but no clear signal. The second thing I tried was establishing counterpoised ground. The Analog Discovery draws power from my laptop. The laptop, even when plugged in, is not connected to earth ground as it has a 2 prong plug! The solution was using a the spool of wire I had to create a ground plane by connecting the GND power rail to the spool and placing it across the room on the floor. 

Below are pictures of the antennae and the counterposed ground used:

<div>
  <img src="" width="750">
  <img src="" width="250">
</div>

From this portion of the project I learned that since the radio station is being transmitted with respect to earth ground, the radio receiver needed to pick up 1000 kHz with respect to earth ground as well to properly recieve the signal. One method to create this reference on the recieving side if no direct access to ground is available is to create a counterpoised ground to function as a 0 volt ground plane reference.


## Developing an Amplification Stage Using Modular Design


### Using an IC to Amplify and Drive a Speaker

### Creating a Cascaded Amplifier Using NMOS



