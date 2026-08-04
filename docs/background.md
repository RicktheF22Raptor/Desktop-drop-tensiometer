# Background
Surface Tension:

Surface tension is the tendency of a liquid surface to minimize its area, arising from cohesive forces between liquid molecules. Molecules at the surface experience a net inward force (since they lack neighboring molecules above them), which creates an effective "skin" that resists deformation. Surface tension is typically reported in millinewtons per meter (mN/m), equivalent to dynes per centimeter (dyn/cm).

Surface tension governs a wide range of physical and biological phenomena, including droplet formation, capillary rise, wetting behavior, foam and emulsion stability, and the spreading of coatings and inks. It is a key parameter in fields such as:

Pharmaceutical formulation (surfactant characterization, drug delivery)
Food science (emulsifiers, foams)
Materials science (coatings, adhesives, wetting agents)
Petroleum engineering (interfacial tension in enhanced oil recovery)
Biology and biochemistry (cell membrane behavior, protein adsorption)
Pendant Drop Tensiometry

Pendant drop tensiometry is one of the most widely used methods for measuring the surface or interfacial tension of a liquid. A droplet is suspended from the tip of a needle, and its shape — a balance between surface tension (which pulls the drop toward a sphere) and gravity (which elongates it) — is captured with a camera. The droplet's outline is then fit to the Young-Laplace equation to extract the surface tension.

This method has several advantages over alternatives like the Du Noüy ring or Wilhelmy plate:

Requires only a small liquid volume (microliters)
Non-invasive; no contact between a probe and the bulk liquid surface
Can measure interfacial tension between two immiscible liquids, not just liquid-air
Works across a wide range of temperatures and pressures with appropriate enclosures
Existing Commercial Instruments

Commercial pendant drop tensiometers, such as the [Biolin Scientific Attension Theta / KRÜSS DSA / TECLIS Tracker] families and the instrument this project is benchmarked against, the [FTÅ200], provide high-precision automated measurement. These systems typically include:

A precision syringe pump for controlled droplet dispensing
A high-resolution monochrome camera with telecentric or long-working-distance optics
Diffuse LED backlighting
Proprietary software for edge detection and Young-Laplace curve fitting
Cost Barriers

Commercial pendant drop tensiometers commonly range from [$15,000–$50,000+], placing them out of reach for many teaching labs, small research groups, and makerspaces. Service contracts and proprietary consumables add ongoing cost. This price point limits access primarily to well-funded industrial and academic laboratories.

Educational Accessibility

Surface tension measurement is a standard topic in physical chemistry and fluid mechanics curricula, but hands-on access to a working pendant drop tensiometer is rare outside of graduate-level research labs. An affordable, open-hardware instrument allows:

Undergraduate labs to run authentic surface tension experiments
Students to see the full measurement pipeline (optics, mechanics, image processing, curve fitting) rather than a black box
Low-resource institutions to build local capability instead of relying on shared or unavailable equipment
Motivation for an Open-Source Alternative

This project was developed to provide a low-cost, open-source pendant drop tensiometer that reproduces the core measurement capability of commercial systems such as the FTÅ200, while remaining fully documented, modular, and buildable from off-the-shelf and 3D-printed components. All mechanical designs, electronics, firmware, and analysis workflows are released openly so that the instrument can be reproduced, modified, and improved by others.

Project Objectives
Achieve surface tension measurement accuracy within [X]% of literature reference values for common calibration liquids (e.g., water, ethanol)
Keep total build cost under [$X]
Use commonly available components (3D-printed frame, off-the-shelf camera, stepper motor, microcontroller)
Provide complete, reproducible build and calibration documentation
Build on and interoperate with existing open-source analysis software (OpenDrop) rather than duplicating that work

# Working Principle

This page explains the physics behind pendant drop tensiometry before describing the hardware that implements it. For the mechanical and electrical design, see [Design Process](design.md). For step-by-step operation, see [Usage Guide](usage.md).

## 1. Surface Tension Fundamentals

Surface tension, γ, is defined as the force per unit length acting along the surface of a liquid, or equivalently the energy required to increase the surface area by one unit:

```
γ = F / L   (force per unit length, N/m)
γ = dE / dA (energy per unit area, J/m²)
```

These two definitions are dimensionally equivalent (N/m = J/m²). For most liquids of interest, γ falls in the range of roughly 15–75 mN/m at room temperature.

## 2. Pendant Drop Formation

When a liquid is dispensed slowly from a needle tip, gravity pulls the growing droplet downward while surface tension resists deformation, holding the drop together and maintaining a curved profile. The resulting droplet shape is an equilibrium between:

- **Gravitational/hydrostatic pressure**, which increases with depth in the drop and depends on liquid density
- **Laplace pressure**, the pressure difference across the curved interface, which depends on surface tension and local curvature

Because the drop is in mechanical equilibrium, its entire outline can be predicted, for a known surface tension and density, by a single differential equation — the Young-Laplace equation.

## 3. Young-Laplace Equation

The Young-Laplace equation relates the pressure difference across a curved liquid interface to its curvature and the surface tension:

```
ΔP = γ (1/R₁ + 1/R₂)
```

where R₁ and R₂ are the two principal radii of curvature at a given point on the surface. For an axisymmetric pendant drop, this reduces to a first-order ODE system describing the drop profile as a function of arc length, parameterized by:

- Surface tension, γ
- Density difference between the liquid and surrounding fluid, Δρ
- Gravitational acceleration, g
- A shape factor, β (the dimensionless "Bond number"), which captures the relative importance of gravity versus surface tension

The full derivation and numerical integration scheme (typically solved via Runge-Kutta methods) are covered in the references listed in [References](references.md). In practice, this project relies on the Young-Laplace fitting routine implemented in OpenDrop rather than re-deriving the solver, so this section focuses on how the physical setup produces the image data that solver requires.

## 4. Image Acquisition

A digital image of the pendant drop is captured with the drop backlit by a diffuse light source, producing a dark droplet silhouette against a bright, uniform background. Key requirements for a usable image:

- The needle must be vertical (perpendicular to the ground) so the drop is axisymmetric about a known axis
- The full droplet, including the point where it meets the needle tip, must be in frame
- Illumination must be uniform to avoid false edges from lighting gradients
- Sufficient contrast must exist between the drop silhouette and background (see [Image Processing](image-processing.md) for how this project addresses low-contrast conditions caused by close droplet-to-backlight spacing)

## 5. Edge Detection

The droplet boundary is extracted from the image using an edge-detection algorithm (e.g., Canny edge detection or a gradient-threshold method) that locates the transition between the dark droplet and light background at sub-pixel resolution along the length of the drop.

## 6. Curve Fitting

The extracted edge coordinates are fit to the theoretical Young-Laplace profile using a nonlinear least-squares optimization. The fitting routine adjusts γ (and other free parameters, such as the apex curvature and drop orientation) to minimize the deviation between the theoretical curve and the measured edge points.

## 7. Surface Tension Calculation

Once the best-fit Young-Laplace profile is found, the surface tension is recovered from the fitted shape factor and the known physical constants:

```
γ = Δρ · g · R₀² / β
```

where R₀ is the radius of curvature at the drop apex and β is the fitted shape factor. The pixel-to-physical-length conversion (from the [Calibration](calibration.md) procedure) and the known needle outer diameter are used to convert pixel measurements into real-world units.

## System Overview

```
Needle
  │
  ▼
Droplet ──► Image ──► Edge Detection ──► Profile Extraction ──► Young-Laplace Fit ──► Surface Tension
```

The physical components that generate each stage of this pipeline (syringe/needle, droplet, camera, backlight) are described in [Design Process](design.md).

