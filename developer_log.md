# Autonomous Indoor Liquid Cleaning Robot

**Developer:** Cheng-Yan Tsai  
**Start Date:** 2026-07-11

---

# Development Log

## 2026-07-11 ~ 2026-07-18

### Objective

Project planning and hardware preparation.

### Work Completed

- Defined the project requirements and functional objectives.
- Planned the overall system architecture.
- Created the Bill of Materials (BOM).
- Purchased the required hardware components for the robot.

### Notes

The hardware procurement phase was completed successfully, allowing the project to move into hardware integration.

### Next Steps

- Set up the Raspberry Pi 5 development environment.
- Verify SSD boot.
- Test the camera module.

---

## 2026-07-19

### Objective

Set up the Raspberry Pi 5 development environment and verify the Camera Module V2.

### Work Completed

#### 1. Raspberry Pi 5 Setup

- Installed and configured Raspberry Pi OS.
- Successfully configured the Raspberry Pi to boot from the SSD.
- Verified SSH remote access from the development computer.
- Learned the proper shutdown procedure using:

```bash
sudo poweroff
```

#### 2. Camera Module V2 Installation

- Installed the Raspberry Pi Camera Module V2.
- Replaced the ribbon cable with a **15-pin to 22-pin FFC cable**, which is required when connecting the Camera Module V2 to the Raspberry Pi 5.
- Verified that the camera was detected successfully using:

```bash
rpicam-hello --list-cameras
```

- Confirmed that the camera sensor was recognized as **Sony IMX219**.

#### 3. Camera Test

Captured the first test image using:

```bash
rpicam-still -o test.jpg
```

Successfully transferred the image from the Raspberry Pi to the Mac using:

```bash
scp chengyantsai@raspberrypi.local:~/test.jpg .
```

The captured image was clear and confirmed that the camera hardware was functioning correctly.

### Observations

- Raspberry Pi Camera Module V2 does not include a power or status LED.
- Camera functionality should be verified through software detection instead of hardware indicators.
- The default `rpicam-still` command requires approximately **2 seconds** before capturing an image because the camera performs automatic exposure, automatic white balance, and gain adjustment during initialization.
- This initialization delay is acceptable for still photography but unsuitable for autonomous robot navigation.

### Issues to Investigate

- Determine whether the startup delay only occurs with `rpicam-still`.
- Evaluate the real-time streaming performance using Picamera2.
- Measure frame rate (FPS) and image latency under different resolutions.
- Identify the optimal camera configuration for OpenCV image processing.

### Lessons Learned

- Raspberry Pi 5 requires a **15-pin to 22-pin** ribbon cable when using the legacy Camera Module V2.
- The camera can be verified using `rpicam-hello --list-cameras`.
- Images can be transferred directly to the development computer using `scp`.

### Next Steps

- Develop a Python camera test program using Picamera2.
- Display live camera images in Python.
- Integrate OpenCV for real-time image acquisition.
- Benchmark camera frame rate and latency.

---

## 2026-07-21

### Objective

Verify Raspberry Pi ↔ Arduino communication before hardware integration and prepare the mechanical design phase.

### Work Completed

#### 1. BOM Update

- Reviewed the current BOM.
- Identified remaining hardware components required for system integration.
- Organized CAD model references.

#### 2. CAD Preparation

- Collected most purchased-part CAD models.
- Imported official Raspberry Pi STEP files and GrabCAD models.
- Prepared for SolidWorks assembly.

#### 3. Raspberry Pi ↔ Arduino Serial Communication Test

##### Hardware

- Raspberry Pi 5
- Arduino Uno R3
- USB Serial

##### Software

- Python (pyserial)
- Arduino IDE
- Baud Rate: 115200

##### Test Procedure

1. Raspberry Pi sends a serial command.
2. Arduino receives the command.
3. Arduino updates the LED blinking interval immediately.
4. Arduino returns an acknowledgement message.
5. Raspberry Pi displays the returned message.

Example:

```text
100
→ LED blinks every 100 ms

500
→ LED blinks every 500 ms

1000
→ LED blinks every 1000 ms
```

##### Result

Successfully verified:

- Raspberry Pi → Arduino communication
- Arduino → Raspberry Pi communication
- Runtime parameter update without reprogramming
- Stable USB serial communication

### Lessons Learned

- Use English for Serial Protocol, Terminal Output, and Development Log.
- SSH + nano may display Chinese characters incorrectly due to UTF-8 encoding.
- Sending runtime parameters is more practical than sending simple ON/OFF commands.
- The communication framework matches the intended robot architecture.

```text
Raspberry Pi
      │
High-Level Decision
      │
USB Serial
      │
Arduino
      │
Real-Time Hardware Control
```

Future commands can directly extend to:

```text
MOTOR_LEFT:120
MOTOR_RIGHT:120
PUMP:ON
PUMP:OFF
SERVO:45
```

without changing the communication architecture.

### Next Steps

- Build the SolidWorks master assembly.
- Complete the first chassis layout.
- Integrate the TB6612 motor driver.
- Replace LED control with motor PWM commands.

### Development Time

Start: 12:00  
End: 14:20  
Duration: 2 h 20 min

---

## 2026-07-23

### Objective

Begin the physical system layout and establish the internal architecture of the robot.

### Work Completed

#### 1. Major Component Import

Imported major electronic components into SolidWorks, including:

- Raspberry Pi 5
- Arduino Uno R3
- 18650 battery system
- Other major electronic components

#### 2. Full-Vehicle Spatial Layout

- Started the full-vehicle spatial layout.
- Established a layered internal architecture:
  - Control Layer
  - Power Layer
- Evaluated component placement based on available chassis volume and wiring requirements.

### Development Time

Start: 13:00  
End: 15:00  
Duration: 2 h

---

## 2026-07-27

### Objective

Develop the detailed Power Layer layout in SolidWorks.

### Work Completed

#### 1. Power Layer Development

Modeled and arranged the Power Layer.

Integrated:

- 18650 2S2P battery system
- 2 × XL4015 DC-DC converter modules
- TB6612 motor driver
- 2 × front drive motors

#### 2. Packaging Evaluation

- Evaluated component spacing and installation positions.
- Continued development of the internal power-system packaging.

### Development Time

Start: 15:30  
End: 17:30  
Duration: 2 h

---

## 2026-07-28

### Objective

Develop the Control Layer layout.

### Work Completed

#### 1. Control Layer Development

Modeled and arranged the Control Layer in SolidWorks.

Integrated:

- Raspberry Pi 5
- Arduino Uno R3
- 2 × prototyping/perfboards

#### 2. Component Placement

- Planned mounting locations for the main control electronics.
- Separated the control electronics from the Power Layer to improve system organization and maintainability.

### Development Time

Start: 13:30  
End: 16:00  
Duration: 2 h 30 min

---

## 2026-07-29

### Objective

Develop the upper chassis structure and allocate installation locations for the cleaning and vision systems.

### Work Completed

#### 1. Upper Chassis Development

- Modeled the upper chassis section in SolidWorks.
- Designed mounting openings for:
  - Pump
  - Liquid reservoir
  - Camera

#### 2. Component Integration Planning

- Evaluated the physical relationship between the cleaning system and electronic compartments.
- Reserved installation space for the camera system.

### Development Time

Start: 13:00  
End: 15:00  
Duration: 2 h

---

## 2026-07-30

### Objective

Refine the upper chassis design and improve wiring, ventilation, and rear-wheel integration.

### Work Completed

#### 1. Upper Chassis Revision

- Revised the upper chassis design.
- Added additional openings for electrical wiring.

#### 2. Thermal and Mechanical Integration

- Planned chassis ventilation and airflow.
- Designed the rear caster-wheel installation area.
- Designed the cooling-fan mounting structure.
- Improved the overall mechanical layout based on wiring and thermal-management requirements.

### Development Time

Start: 10:30  
End: 13:30  
Duration: 3 h

---

## 2026-08-03

### Objective

Integrate the pump into the mechanical assembly and reserve space for future robotic-arm installation.

### Work Completed

#### 1. Pump Integration

- Imported the pump model into the SolidWorks assembly.
- Evaluated the pump installation position.

#### 2. Robotic Arm Installation Planning

- Planned the mounting location for a future robotic arm.
- Checked available space and potential interference with existing components.

### Development Time

Start: 11:00  
End: 14:00  
Duration: 3 h

---

## 2026-08-04

### Objective

Perform a full mechanical design review and refine the vehicle assembly.

### Work Completed

#### 1. Full-Vehicle Design Review

- Conducted a full-vehicle SolidWorks design review.
- Performed detailed dimensional and positional adjustments.
- Reviewed component clearances and internal packaging.
- Refined the overall mechanical arrangement.

#### 2. Modular Mounting Design

- Added LEGO-compatible mounting points to increase modularity and allow future attachments or experimental components.

### Development Time

Session 1: 11:00 – 14:00  
Session 2: 21:00 – 23:00  
Total Duration: 5 h

---

## 2026-08-05

### Objective

Begin detailed electrical schematic development in SolidWorks Electrical.

### Work Completed

#### 1. Electrical Symbol Creation

Created electrical schematic symbols for major system components:

- Raspberry Pi 5
- Arduino Uno R3
- TB6612 motor driver
- XL4015 DC-DC converter
- JGA25-370 geared DC motor with quadrature encoder

#### 2. Component Library Preparation

Prepared the component library required for subsequent electrical schematic development.

### Development Time

Start: 10:00  
End: 12:00  
Duration: 2 h

---

## 2026-08-06

### Objective

Develop the robot power-distribution schematic in SolidWorks Electrical.

### Work Completed

#### 1. Power-Distribution Schematic

- Started the electrical power-distribution drawing.
- Defined the relationship between:
  - Battery system
  - Power conversion modules
  - Motor power system
  - Control electronics

#### 2. Electrical Architecture Documentation

- Began documenting the electrical architecture developed during the previous mechanical-layout phase.
- Established electrical documentation intended to maintain consistency between the physical SolidWorks assembly and the actual wiring architecture.

### Development Time

Start: 09:00  
End: 12:00  
Duration: 3 h

---

## Development Progress Summary: 2026-07-23 ~ 2026-08-06

During this period, the project progressed from initial hardware preparation into detailed mechanical packaging and electrical-system documentation.

### Mechanical Design

Completed or substantially developed:

- Full-vehicle component layout
- Control Layer
- Power Layer
- Upper chassis
- Battery installation
- Raspberry Pi installation
- Arduino installation
- Motor-driver installation
- Pump installation
- Liquid-reservoir mounting area
- Camera mounting area
- Rear caster-wheel structure
- Cooling-fan mounting structure
- Wiring openings
- Ventilation design
- Robotic-arm installation space
- LEGO-compatible modular mounting points

### Electrical Design

Started detailed SolidWorks Electrical development for:

- Raspberry Pi 5
- Arduino Uno R3
- TB6612
- XL4015
- JGA25-370 geared DC motors with quadrature encoders
- Power distribution

### Current Project Phase

The project has progressed through:

```text
Project Planning
      ↓
Hardware Procurement
      ↓
Raspberry Pi Bring-Up
      ↓
Pi ↔ Arduino Communication Verification
      ↓
Mechanical Packaging
      ↓
Chassis Design
      ↓
Electrical Component Library Development
      ↓
Power Distribution Schematic
      ↓
Hardware Integration and Testing
```

---

## 2026-08-10

### Objective

Begin physical integration of the drivetrain.

### Work Completed

#### 1. Front Drivetrain Installation

- Installed the two front JGA25-370 geared DC motors.
- Installed the front wheels onto the motor assemblies.
- Verified the basic mechanical installation of the front drivetrain.
- Prepared the drivetrain for subsequent electrical connection and motor testing.

### Development Time

Start: 21:00  
End: 21:30  
Duration: 30 min

---

## 2026-08-11

### Objective

Prepare the 2S2P battery system for BMS integration, improve the battery-holder wiring, and configure the two XL4015 DC-DC power rails for subsequent hardware integration.

### Work Completed

#### 1. 2S Battery Holder Wiring Modification

Modified both 2S 18650 battery holders to improve their current-carrying capability and prepare them for integration with the 2S BMS.

The original thin battery-holder wires were desoldered and replaced with:

- 18 AWG power wiring

Additional wiring modifications included:

- Added a dedicated positive output wire.
- Added a dedicated negative output wire.
- Added a midpoint/balance wire at the electrical junction between the two series-connected cells.
- Prepared the midpoint connection for subsequent integration with the 2S BMS.

Each modified 2S battery holder now provides three electrical connections:

```text
B+  → Battery Pack Positive
BM  → Battery Midpoint
B-  → Battery Pack Negative
```

These connections will later be used to combine the two battery holders into the 2S2P battery configuration and connect them to the 2S BMS.

#### 2. Battery Holder Mechanical Modification

Modified both battery holders to improve cable routing.

Additional holes were drilled into each battery holder to provide dedicated wire exits for:

- Positive terminal wire
- Negative terminal wire
- Battery midpoint wire

This allows the three electrical connections to exit the battery holders independently and provides cleaner cable routing for subsequent BMS installation.

#### 3. Battery Installation and Electrical Verification

Installed the 18650 cells into both modified 2S battery holders.

Used a digital multimeter to verify the electrical condition of the battery assemblies after soldering and mechanical modification.

Verified:

- Electrical continuity of the modified wiring
- Correct positive and negative terminal connections
- Accessibility of the battery midpoint connection
- Output voltage of each 2S battery holder
- No unintended open circuits after soldering and rewiring

The modified battery holders were confirmed to be electrically functional and ready for subsequent 2S2P and BMS integration.

#### 4. XL4015 Power-Rail Configuration

Configured the two XL4015 CC/CV buck converters for the robot's two primary low-voltage power rails.

The converters were configured as:

```text
XL4015 #1
Output Voltage: 5.1 V
Intended Load: Raspberry Pi 5
```

```text
XL4015 #2
Output Voltage: 6.0 V
Intended Load: Motor / Actuator Power Rail
```

The 5.1 V rail is intended primarily for the Raspberry Pi 5.

The 6.0 V rail is intended for the motor and actuator system, including the 6 V JGA25-370 drive motors and other compatible 6 V loads.

Both output voltages were adjusted and verified using a digital multimeter before connecting the final loads.

During this process, the operating principle of the XL4015 CC/CV control was also investigated.

It was confirmed that:

- CV (Constant Voltage) determines the target output voltage.
- CC (Constant Current) establishes the current-limit threshold.
- The connected load determines the actual operating current.
- Setting the CC limit to 5 A does not force 5 A continuously into the connected device.

#### 5. XL4015 Current Measurement Incident

After configuring the XL4015 output voltages, the next objective was to verify the output-current capability and better understand the operation of the CC adjustment.

Initially, there was a misunderstanding that the output current of the XL4015 could be measured in the same way as its output voltage.

The digital multimeter was therefore switched to DC current measurement mode and connected directly across the XL4015 output terminals:

```text
XL4015 OUT+ ── Ammeter ── XL4015 OUT-
```

Immediately after making this connection, a large current flowed through the multimeter and caused its internal current-input protection fuse to blow.

After investigating the incident, the problem was identified as an incorrect measurement configuration rather than a failure of the XL4015 converter.

When measuring voltage, the multimeter is connected in parallel because the voltmeter input has very high internal resistance:

```text
             ┌── Voltmeter ──┐
             │               │
XL4015 OUT+ ─┴──── Load ─────┴─ XL4015 OUT-
```

However, when measuring current, the multimeter operates as an ammeter with very low internal resistance.

The ammeter must therefore be inserted in series with an actual load:

```text
XL4015 OUT+
      │
      ↓
   Ammeter
      │
      ↓
     Load
      │
      ↓
XL4015 OUT-
```

Connecting the ammeter directly between `OUT+` and `OUT-` effectively created a near-short circuit across the XL4015 output.

The resulting high current exceeded the protection limit of the multimeter current-measurement circuit, causing the internal fuse to interrupt the circuit.

The incident did not indicate a failure of the XL4015 module; it resulted from the incorrect use of the multimeter in current-measurement mode.

### Root Cause

The root cause of the incident was confusion between voltage-measurement and current-measurement methods.

The incorrect assumption was:

```text
Voltage:
Measure directly across OUT+ and OUT-

Current:
Measure directly across OUT+ and OUT-
```

The correct principle is:

```text
Voltage Measurement → Parallel Connection

Current Measurement → Series Connection
```

A voltmeter is designed with very high internal resistance and can therefore be connected across a voltage source.

An ammeter is designed with very low internal resistance so that the circuit current can flow through the meter.

Therefore, directly connecting an ammeter across a powered voltage source effectively creates a short circuit.

### Lessons Learned

#### 1. Voltage and Current Measurement

- Voltage must be measured in parallel with the source or load.
- Current must be measured in series with the load.
- A voltmeter has high internal resistance and draws very little current from the circuit.
- An ammeter has very low internal resistance because the measured current must pass through it.
- An ammeter must never be connected directly across the positive and negative terminals of a powered voltage source.
- Connecting an ammeter directly across a power supply can effectively create a short circuit.
- The multimeter's internal fuse is a protection device and should not be relied upon as a normal method of limiting test current.

#### 2. Power Supply Current Rating

A power supply's rated current represents its available current capability, not a fixed current that is continuously delivered to the connected device.

For example:

```text
Power Supply:
5.1 V / Maximum 5 A
```

does not mean:

```text
Load continuously receives 5 A
```

Instead, the connected load determines its actual operating current.

For example:

```text
Load requires 1 A → Power supply provides approximately 1 A
Load requires 2 A → Power supply provides approximately 2 A
Load requires 4 A → Power supply provides approximately 4 A
```

The power supply must simply be capable of safely supporting the current demanded by the load.

#### 3. XL4015 CC/CV Operation

The XL4015 normally operates in Constant Voltage (CV) mode when the load current remains below the configured CC threshold.

```text
Load Current < CC Limit
        │
        ↓
     CV Mode
        │
Output Voltage ≈ Configured Voltage
```

When the load attempts to draw current beyond the configured limit:

```text
Load Current → CC Limit
        │
        ↓
     CC Mode
        │
Output Current is Limited
        │
Output Voltage May Decrease
```

Therefore, for this application, the CC adjustment should primarily be understood as a current-limit setting rather than a command that forces constant current into the connected device.

#### 4. Measuring Power-Supply Current Capability

A multimeter alone cannot determine the maximum output-current capability of a power supply by being connected directly across its output.

An appropriate electrical load is required.

Possible test loads include:

- The intended system load
- A suitable high-power resistor
- A DC electronic load

The multimeter can then be inserted in series with the load if direct current measurement is required.

#### 5. Battery Wiring and BMS Preparation

- The original battery-holder wiring was replaced with 18 AWG wire to improve current-carrying capability.
- A 2S BMS requires access to the battery-pack positive terminal, battery midpoint, and battery-pack negative terminal.
- Battery wiring should be mechanically secured and routed through dedicated openings to reduce the risk of wire damage.
- Electrical continuity and voltage should always be verified after soldering or modifying battery wiring.
- Battery polarity must be confirmed before connecting the battery system to the BMS or downstream electronics.

#### 6. Measurement Safety Procedure

Before energizing a circuit, verify the following sequence:

```text
Measurement Mode
      ↓
Probe Port
      ↓
Connection Method
      ↓
Expected Measurement Range
      ↓
Apply Power
```

For future electrical measurements:

1. Estimate the expected voltage or current before connecting the multimeter.
2. Select the correct DC voltage or DC current measurement mode.
3. Verify that the probes are connected to the correct multimeter input terminals.
4. Confirm whether the measurement requires a parallel or series connection.
5. When measuring an uncertain current, begin with the appropriate high-current range.
6. Never connect the current-measurement input directly across a powered voltage source.
7. Verify polarity before energizing DC power circuits.
8. Configure and verify DC-DC converter output voltage before connecting sensitive electronic loads.

This incident reinforced the importance of understanding both the circuit under test and the electrical behavior of the measurement instrument before applying power.

### Next Steps

- Complete the 2S2P battery interconnection.
- Install and connect the 2S BMS.
- Verify `B+`, `BM`, and `B-` voltages before connecting downstream electronics.
- Connect the 6 V power rail to the TB6612 motor driver.
- Bench-test both JGA25-370 geared DC motors.
- Verify forward and reverse motor operation.
- Test the quadrature encoder A/B signals.
- Begin Arduino-based PWM motor control through the TB6612.

### Development Time

Start: 10:30  
End: 13:30  
Duration: 3 h

---
