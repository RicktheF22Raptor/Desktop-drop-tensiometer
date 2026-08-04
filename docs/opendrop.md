# OpenDrop Installation

This project uses **OpenDrop** to analyze pendant drop images and calculate surface tension. The hardware and additional software in this repository is designed to produce images that are compatible with OpenDrop's **Pendant Drop** analysis mode.

> **Official Documentation:**
> https://opendrop.readthedocs.io/en/latest/

---

# System Requirements

OpenDrop supports:

* Windows
* Linux
* macOS

---

# Installation

## Option 1 – Download a Release (Recommended)

1. Visit the OpenDrop GitHub releases page:

   * https://github.com/jdber1/opendrop/releases
2. Download the latest release for your operating system.
3. Install or extract the application.
4. Launch **OpenDrop**.

---

## Option 2 – Install from Source

If you wish to modify or contribute to OpenDrop, follow the installation instructions in the official documentation:

https://opendrop.readthedocs.io/en/latest/

---

# Using OpenDrop

After launching OpenDrop:

1. Select **Pendant Drop** mode.
2. Connect your USB camera to the USB port of your computer.
3. Select cv2.VideoCapture as you image source
4. select your desired number of images and capture interval
5. select "1" as your video capture source
6. Focus the camera until the droplet edge is sharp.
7. Enter the **outside diameter** of the dispensing needle.
8. Verify that the detected droplet outline matches the actual droplet.
9. Run the analysis.
10. .

---

# Image Quality Tips

For the most accurate results:

* Ensure the droplet is in sharp focus.
* Keep the camera perpendicular to the droplet.
* Avoid vibrations during image capture.

---

# Calibration

OpenDrop uses the **outside diameter of the dispensing needle** to convert image pixels into real-world dimensions.

For accurate measurements:

1. Measure or verify the needle's outside diameter.(Use a caliper for best result)
2. Enter this value into OpenDrop before analysis.
3. Confirm that the detected needle edges align with the image.(you will see a stable Yellow marker on the edge of the needle, if it is constantly shifting or or not present, adjust your needle)

Incorrect calibration will directly affect the calculated surface tension.

---

# Workflow

```text
Prepare Sample
      │
      ▼
Form Pendant Drop
      │
      ▼
Capture Image
      │
      ▼
Open in OpenDrop
      │
      ▼
Enter Needle Diameter
      │
      ▼
Verify Edge Detection
      │
      ▼
Run Analysis
      │
      ▼
Export Results
```

---

# Troubleshooting


---

# Additional Resources

* **OpenDrop Documentation:** https://opendrop.readthedocs.io/en/latest/
* **OpenDrop GitHub Repository:** https://github.com/jdber1/opendrop

For more detailed information on OpenDrop's features, supported cameras, and advanced analysis options, refer to the official documentation.



# Camera Orientation

To maximize the usable field of view, the camera is mounted in **portrait orientation** (rotated 90° clockwise or counterclockwise). Because the camera sensor is physically rotated, the video stream must also be rotated in your operating system so that the image appears upright in OpenDrop.

This configuration provides significantly more vertical image area, allowing larger pendant drops to remain in frame while maintaining the camera's full resolution.

---

## Step 1 – Rotate the Camera

Install the camera in the mount rotated **90°** from its normal landscape orientation.

Instead of this:

```text
┌────────────────────┐
│                    │
│                    │
│                    │
└────────────────────┘
Landscape
```

Mount it like this:

```text
┌────────┐
│        │
│        │
│        │
│        │
│        │
└────────┘
Portrait
```

---

## Step 2 – Rotate the Video Feed

Because the camera is now mounted sideways, the image will also appear sideways until it is rotated in software.

### Windows 11

Windows 11 can rotate the video stream from many USB (UVC) cameras.

1. Open **Settings**.

2. Navigate to **Bluetooth & devices → Cameras**.

3. Select your USB camera.

4. Under **Video rotation**, select:

   * **Right 90°** or
   * **Left 90°**

   Choose the option that makes the needle vertical.

5. Close and reopen OpenDrop if it was already running.

> **Note:** The **Video rotation** option is only available if the camera and its driver support this feature. Windows stores this setting for the selected camera and automatically applies it whenever supported applications access the camera.

---

### macOS

macOS does **not** provide a system-wide rotation setting for external USB webcams.

If your camera is mounted in portrait orientation, you have two options:

#### Option 1 (Recommended)

Rotate the image using the camera manufacturer's software, if available.

#### Option 2

Use a virtual camera application such as **OBS Studio**:

1. Add the USB camera as a **Video Capture Device**.
2. Right-click the camera source.
3. Select **Transform → Rotate 90° Clockwise** (or Counterclockwise).
4. Start the **OBS Virtual Camera**.
5. Select **OBS Virtual Camera** as the camera source in OpenDrop (if your version supports virtual cameras).

If you are capturing still images outside of OpenDrop, simply rotate the image before importing it for analysis.

---

## Verify Orientation

Before beginning any measurements, confirm that:

* ✅ The needle is perfectly vertical.
* ✅ The droplet hangs straight downward.
* ✅ The entire droplet fits within the image.
* ✅ The needle tip is near the top of the frame.
* ✅ There is sufficient space below the droplet for larger drops.
* ✅ The droplet is sharply focused.

A correctly configured image should appear similar to the example below:

```text
        Needle
          │
          │
          ▼
         ( )
        (   )
       (     )
        (   )
         ( )
```

A correctly rotated image maximizes the vertical field of view and allows larger droplets to be analyzed without sacrificing image resolution.

