# WIFIfence

### Camera-free virtual boundaries using Wi-Fi CSI sensing.

WIFIfence is an experimental hardware + software project that explores whether everyday Wi-Fi signals can be used as a privacy-first indoor security layer. Instead of using cameras, microphones, or wearables, WIFIfence uses an ESP32-based Wi-Fi sensing node to collect signal changes from a room and detect movement near user-defined virtual boundaries.

The goal is simple:

> Draw an invisible line in a room. Let Wi-Fi sense movement near it. Get alerted when that boundary appears to be crossed.

---

## Why WIFIfence?

Most indoor security systems rely on cameras or motion sensors. Cameras can feel invasive inside private spaces such as bedrooms, dorm rooms, study areas, or shared homes.

WIFIfence explores a different idea: using the Wi-Fi signals already present in indoor spaces as a low-cost, camera-free sensing layer.

This project is not meant to replace a professional security system. It is a research-style prototype for learning about wireless sensing, embedded systems, signal processing, and real-time alerting.

---

## Core Idea

Wi-Fi signals do not travel through a room in a perfectly straight path. They reflect off walls, furniture, doors, and human bodies. When a person moves through a room, the wireless channel changes.

WIFIfence uses this idea to build a virtual boundary system:

1. A user creates a simple room map.
2. The user places a red virtual fence line near a door, hallway, or protected area.
3. An ESP32 collects Wi-Fi Channel State Information.
4. The system calibrates the normal empty-room signal pattern.
5. Movement near the boundary creates signal disturbances.
6. If the disturbance matches a possible crossing event, WIFIfence logs it and sends an alert.

---

## Why CSI Instead of Cameras?

WIFIfence focuses on **Channel State Information (CSI)** rather than video or audio.

CSI gives a lower-level view of how Wi-Fi signals are changing across subcarriers. Each received packet can contain information about amplitude and phase changes in the wireless channel.

A simplified representation of the wireless channel is:

```txt
H(f, t) = |H(f, t)| e^(j∠H(f, t))
```

Where:

* `|H(f, t)|` represents signal amplitude changes
* `∠H(f, t)` represents phase changes
* `f` represents subcarrier frequency
* `t` represents time

These changes can be processed over time to detect motion patterns inside a room.

---

## Planned Signal Processing Pipeline

WIFIfence is being built around a CSI-first sensing pipeline.

The planned pipeline includes:

1. **CSI Capture**
   ESP32 firmware collects raw CSI frames from Wi-Fi packets.

2. **Serial Data Ingestion**
   CSI frames are streamed from the ESP32 to a local Python backend.

3. **Baseline Calibration**
   The system records an empty-room baseline to understand normal environmental noise.

4. **Phase and Amplitude Processing**
   CSI amplitude and phase data are cleaned and prepared for analysis.

5. **Filtering**
   Signal filters are used to reduce static noise and focus on human-scale motion changes.

6. **Dimensionality Reduction**
   PCA or similar techniques may be used to extract dominant movement-related features from noisy CSI data.

7. **Boundary Event Detection**
   Processed signal changes are compared against calibrated crossing patterns to detect possible boundary events.

8. **Alert Dispatch**
   If a boundary crossing is detected, the event is logged and a Telegram alert is sent.

---

## System Architecture

| Layer             | Purpose                                    | Tools                          |
| ----------------- | ------------------------------------------ | ------------------------------ |
| Edge Hardware     | Collect Wi-Fi CSI data                     | ESP32                          |
| Firmware          | Configure Wi-Fi sensing and stream packets | C++ / PlatformIO               |
| Data Ingestion    | Receive CSI frames from the ESP32          | Python / Serial / WebSockets   |
| Signal Processing | Filter, clean, and analyze signal changes  | NumPy / SciPy / Scikit-learn   |
| Detection Logic   | Identify possible boundary-crossing events | Calibration + threshold models |
| Dashboard         | Display room map, signal data, and events  | Next.js / React / Tailwind CSS |
| Alerts            | Notify user when boundary is crossed       | Telegram Bot API               |

---

## Current Development Plan

WIFIfence is being developed in stages so each version can be tested and demonstrated clearly.

### Version 0.1 — Control Dashboard

* Build the room map UI
* Add a red virtual boundary line
* Add simulated movement crossing
* Log crossing events in the dashboard

### Version 0.2 — Alert System

* Connect Telegram Bot API
* Send an alert when the simulated boundary is crossed
* Add timestamps and event history

### Version 0.3 — ESP32 CSI Collection

* Set up ESP32 firmware
* Enable CSI data collection
* Stream raw CSI data to the laptop
* Save captured data for analysis

### Version 0.4 — Live Signal Dashboard

* Plot incoming CSI/RSSI test data
* Show signal changes over time
* Compare empty-room data with movement data

### Version 0.5 — Calibration Mode

* Record an empty-room baseline
* Guide the user through 30-second mapping samples
* Mark low-confidence or under-sampled areas on the room map

### Version 1.0 — Working Prototype

* Combine room map, ESP32 signal data, calibration, and alerts
* Detect possible movement near a virtual boundary
* Trigger a real-time alert when the boundary appears to be crossed

---

## Features

* [ ] Wireframe room map dashboard
* [ ] Red virtual fence line
* [ ] Movement simulation mode
* [ ] Boundary crossing event log
* [ ] Telegram alert integration
* [ ] ESP32 CSI data collection
* [ ] Live signal graph
* [ ] Empty-room baseline calibration
* [ ] Guided room mapping mode
* [ ] Low-confidence area indicators
* [ ] Real movement-based boundary detection

---

## Privacy & Safety

WIFIfence is designed to be privacy-first.

It does not record video.
It does not record audio.
It does not decode Wi-Fi traffic.
It only studies wireless signal changes caused by movement in a controlled environment.

This project should only be tested in spaces where the builder has permission.

---

## Limitations

WIFIfence is an experimental prototype.

Wi-Fi sensing can be affected by router placement, furniture, walls, interference, body position, and other people moving nearby. The project does not claim perfect tracking, exact 3D mapping, or guaranteed security detection.

The first goal is to build a working camera-free virtual boundary prototype and learn from real CSI data.

---

## AI Usage

I used AI tools such as Codex and ChatGPT as coding assistants while building this project. They helped with planning, debugging, code explanations, and small starter snippets.

I reviewed the code, tested it locally, changed parts when needed, and made sure I understood the main logic before including it in the project. AI was used as a helper, not as a replacement for building, testing, or understanding the project.

---

## License

This project is licensed under the MIT License.

---

## Author

Built by **Vihaan Pandya** as an open-source experiment in Wi-Fi sensing, embedded systems, signal processing, and privacy-first indoor security.
