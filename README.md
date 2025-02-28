[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/h0hAJrx7)
# WS2 Bare Metal

* **Name:** Ben Saxon
* **GitHub Repository URL:** https://github.com/b3prime/bsaxon_ws2/edit/main/README.md
* **Description of hardware (embedded hardware, laptop, etc):**

No hardware used.

## Design 1: An Autonomous Vehicle

**(R1)**	**Discuss your approach to solving this design problem.**

Peripheral Detection: I’d use current-sensing circuits (sense resistors) on each peripheral’s power line. The ATmega328PB’s ADC reads voltage across the resistors; firmware detects current flow (indicating a connected peripheral) and triggers LEDs.

Low Battery Alert: A voltage divider scales the battery voltage (9.5V to  ~3.17V) for ADC. Firmware compares readings to a threshold (9.5V) and activates a buzzer via GPIO.

30-Minute Timer: Configure a 16-bit timer to count system uptime. After 30 minutes, firmware triggers a rapid LED blink (PWM) and buzzer (GPIO toggling). A button press (polled or interrupt) resets the timer.


**(R2)**	**Describe how each of your sensors and actuators will exchange information with the MCU.**

Current Sensors: ADC raw values -> converted to current (thru Ohm’s law). Threshold comparison in firmware controls the GPIO that drives LEDs.

Battery Voltage: ADC readings are scaled to actual voltage. If <9.5V, buzzer GPIO set HIGH.

Timer: Timer overflow interrupts track time. On timeout, firmware toggles LEDs/buzzer. Button input (polled) silences alarms.


**(S1)**	**Create a flowchart to explain the sequence of tasks your MCU will handle in firmware.**

![Screenshot](Screenshot%202025-02-27%20224115.png)

**(R3)**	**If you need extra parts, record those parts in the Design 1 tab of the BOM.**

Current sense resistors (0.1Ω, 1W) for current sensing
LM358 for amplifying sense voltages 
Voltage divider resistors (10kΩ, 3.3kΩ) for battery monitoring

## **Design 2: Help Hermione**

**(R4)**	**Discuss your approach to solving this design problem.**

5:00AM Trigger: Use Wi-Fi/UART to sync time. Start PWM for buzzer (15s ramp) and light (10s ramp).

Pressure Sensor: Poll analog/digital input. If no weight after 5 minutes, max buzzer (PWM 100% duty).

Actions on Pressure: Activate coffee maker (relay GPIO), fetch weather/news via Wi-Fi (UART to screen), and trigger cat feeder (GPIO timer).

**(R5)**	**Describe how each of your sensors and actuators will exchange information with the MCU.**

Wi-Fi Module: Sends time data via UART. Firmware parses for 5:00AM.

Buzzer/Light: PWM outputs with increasing duty cycles.

Pressure Sensor: ADC/digital input causes firmware to start a 5-minute timer.

Coffee Maker/Cat Feeder: GPIO relays. Screen data sent via UART.

**(S2)**	**Create a flowchart to explain the sequence of tasks your MCU will handle in firmware.**

![Screenshot](Screenshot%202025-02-27%20224121.png)

**(R6)**	**If you need extra parts, record those parts in the Design 2 tab of the BOM.**

## Short Answers

#### Timers and GPIO

**(R7)**	**What does it mean for a pin to be in a state of high impedance?**

 High impedance (Hi-Z) means the pin neither sources nor sinks current (input/disconnected state).

**(R8)**	**How is a pin change interrupt different from input capture?**

Pin-change interrupts detect any logic-level change. Input capture records the exact time a specific edge (rising/falling) occurs.

**(R9)**	**When would you use one or the other?**

Use pin-change for general state monitoring (e.g., button press). Use input capture for timing-critical events (e.g., pulse width monitoring).

**(R10) What’s the difference between generating a PWM wave in PWM mode compared to one of the non-PWM modes?**

PWM mode automates waveform generation via hardware. Non-PWM modes require manual toggling (e.g., timer overflow interrupts).

#### ADC

**(R11) List two methods of ADC and give a brief explanation of each. It can be methods explained in class or another method that you explore independently.**

SAR (Successive Approximation) compares input voltage iteratively using a DAC. Dual-Slope integrates voltage over time for better noise immunity.

**(R12) Why is ADC resolution important?**

Resolution (e.g., 10-bit vs. 12-bit) determines the smallest detectable voltage change. Critical for precision applications.

**(R13) List 5 uses for ADC.**

Temperature sensing, battery monitoring, light detection, audio input, potentiometer position.

#### Button Debouncing

**(S3)**	**Include the schematic as an image in the README.md file and a share link to the simulation in Circuit Lab.**

![Screenshot](Screenshot%202025-02-27%20224128.png)

CREDIT: I did not design the Schmitt trigger. In reality you’d just use in IC for this

https://www.circuitlab.com/circuit/24y4tg2as574/schmitt-debouncer/

**(R14) How many components are in your circuit?**

6 components (I consider the Schmitt trigger one component):
2x resistors
1x diode
1x capacitor
1x switch
1x Schmitt trigger

**(R15) Why might (or might not) a company implement your circuit when shipping a product? Would it matter if the product is a children’s toy or a medical device? What about a wearable device?**

A company might skip hardware debouncing in cost-sensitive products (toys) but use it in medical/wearables for reliability.

---



* [Markdown Guide: Basic Syntax](https://www.markdownguide.org/basic-syntax/)
* [Adobe free video to gif converter](https://www.adobe.com/express/feature/video/convert/video-to-gif)
* [Curated list of example READMEs](https://github.com/matiassingers/awesome-readme)
* [VS Code](https://code.visualstudio.com/) is heavily recommended to develop code and handle Git commits

  * Code formatting and extension recommendation files come with this repository.
  * Ctrl+Shift+V will render the README.md (maybe not the images though)
