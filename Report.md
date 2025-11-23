# HC‐SR04 Ultrasonic Emergency Stop: Design & Implementation
---
## Sensor Functionality
The HC‐SR04 uses ultrasonic time‐of‐flight. 

A short trigger pulse makes the transmitter emit an 40 kHz burst.
The receiver listens for the echo reflected by obstacles.

The Arduino measures the echo pulse width t and computes distance d via the speed of sound c ≈ 343 m/s:
