# Hardware

## Camera

## Lighting

## Frame

## Electronics
# USB Camera + Potentiometer-Controlled LED — Wiring Guide

A single protoboard hosts a USB-connected camera and a potentiometer-dimmed LED, both powered from the same USB 5V/GND source.

## Parts

| Part | Link | Notes |
|---|---|---|
| Arducam 1080P USB2 UVC camera module | [product page](https://www.arducam.com/arducam-1080p-ultra-low-light-100-degree-wide-angle-usb2-uvc-camera-module.html) | 5V, ~1W max, UVC plug-and-play |
| 10K linear potentiometer | [Adafruit #562](https://www.adafruit.com/product/562) | Breadboard-friendly, 0.2" leg spacing |
| Round LED backlight, 50mm | [Adafruit #4882](https://www.adafruit.com/product/4882) | ~3V @ 20mA, needs series resistor |
| ElectroCookie solderable protoboard | [Amazon listing](https://www.amazon.com/ElectroCookie-Solderable-Breadboard-Electronics-Gold-Plated/dp/B081MSKJJX) | Standard breadboard hole pattern |
| 100Ω resistor, 1/4W, 5% | [E-Projects pack of 100](https://www.amazon.com/Projects-100EP514100R-100-Resistors-Pack/dp/B00AVSDYFO) | Current limiter for the LED |

## Resistor calculation

```
R = (V_supply − V_forward) / I_forward
R = (5V − 3V) / 0.02A
R = 100Ω
```

Note: with a 10KΩ pot in series, most of the knob's rotation will barely dim the LED — visible dimming happens mostly in the first portion of the sweep, since 10K is much larger than the 100Ω needed for the LED's full range.

## Circuit overview

```
USB (computer)
   │
   ├── 5V / GND / D+ / D−  →  camera (data + power, untouched)
   │
   └── 5V / GND  →  potentiometer → 100Ω resistor → LED → GND
```

## Protoboard hole map

Standard breadboard numbering: columns `1, 2, 3…`, rows `A–E` (top half) and `F–J` (bottom half). Within a column, `A–E` are tied together, and `F–J` are tied together separately — the two halves only connect if jumpered across the center gap.

### USB / camera junction — columns 1–4, rows A and E

| Wire | Position |
|---|---|
| USB 5V (red) | `A1` |
| USB GND (black) | `A2` |
| USB D+ | `A3` |
| USB D− | `A4` |
| Camera 5V lead | `E1` |
| Camera GND lead | `E2` |
| Camera D+ lead | `E3` |
| Camera D− lead | `E4` |

### Power taps (jumper across center gap)

| Jumper | From | To |
|---|---|---|
| VCC tap | `C1` | `F1` |
| GND tap | `C2` | `F2` |

### Potentiometer (Adafruit #562)

| Leg | Position | Notes |
|---|---|---|
| Outer leg 1 | `F1` | Same net as VCC tap |
| Wiper (middle) | `F3` | Circuit output |
| Outer leg 2 | `F5` | Leave unconnected |

### 100Ω resistor

| Lead | Position |
|---|---|
| Lead 1 | `H3` (same net as wiper, via column 3) |
| Lead 2 | `H7` |

### LED backlight (Adafruit #4882)

| Lead | Position |
|---|---|
| Anode (+) | `G7` (same net as resistor lead 2, via column 7) |
| Cathode (−) | `G2` (same net as GND, via column 2) |

## Build notes

- Only **2 physical jumper wires** are required (`C1→F1`, `C2→F2`) — everything else relies on the board's built-in column connections.
- The column numbers above (1–7) are a relative anchor — shift the whole layout left/right on the physical board as long as spacing between components stays the same.
- Confirm VCC vs. GND on your soldered USB wires before connecting: on a standard USB-A plug, the two *outer* pins are power (VCC/GND), the two *inner* pins are data (D+/D−). A multimeter reading of ~5V between the two candidate wires confirms VCC/GND.
- Total current draw (camera ~200mA + LED ~20mA) is well within a single USB port's supply — no need for a second port or external power.
- Solder power-tap connections securely; a loose VCC/GND joint here interrupts power to the camera as well as the LED.
