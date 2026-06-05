# Active Variable Incidence Wing System (AVIWS)

Arduino-based active aerodynamic wing system. IMU-driven, 3-mode angle control — airbrake, cruise, and neutral. Validated by real-world on-road testing on a 2011 BMW E93.

Inspired by the variable incidence wing of the **Vought F-8 Crusader** — the only production aircraft to implement this concept — and the active lateral aero system of the **Zenvo TSR-1**.

---

![AVIWS on BMW E93](assets/on_car.jpg)

---

## Overview

Fixed rear wings represent a fundamental engineering compromise — a wing optimized for high-downforce cornering induces excessive drag on acceleration, and a wing optimized for low drag provides inadequate downforce under braking. The AVIWS resolves this by dynamically adjusting the wing's angle of incidence in real time based on the vehicle's instantaneous acceleration state, with no driver input required.

---

## Wing Modes

| Mode | Trigger | Angle | Aerodynamic Function |
|---|---|---|---|
| Neutral | Acceleration > +0.09g | −5° | Drag reduction, maximum straight-line performance |
| Cruise | \|accel\| < 0.09g | +5° | Baseline downforce, stability |
| Airbrake | Deceleration > −0.07g | +40° | Maximum drag and downforce, braking aid |

---

## CFD Results — 30 mph (13.4 m/s)

Simulated in Autodesk Fusion CFD 2027, k-epsilon turbulence model, steady-state incompressible flow.

| Mode | Angle | Downforce | Drag | D/D Ratio |
|---|---|---|---|---|
| Neutral | −5° | 88.4 N | 9.4 N | 9.4 : 1 |
| Cruise | +5° | 882.4 N | 10.3 N | 85.7 : 1 |
| Airbrake | +40° | 1,708.8 N | 262.0 N | 6.5 : 1 |

The airbrake position generates a **2,443% increase in drag** over cruise, driven by flow separation off the upper surface at high angle of attack.

---

## Hardware

| Component | Part | Purpose |
|---|---|---|
| Microcontroller | Arduino Nano | Control logic |
| IMU | GY-521 MPU-6050 | Acceleration sensing |
| Servo x2 | DS3218 (20 kg·cm) | Wing actuation |
| Power | LM2596 Buck Converter | 12V → 5V regulation |
| Capacitor | 100µF electrolytic | Rail voltage stability |

---

## Wing Specifications

- **Total span:** 1400mm tip to tip
- **Center element:** 900mm, primary downforce surface
- **Tip sections:** 250mm each, lofted transition for spanwise load distribution
- **Actuation range:** −5° to +40° (45° total sweep)
- **Fabrication:** FDM 3D printed PLA, modular 200mm sections joined with 10mm dowel pins and plastic welded

---

## Gallery

![CAD render](assets/cad_render.jpg)

![CFD airbrake](assets/cfd_40deg.jpg)

![Electronics](assets/electronics.jpg)

![Assembled wing](assets/assembled_wing.jpg)

---

## Firmware

Written in C++ on Arduino IDE. Three-state bang-bang controller with:
- 10-sample moving average filter on IMU data
- 0.07g empirical gravity offset calibration
- 1-second debounce delay on all mode transitions
- Mirrored servo logic for symmetric actuation

See [`firmware/`](firmware/) for full source code.

---

## Repository Structure

firmware/ — Arduino .ino source code

docs/ — Full project report PDF

assets/ — Images used in this README

---

## Future Work

- Lateral aero axis — second servo for cornering yaw stabilization (Zenvo TSR-1 inspired)
- ESP32 migration for real-time WiFi telemetry
- Permanent trunk-mounted bracket replacing suction cup system
- CF-PLA connecting arm for improved structural margin

---

## Author

**Arsh Hashmi**
Mechanical Engineering + Computer Science, Binghamton University
Class of 2029

---

## License

MIT License — see [LICENSE](LICENSE)
