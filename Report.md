# HC‐SR04 Ultrasonic Emergency Stop: Design & Implementation
---
### Robot Configuration

The robot is built on an **Adafruit M0 Feather** microcontroller and is equipped with **four mecanum wheels**, allowing it to move in all directions, including sideways.  

An **ultrasonic distance sensor** (HC-SR04) is mounted at the front of the robot to detect obstacles and measure distances in real time. The microcontroller reads the sensor measurements and uses them to control the robot's motion.

---
## Sensor Functionality
The HC‐SR04 uses ultrasonic time‐of‐flight. 

A short trigger pulse makes the transmitter emit an $40 kHz$ burst.
The receiver listens for the echo reflected by obstacles.

The Arduino measures the echo pulse width $t$ and computes distance $d$ via the speed of sound $c \approx 343 \frac{\mathrm{m}}{\mathrm{s}}$.

$$
d = \frac{c \cdot t}{2} = \frac{343 \frac{\mathrm{m}}{\mathrm{s}} \cdot t}{2}
$$

With $t$ in microseconds, a common conversion used in the code is:

$$
d_\text{cm} \approx \frac{t}{58}
$$

---
## HC‐SR04 pin functions
| Pin  | Function |
|:----:|:--------:|
| VCC  | +5 V supply for HC‐SR04 |
| GND  | Ground reference (common with Arduino) |
| TRIG | Digital pin 0 (output). 10 µs pulse to start measurement |
| ECHO | Digital pin 1 (input). High pulse width proportional to distance |

---
## Hysteresis
### In Physics
In physics, **hysteresis** refers to a system’s dependence on its history. That is, the system’s response doesn’t just depend on the current input, but also on its past states. A classic example is a magnetic material:

- If you apply a magnetic field to iron, it becomes magnetized.
- When you reduce the field back to zero, the magnetization doesn’t immediately return to zero.
- The curve of magnetization and the applied field form a loop, showing that the material remembers its previous state.

So, hysteresis prevents sudden flipping of the system when inputs fluctuate around a threshold.

### Hysteresis In This System

In this robot system:

- The robot stops if an obstacle is too close.
- The robot does not immediately resume motion if the distance increases slightly above the stop threshold, because sensor readings may fluctuate due to noise.

Using hysteresis, two distance thresholds are, for example, defined:

| Action | Distance Threshold |
|--------|------------------|
| Stop   | 25 cm            |
| Resume | 40 cm            |

The behavior is as follows:

1. The robot moves forward (N, NW, NE movements).
2. When an obstacle appears at 30 cm, the robot keeps moving (above the stop threshold).
3. When the obstacle reaches 25 cm, the robot stops.
4. If the obstacle moves back to 30 cm, the robot remains stopped (below the resume threshold).
5. Once the obstacle reaches 40 cm, the robot resumes motion.

The gap between the stop and resume thresholds prevents rapid oscillations when the measured distance fluctuates around a single value.

---
## References






