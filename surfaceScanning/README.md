# What is surface (structured-light & laser) 3D scanning?

Surface 3D scanning is a family of techniques that measure the outer shape of an object using **active light** — the scanner projects its own light (a pattern, a laser line, or a laser dot) onto the specimen and a camera watches how that light lands on the surface. From the geometry of "where the light was sent" versus "where the camera saw it," the scanner computes the 3D position of each point on the surface. The result is a dense surface mesh, and on many systems a color texture as well.

Like [photogrammetry](../photogrammetry/README.md), surface scanning recovers only the *outer surface* of a specimen — it cannot see inside the way [microCT](../microCT/README.md) does. The difference between the two surface methods is the light: photogrammetry is **passive** (it relies on whatever texture the specimen already has, photographed under ordinary light), while structured-light and laser scanners are **active** (they project their own, precisely controlled light and so do not depend on the specimen having visible texture).

This document is background on surface scanning as an imaging modality — what it is, the main technologies, what they are good at, and where they struggle. It is not a guide to operating a specific scanner; every scanner ships with its own capture software, and that software, not Slicer, is used to acquire and align the scans. 3D Slicer and SlicerMorph come in afterward, once you have a finished surface model to import and analyze.

## How surface scanning compares to other 3D imaging

- **Surface only.** Like photogrammetry, surface scanning captures the visible exterior — no cross-sections, no internal anatomy. For internal structure you need [microCT](../microCT/README.md).
- **Active vs. passive light.** Because the scanner supplies its own light pattern, it can digitize smooth, featureless surfaces that would defeat photogrammetry's feature-matching — a plain white bone, for example. The trade-off is dedicated hardware rather than just a camera.
- **Usually metric out of the box.** Structured-light and laser systems are factory-calibrated and report results directly in real-world units (millimeters), so unlike photogrammetry they generally do not need a separate scaling step. (You should still check against a known standard.)
- **Color varies by system.** Some scanners capture color/texture along with shape; many capture geometry only. This is a per-device feature, not a given.
- **Cost and portability.** Sits between photogrammetry (cheapest) and microCT (most expensive, least portable). Handheld units are very portable; high-accuracy desktop and articulated-arm systems are pricier and more stationary. As with the other modalities, avoid mixing models from different methods (or even different scanners) in a single geometric morphometric analysis without first checking that the systematic differences are negligible for your question.

## How structured-light scanning works

A structured-light scanner has a **projector** and one or more **cameras** mounted a fixed, known distance and angle apart. The projector casts a known pattern — typically a series of parallel **stripes or fringes** — onto the specimen. From the camera's viewpoint, those stripes appear bent and displaced wherever the surface rises, falls, or tilts. Because the scanner knows exactly what pattern it sent and exactly where the camera is relative to the projector, it can **triangulate** the 3D position of every point on the surface from how the pattern is deformed.

To resolve the surface precisely, the scanner usually projects a *sequence* of patterns in rapid succession (for example, binary/Gray-code patterns to coarsely locate each stripe, then phase-shifted fringes to refine it). Each camera frame captures a snapshot of one pattern; combined, they yield a dense, accurate "patch" of surface in a fraction of a second.

<figure>
  <!-- TODO: add figure. Suggested filename: structured_light_triangulation.png -->
  <!-- <img src="./structured_light_triangulation.png" alt="Diagram of a projector casting striped patterns onto a specimen and a camera viewing the deformed stripes, with triangulation geometry"> -->
  <figcaption><b>Fig.1 Structured-light triangulation.</b> A projector casts a known striped/fringe pattern onto the specimen. A camera, offset by a fixed and calibrated angle, sees the stripes bend and shift according to surface shape. Knowing the projected pattern and the projector–camera geometry, the scanner triangulates the 3D coordinate of every surface point captured in that view.</figcaption>
</figure>

## How laser scanning works

Laser scanners use the same triangulation idea but with a laser instead of a projected pattern. A **laser-line (or laser-stripe)** scanner sweeps a single bright line of laser light across the specimen while a camera, offset at a known angle, records how the line bends over the surface; each frame yields a thin profile of 3D points, and sweeping the line builds up the full surface. **Laser-point** scanners do the same with a single spot. These emitters are often mounted on a **handheld wand** or an **articulated measuring arm** whose joints report the scanner's exact position, so successive sweeps land in a common coordinate frame.

A related but distinct technology is **time-of-flight** (including LiDAR), which measures how long a light pulse takes to return rather than triangulating a deformed line. Time-of-flight excels at large scenes (rooms, landscapes, large skeletons) but is generally too coarse for the small specimens typical of specimen-level morphometrics, where triangulation-based structured-light and laser-line scanners dominate.

<figure>
  <!-- TODO: add figure. Suggested filename: laser_line_triangulation.png -->
  <!-- <img src="./laser_line_triangulation.png" alt="Diagram of a laser line swept across a specimen with a camera observing the deformed line"> -->
  <figcaption><b>Fig.2 Laser-line triangulation.</b> A laser projects a single line onto the specimen; an offset camera records how that line deforms over the surface, yielding one profile of 3D points per frame. Sweeping the line across the specimen — by moving the specimen, the scanner head, or an articulated arm — accumulates the profiles into a full surface.</figcaption>
</figure>

## Types of surface scanners

The same underlying physics appears in several hardware form factors:

- **Desktop / turntable scanners.** A structured-light head views a specimen on a motorized turntable. Well suited to small, portable specimens (skulls, teeth, shells); often the highest accuracy for their size class and frequently capture color.
- **Handheld scanners.** Structured-light or laser units the operator moves around the specimen; the software stitches the stream of views together live. Very flexible and portable, good for larger or awkwardly shaped specimens.
- **Articulated-arm scanners.** A laser-line head on a precision measuring arm; the arm's encoders give exact head position, prizing accuracy and repeatability for industrial and large-specimen work.

## Capturing good scans

Because the method depends on a camera cleanly seeing projected light on the surface, the **optical properties of the specimen matter just as much as for photogrammetry** — sometimes more.

### Surface properties
Matte, opaque, light-to-medium-toned surfaces scan best. Problem surfaces include:

- **Shiny / specular** surfaces (wet, polished, varnished), which throw the projected light back as glare instead of scattering it for the camera to see.
- **Transparent or translucent** material, where light enters the surface and scatters internally rather than reflecting cleanly.
- **Very dark** surfaces, which absorb the projected light so little returns to the camera.

A traditional workshop fix is to dust the object with a fine **matte scanning powder/spray** to make it temporarily opaque and diffuse. For biological and museum specimens this is often **undesirable or prohibited**, because even removable coatings risk contaminating or altering irreplaceable material — confirm what is acceptable for your collection before considering it, and prefer scanners and settings tuned for difficult surfaces instead.

### Coverage, occlusion, and alignment
A single view only captures the surface the scanner can see; deep cavities, undercuts, and the underside of a specimen are occluded. Complete coverage therefore requires **multiple scans from different angles**, which the software then **registers (aligns) and merges** into one model — typically by matching overlapping geometry between scans, sometimes aided by stick-on registration targets or a calibrated turntable. As with photogrammetry, plan several specimen orientations and ensure the scans overlap enough to align reliably. Thin sharp edges and fine filaments (hair, bristles, delicate processes) remain difficult and may reconstruct incompletely.

### Resolution and fixturing
Scanners specify a **point spacing / resolution** (how finely the surface is sampled); choose one appropriate to the smallest feature you need to measure. The specimen must stay **rigid and still** relative to the scanner during each capture, so stable fixturing (a cradle, clay, a turntable mount) is important.

## Scale and calibration

A major practical advantage over photogrammetry: structured-light and laser systems are **factory-calibrated and inherently metric**, so the finished model is already in real-world units (mm) without a separate scaling step. Calibration can drift over time or with temperature, so periodic recalibration with the manufacturer's calibration plate, and a sanity check against an object of known size, are good practice before quantitative work.

## Output data formats

Surface scanners export the same kinds of **surface mesh** files as photogrammetry (the scanner's own software handles capture, alignment, and merging first):

- **PLY** and **OBJ** — meshes that can carry color, either as per-vertex colors or as a separate texture image (with OBJ, keep the `.mtl` and texture image files together with the `.obj`).
- **STL** — geometry only, no color; common for 3D-printing and basic shape analysis.
- **WRL/VRML** and vendor-specific formats also appear; most can be converted to PLY/OBJ.
- **Point clouds** — the raw measured points, before meshing, can usually also be exported.

When bringing a scan into 3D Slicer, import it through the **Models** module (see the [Models tutorial](../Slicer_Modules/Models/README.md)). Practical notes:

- If the scan carries color and you want to keep it, make sure the **texture/color travels with the mesh**.
- Scans frequently need light **cleanup** — removing stray points and turntable/background fragments, filling small holes, and sometimes **decimating** a very dense mesh to a manageable size. Slicer's [Surface Toolbox and Dynamic Modeler](../Slicer_Modules/Surface_Toolbox/README.md) cover these operations.
- A scan is a **surface model, not a volume**: there are no slices to scroll through and no internal anatomy to segment. Surface-based workflows — landmarking, surface registration, and geometric morphometrics — apply directly; volume-based ones (volume rendering, segmenting internal structures) do not.
