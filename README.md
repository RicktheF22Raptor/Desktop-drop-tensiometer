# Open Drop Tensiometer

An open-source, low-cost **pendant drop tensiometer** — a 3D-printable, DIY alternative to commercial surface tension instruments such as the FTÅ200, built for research and educational labs that don't have **[$15,000+]** for a proprietary system.

**[Insert device photo here]**

---

## Overview

This repository contains everything needed to build, calibrate, and operate an open-source pendant drop tensiometer: mechanical designs, electronics, firmware, software, and full documentation. The instrument measures liquid surface tension by imaging a suspended droplet and fitting its shape to the Young-Laplace equation — the same principle used by commercial pendant drop systems.

Image capture and Young-Laplace analysis are performed using [OpenDrop](https://github.com/jdber1/opendrop), an existing open-source tool. This project provides the hardware platform, build documentation, and a supporting image-processing tool that improves droplet contrast for this design's optical layout.

## Motivation

Commercial pendant drop tensiometers are accurate but expensive, putting hands-on surface tension measurement out of reach for many teaching labs and small research groups. This project aims to close that gap with a fully open, reproducible instrument — see [Background](docs/background.md) for the full motivation and context.

## Features

-  Pendant drop surface tension measurement via Young-Laplace fitting
-  Fully 3D-printable frame and mounts
-  Off-the-shelf electronics
-  Built on [OpenDrop](https://github.com/jdber1/opendrop) for image capture and analysis
-  Custom contrast-enhancement tool for reliable edge detection on this hardware's optical path
-  Complete, from-scratch build and calibration documentation


## Quick Start

1. **Read the background and theory** — [Background](docs/background.md).
2. **Order parts and print components** — see the [Bill of Materials](docs/bill_of_materials.md) and [Build Instructions](docs/assembly.md).
3. **Assemble the hardware** — follow [Build Instructions](docs/assembly.md) step by step, including firmware upload and software install.
4. **Take a measurement** — follow the [Usage Guide](docs/usage.md), 


## Contributing

Contributions are welcome — bug reports, design improvements, and documentation fixes alike.

## License

This project is released under the **[license name — e.g., MIT / CERN-OHL-S / CC-BY-SA]** license. See [LICENSE](LICENSE) for the full text.

---

*Built as an open, reproducible alternative to commercial pendant drop tensiometers like the FTÅ200 — for labs and classrooms that need real measurement capability without the price tag.*
