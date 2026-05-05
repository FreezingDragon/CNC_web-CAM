# CNC CAM — DXF to G-code Converter

[Japanese](README.md) | English

**A browser-based DXF to G-code CAM tool. No installation required.**

🔗 **[Open the Tool](https://freezingdragon.github.io/CNC_web-CAM/cnc-cam.html)**

---

## Overview

A lightweight CAM tool optimized for Genmitsu 3018Pro / 3040 (GRBL). Compatible with most GRBL-based CNC routers.

Drop a DXF file into the browser and generate cutting G-code instantly. No server, no installation — runs entirely as a single HTML file.

## Features

- **Direct DXF import** — Supports VectorWorks, Illustrator, and Affinity
- **SPLINE support** — High-accuracy NURBS linearization via De Boor algorithm
- **Toolpath chain optimization** — Automatically joins connected segments to minimize Z plunges (up to 70% reduction in machining time)
- **G2 arc output** — Converts CIRCLE entities to arc G-code
- **Work offset** — Shift the entire toolpath to any position
- **Bounding box trace** — Generates a separate dry-run G-code file for range verification before cutting
- **2.5D support** — Multiple layers with different cutting depths (multi-pass 2.5D machining)
- **Tab support** — Set tabs on any layer for profile cutout operations
- **Travel limit validation** — Alerts when toolpath exceeds configured machine travel

## Supported DXF Entities

| Entity | Support | Notes |
|---|---|---|
| LINE | ✅ | |
| LWPOLYLINE | ✅ | Vertices automatically decomposed |
| SPLINE (degree-3 NURBS) | ✅ | Linearized via De Boor algorithm |
| CIRCLE | ✅ | Output as G2 arc |

## Supported DXF Software

| Software | Notes |
|---|---|
| VectorWorks | Direct export supported (no Illustrator conversion needed) |
| Illustrator | Auto-corrects $INSUNITS inch scaling (×25.4) |
| Affinity Designer / Publisher | Handles CR-only line endings and UTF-8 encoding |

## How to Use

1. **[Open the tool](https://freezingdragon.github.io/CNC_web-CAM/cnc-cam.html)**
2. Drag and drop your DXF file onto the drop zone
3. Configure layer names and cutting parameters
4. Click **◈ Parse** to load the DXF
5. Click **⬜ Trace** to generate a dry-run G-code for range verification
6. Click **⚙ Generate G-code** to create the cutting G-code
7. Download the `.nc` file and run it with CNCjs or Candle

## DXF Layer Setup

| Layer | Purpose | Geometry |
|---|---|---|
| Cut layer (default: `L1`) | Cutting path | LINE / LWPOLYLINE / SPLINE / CIRCLE |
| Origin layer (default: `ORIGEN`) | Work origin definition | CIRCLE (exactly one) or SPLINE |

## Generated G-code Structure

```gcode
G21        ; mm mode
G90        ; absolute coordinates
M3 S10000  ; spindle on
G4 P2.0    ; warmup delay
G0 Z5.0    ; move to safe height
; --- depth pass loop ---
G0 Z5.0
G0 X-10.0 Y-20.0
G1 Z-0.5 F150
G1 X-30.0 Y-20.0 F500
; --- end ---
M5         ; spindle stop (highest priority)
G0 Z30.0   ; final retract
G0 X0 Y0
M2
```

## Parameters

| Parameter | Default | Description |
|---|---|---|
| Total depth | 2.0mm | Final cutting depth |
| Step per pass | 0.5mm | Depth increment per pass |
| Feed rate (XY) | 500mm/min | Horizontal cutting speed |
| Feed rate (Z) | 150mm/min | Plunge speed |
| Safe height | 5.0mm | Retract height between moves |
| Final retract | 30.0mm | Retract height at end of program |
| Spindle S value | 10000 | RPM reference ($30) |
| SPLINE tolerance | 0.05mm | Linearization accuracy for curves |
| X/Y offset | 0mm | Work positioning offset |
| Scale correction | 1.0 | Auto-detected for Illustrator inch export |

## Requirements

- Chrome / Edge (recommended)
- Firefox / Safari (functional with minor limitations)
- No server required — works fully offline

## Target Machines

- Genmitsu 3018Pro / 3040
- Any CNC router running GRBL
- Machine home position: right-rear (X+/Y+ limit switches)

## Related Article

[Replacing NCVC & Candle with a Browser-based CAM Tool](https://note.com/freezing_dragon/n/n458ccc960970?app_launch=false)

## Feedback & Feature Requests

Feel free to open an [Issue](https://github.com/FreezingDragon/CNC_web-CAM/issues) for bug reports, feature requests, or questions.

## License

MIT License

---

*Made by [@FreezingDragon](https://github.com/FreezingDragon)*
