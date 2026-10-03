# Assembly Guide
---

# Electronics set up:


# USB Camera + Switch/Potentiometer-Controlled LED — Wiring Guide

A single protoboard hosts a USB-connected camera and an on/off, potentiometer-dimmed LED, both powered from the same USB 5V/GND source.

## Parts

| Part | Link | Notes |
|---|---|---|
| Arducam 1080P USB2 UVC camera module | [product page](https://www.arducam.com/arducam-1080p-ultra-low-light-100-degree-wide-angle-usb2-uvc-camera-module.html) | 5V, ~1W max, UVC plug-and-play |
| 6mm trimmer potentiometer, 500Ω | [BOJACK 15-value assortment kit](https://www.amazon.com/BOJACK-Variable-Resistor-Potentiometer-Assortment/dp/B07WHJMVR4) | Use the "501" (500Ω) value from the kit |
| On-Off-On-Off alternating pushbutton | [Adafruit #1684](https://www.adafruit.com/product/1684) | 3-pin momentary switch; only common + one side used (see note below) |
| Round LED backlight, 50mm | [Adafruit #4882](https://www.adafruit.com/product/4882) | ~3V @ 20mA, needs a series resistor |
| ElectroCookie solderable protoboard | [Amazon listing](https://www.amazon.com/ElectroCookie-Solderable-Breadboard-Electronics-Gold-Plated/dp/B081MSKJJX) | Standard breadboard hole pattern |
| 150Ω resistor, 1/4W, 5% | Not linked — any standard 150Ω, 1/4W resistor works | Current limiter for the LED |

**Tools:** soldering iron + solder, wire strippers, a few short jumper wires, a multimeter (for verifying VCC/GND before powering up).

## How the circuit works

```
USB (computer)
   │
   ├── 5V / GND / D+ / D−  →  camera (data + power, untouched)
   │
   └── 5V → potentiometer (500Ω) → resistor (150Ω) → switch → LED → GND
```

The camera branch and the LED branch both pull from the same USB 5V/GND — they share power but not any other connection, so nothing on the LED side can interfere with the camera's data lines.

**Current calculation:**
```
I = (V_supply − V_LED) / (R_pot + R_fixed)
V_supply = 5V,  V_LED ≈ 3V,  R_fixed = 150Ω

Pot = 0Ω (brightest):  I = 2V / 150Ω  = 13.3 mA
Pot = 500Ω (dimmest):  I = 2V / 650Ω  ≈ 3.1 mA
Switch open:           I = 0 mA (LED off, regardless of pot position)
```

**Note on the switch:** the #1684 pushbutton cycles through 4 states per repeated presses (side A on → off → side B on → off). Only the common pin and one side pin are wired here. Practical effect: from "on," a single press always turns the LED off. From "off," it can take up to a few extra presses to cycle back to "on," since the switch is also passing through its unused side. It's a reliable off-switch; just not a perfectly symmetric single-press toggle.

## Step-by-step wiring

Breadboard numbering: columns `1, 2, 3…`, rows `A–E` (top half) and `F–J` (bottom half). Holes in the same column and same half are already tied together inside the board. The two halves only connect where you add a jumper across the center gap.

1. **Land the USB cable's 4 wires in columns 1–4, row A.**
   - VCC (red) → `A1`
   - GND (black) → `A2`
   - D+ → `A3`
   - D− → `A4`

2. **Run the camera's 4 leads into the same columns, row E** (so each camera lead shares a column — and therefore a net — with its matching USB wire):
   - Camera VCC → `E1`
   - Camera GND → `E2`
   - Camera D+ → `E3`
   - Camera D− → `E4`

3. **Add the two power jumpers**, crossing from the top half to the bottom half:
   - VCC jumper: `B1` → `H5`
   - GND jumper: `B2` → `F2`

4. **Place the potentiometer.** Its 3 legs form a triangle (round trimmer body), not a straight line:
   - Outer leg 1 → `F5` (lands on the VCC jumper's column)
   - Outer leg 2 → `F7` (leave unconnected)
   - Wiper (middle, set back one row) → `G6`

5. **Place the resistor**, bridging from the wiper's column to a fresh column:
   - Lead 1 → `H6` (same column as the wiper)
   - Lead 2 → `H9`

6. **Place the switch**, using only the common pin and one throw pin:
   - Throw pin A → `F9` (same column as resistor lead 2)
   - Common pin → `F11`
   - Other throw pin → leave unconnected, bend it out of the way

7. **Place the LED:**
   - Anode → `G11` (same column as the switch's common pin)
   - Cathode → `G2` (same column as the GND jumper)

8. **Double-check before powering on** (see Verification below), then plug the USB cable into your computer.

## Verification

- With a multimeter, confirm ~5V between `A1`/`E1` and `A2`/`E2` before connecting anything else — this confirms you've identified VCC/GND correctly on the USB wires.
- Check for continuity between `A1` and `H5` (should show the VCC jumper), and between `A2` and `F2` (GND jumper).
- Check there is **no continuity** between column 1–4 (camera/USB zone) and columns 5–11 (LED zone) except through the two jumpers — this confirms the two branches aren't accidentally shorted together.
- Plug in the camera — it should enumerate as a UVC webcam with no driver install needed.
- Turn the pot — LED brightness should sweep smoothly from dim to bright.
- Press the switch — LED should turn off from any brightness level.

## Full hole position reference

| Component | Position | Net |
|---|---|---|
| USB VCC | `A1` | VCC |
| USB GND | `A2` | GND |
| USB D+ | `A3` | D+ |
| USB D− | `A4` | D− |
| Camera VCC | `E1` | VCC |
| Camera GND | `E2` | GND |
| Camera D+ | `E3` | D+ |
| Camera D− | `E4` | D− |
| VCC jumper | `B1` → `H5` | VCC |
| GND jumper | `B2` → `F2` | GND |
| Pot outer leg 1 | `F5` | VCC |
| Pot outer leg 2 | `F7` | unconnected |
| Pot wiper | `G6` | Wiper |
| Resistor lead 1 | `H6` | Wiper |
| Resistor lead 2 | `H9` | R-out |
| Switch throw A | `F9` | R-out |
| Switch common | `F11` | Switched |
| LED anode | `G11` | Switched |
| LED cathode | `G2` | GND |

## Design notes

- **Camera and LED branches use entirely separate columns** (1–4 vs. 5–11) so a stray probe or wire can't accidentally bridge an unrelated net.
- **Only 2 jumpers are used**, because only VCC and GND need to reach the bottom half — the USB data lines (D+/D−) stay entirely within the top half between the USB cable and the camera, so they never need to cross the gap.
- **Component order (pot → resistor → switch → LED) doesn't change the math** — it's a series loop, so total resistance and current are the same regardless of order. The resistor sits right before the LED so the safety-critical current limit stays physically close to the part it's protecting.
- **The pot's wiper lands one row below its outer legs** (`G6` instead of the same row as `F5`/`F7`) because the round trimmer's wiper pin is physically set back from the two front legs — the hole map follows the part's real shape rather than forcing it into a straight line.

## Build notes

- Total current draw (camera ~200mA + LED ~13.3mA max) is well within a single USB port's supply — no second port or external power needed.
- The pushbutton's leads aren't on a perfect 0.1" grid — bend them slightly to seat cleanly in `F9`/`F11`.
- Solder power connections securely; a loose VCC/GND joint interrupts power to the camera as well as the LED branch, since both share the same jumpers.


# Procedure(V1)
### 1. After printing all of the parts, use the light holder and place the LED in the circular slot, make sure the leads go through the other side 
<img width="924" height="638" alt="image" src="https://github.com/user-attachments/assets/1238b57d-3583-4964-a1bd-b0d19f2b7230" />

### 2. Use 4 m2.5 screws and nuts to secure the usb camera, ensure that the connector is place vertically on the left or right, a 90 degree rotation from the standard position)
<img width="887" height="521" alt="image" src="https://github.com/user-attachments/assets/0ee70b0a-0243-4626-a654-63a97b4e33e2" />

### 3. Follow the electronics assembly instructions, mount the board to the apparatus, using 4 m2 screws on each corner tot the inside back wall in the camera chamber.
<img width="724" height="469" alt="image" src="https://github.com/user-attachments/assets/1bb43d69-d72e-4ce7-ba8c-957ab6863ea5" />

### 4. Place lid on the apparatus and procced with usage guide
<img width="262" height="320" alt="image" src="https://github.com/user-attachments/assets/45cc23df-eb20-4f67-ad35-859a070920cf" />


# Procedure (V2)

V2 uses a different light source and resistor value, which provides sufficient droplet/background contrast directly — no image processing step is required before analysis. Electronics, mounting hardware, and general assembly are otherwise identical to V1 except where noted below. The wiring diagram can be followed as previously described by switiching out the resistor.

Parts that differ from V1:

Part	Link	Notes
[V2 light source]	[link]	[voltage/current spec]
[V2 resistor value]	[link, or "any standard X, 1/4W resistor works"]	[what it's current-limiting]

Steps:

### 1. After printing all of the parts, Place the assembled electronics inside the plate making sure it securely screwed in, with all components connected piror to installation
<img width="789" height="509" alt="Screenshot 2026-10-03 131432" src="https://github.com/user-attachments/assets/05ee5682-949e-4e52-aac9-4e605a5515d5" />

### 2. Use 4 m2.5 screws and nuts to secure the usb camera, ensure that the connector is place vertically on the left or right, a 90 degree rotation from the standard position)
<img width="697" height="429" alt="Screenshot 2026-10-03 135050" src="https://github.com/user-attachments/assets/6accb293-fd52-4cca-b248-42233e108e84" />


### 3. Place light in vertical slot with wires pointing up as they will be threaded through the lid slot 
<img width="715" height="527" alt="Screenshot 2026-10-03 135921" src="https://github.com/user-attachments/assets/aec0714b-9165-4b6b-9580-495570e1ef9b" />


### 4. Place the lid and connect wires for the ligh tthrought the hole in the side and procced with usage guide.
<img width="733" height="473" alt="Screenshot 2026-10-03 140202" src="https://github.com/user-attachments/assets/54648f26-da62-488b-911f-c777b7f5d774" />


