# Usage Guide

This guide covers day-to-day operation of the instrument using **[OpenDrop](https://github.com/jdber1/opendrop)**, the open-source software this project uses for image capture, edge detection, and Young-Laplace fitting. It assumes the hardware is already built ([Build Instructions](assembly.md)).

## Before You Start

- Confirm calibration is still valid for your current camera/needle setup.
- Have your sample liquid prepared and the syringe loaded with air bubbles cleared.

## 1. Launch OpenDrop and Connect the Camera

1. Open OpenDrop.
2. Select the **Live Video** option as your image source.
3. Choose your camera — this is usually **1**
4. Confirm a live preview appears before continuing.

## 2. Set Up Camera Orientation

To maximize the usable field of view, the camera in this design is mounted in **portrait orientation** (rotated 90° from its default landscape orientation). Because most USB cameras natively capture in landscape, the video stream needs to be rotated in your OS so it appears upright in OpenDrop.

**Windows 11**

1. Open **Settings → Bluetooth & devices → Cameras**.
2. Select your USB camera.
3. Under **Video rotation**, choose **Right 90°** or **Left 90°** — whichever makes the needle appear vertical in OpenDrop's preview.
4. Close and reopen OpenDrop if it was already running.

> If the **Video rotation** option isn't available, your camera/driver doesn't support it — use the macOS/alternative method below instead.

**macOS**

macOS has no built-in rotation setting for external USB webcams. Options, in order of preference:

1. Use the camera manufacturer's software to rotate the stream, if it offers one.
2. Use **OBS Studio** as a virtual camera: add the USB camera as a Video Capture Device, right-click it → **Transform → Rotate 90°**, start the **OBS Virtual Camera**, then select it as the source in OpenDrop (if your OpenDrop version supports virtual camera sources).
3. If capturing still images outside OpenDrop entirely, rotate them before importing.

**No software rotation available**

Rotate the camera physically 90° in its mount instead, so the sensor itself captures portrait images.

**Verify before continuing** — in the OpenDrop preview, confirm:

- ✅ Needle is perfectly vertical
- ✅ Needle tip is near the top of the frame
- ✅ There's enough space below the needle for the droplet to grow into
- ✅ Image is in focus

## 3. Frame the Needle

1. Adjust the camera mount (or reposition the frame) so the needle is horizontally centered in the OpenDrop preview.
2. Confirm the full expected droplet size will fit within the frame — grow a test droplet if unsure, then retract it before your real measurement.

## 4. Set Up a New Measurement in OpenDrop

1. In OpenDrop, start a new **Pendant Drop** experiment/project.
2. Enter the needle's measured outer diameter when prompted for the reference length. OpenDrop uses this to convert pixel measurements to physical units.


## 5. Set Image Count and Interval

Before capturing, set how many images OpenDrop should take and the time interval between them:

1. Set the **number of images** to capture — a single image is enough for a static measurement; capture a series (with an interval) if you want to track surface tension over time (e.g., for surfactant adsorption studies).
2. Set the **interval** between images (in seconds).
3. Slowly advance the syringe plunger (manually, or via the motorized stage if installed) until a droplet forms at the needle tip, and let it settle before starting capture — a droplet that's still growing or oscillating won't represent true equilibrium and will bias the fit.

## 6. Improve Contrast, If Needed

Because this hardware places the droplet close to the LED backlight for even illumination, raw images often have lower droplet-to-background contrast than commercial systems produce. As a result, the droplet may appear light gray rather than nearly black, which can cause OpenDrop's edge detection to struggle once you run the analysis (step 7).

If this happens, run your images through the contrast-enhancement tool before analyzing:

1. Save your captured image(s) from OpenDrop.
2. Upload the saved image(s) to the image processing tool (**[hosted at: URL, or run locally from `tools/image-processor/`]**).
3. Wait for processing to complete, then download the processed images.
   - The tool increases contrast between the droplet and background and darkens the droplet silhouette, while preserving the true droplet geometry — no geometric transformations or scaling are applied, so the droplet shape used for the fit is unaffected.
   - The result should show a noticeably darker droplet silhouette against a brighter, more uniform background, similar to the high-contrast examples in the OpenDrop documentation.
4. In OpenDrop, open the processed image(s) in place of the originals.
5. Continue to step 7 to run the analysis.

This contrast-enhancement step is a normal part of the workflow for this hardware, not a workaround — treat it as **Capture → Enhance → Analyze** whenever raw contrast looks low.

## 7. Run the Analysis

This is the easy part — OpenDrop handles edge detection and the Young-Laplace fit together behind a single **Analyze** button.

1. Click **Analyze**.
2. OpenDrop detects the droplet edge, fits it to the Young-Laplace profile, and reports the surface tension along with the fitted profile overlaid on the image.
3. Visually check that the overlaid fit tracks the actual droplet outline along its full length, not just near the apex. If it doesn't — or if OpenDrop flags a poor fit — the usual causes are low contrast, poor focus, or the needle/droplet being out of frame; see [Troubleshooting](troubleshooting.md), and step 6 above if contrast looks like the issue.

## 8. Save the Results

1. Use OpenDrop's save/export function to save the results (surface tension value, and image series results if you captured more than one image).
2. Save alongside the liquid identity, temperature, and date for your own records — OpenDrop won't capture this context automatically.
3. Keep exported results in [`Data/`](../Data/) for your records.
4. For a first-time setup or after any recalibration, run this full workflow on distilled water and compare against the literature value (72.8 mN/m at 20 °C) before trusting results on unknown samples — see [Calibration → Baseline Validation](calibration.md#baseline-validation).

## Quick Reference

```
Launch OpenDrop → Live Video → select camera (usually Camera 1) → check orientation
   → frame needle → set image count + interval → generate droplet → capture
   → Analyze → [enhance contrast + re-analyze if needed] → Save results
```

For hardware assembly, see [Build Instructions](assembly.md). For the physics behind the fit, see [Working Principle](theory.md). For anything that isn't working as expected, see [Troubleshooting](troubleshooting.md).
