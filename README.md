# Rakshak Vayu - Low-Cost AI-Enabled Autonomous Multipurpose Survival Drone

A low-cost quadcopter for **disaster response and public-safety monitoring**. The target
system combines a stabilised flight platform with an onboard camera and AI models that
detect disasters (fire, smoke, flood, debris) and locate trapped people, with live alerts
sent to a smartphone app.

> **Status: ongoing project.** The airframe, flight electronics and power system are
> built and bench-tested (see the technical report). The AI detection pipeline and
> autonomous decision logic are the target architecture shown below and are under development.

## Photos
<p>
  <img src="images/drone_body.jpg" alt="Drone body and arms" width="260">
  <img src="images/drone_battery_camera.jpg" alt="Drone underside with battery and camera" width="260">
</p>

## System architecture
![System block diagram](images/system_block_diagram.png)

**Flight side:** a ground station / smartphone app sends manual flight commands to the
flight controller (MCU). The controller runs a PID loop with gyro and barometer feedback and
drives four motors through a 4-in-1 ESC.

**Vision and AI side:** an onboard camera feeds a preprocessing stage, then two models:
a CNN disaster classifier (fire / smoke, flood, structural debris) and a person-detection
model (bounding boxes and human-presence alert). Results are streamed to the app over Wi-Fi.

**Power:** 1S LiPo, push-button power switch, 5 V / 3.3 V regulator, VBAT to the ESC.

## Hardware platform
From the technical report (`docs/Drone_Technical_Report.docx`):

| Subsystem | Component |
|-----------|-----------|
| Propulsion | 4 x 1503 brushless motors, 4-in-1 ESC |
| Flight controller MCU | STM32F030F4P6 (ARM Cortex-M0, 48 MHz) |
| Flight firmware | Betaflight-based build |
| Gyroscope | L3GD20H (3-axis) |
| Barometer | BMP180 |
| RF command link | XN297LBW 2.4 GHz transceiver |
| Video / Wi-Fi camera | Lewei LW9809 |
| Battery | 1S 3.7 V, 1800 mAh LiPo |
| All-up weight | 104.5 g |

## Test finding: why the first flight lasted only 7.7 s
The report traces the short flight to the battery, not the motors. The 1S cell measured about
102 mOhm internal resistance, so at about 16.8 A the pack voltage collapsed to roughly 2.4 V.
That dropped the 3.3 V regulator out of regulation and triggered the MCU brown-out reset,
which disarmed the motors.

**Fixes proposed in the report:**
1. Use a high-discharge 1S pack (30C continuous, internal resistance under 15 mOhm).
2. Move to a 2S (7.4 V) pack, which halves the current for the same thrust.

## Repository contents
| Path | Contents |
|------|----------|
| `images/` | Drone photos and system block diagram |
| `docs/Drone_Technical_Report.docx` | Hardware specification and test report |
| `docs/system_block_diagram.pdf` | Block diagram (PDF) |

## References
- [Betaflight - Firmware Installation guide](https://betaflight.com/docs/wiki/getting-started/firmware-installation)

## Roadmap
- Integrate the camera with the onboard CNN disaster and person-detection models
- Add Python control and decision logic for autonomous navigation
- Retest flight time with a high-discharge battery

## Author
Kavana N | Sir MVIT, Bengaluru
