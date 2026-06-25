# What is photogrammetry?

Photogrammetry is a technique that builds a 3D model of an object from a set of ordinary 2D photographs taken from many different viewpoints. Where microCT uses X-rays to recover the *internal* structure of a specimen, photogrammetry uses visible light and recovers only the *outer surface* — but it does so in full color, capturing the real texture, pigmentation, and surface detail of the specimen. Like microCT, it is non-destructive: the specimen is only photographed, never altered.

The modern form of photogrammetry used for biological specimens is called **Structure-from-Motion (SfM)**. The name captures the core idea: by observing how features on the specimen shift between overlapping photographs, the software can work backwards to figure out both the 3D shape of the object *and* where each camera was when the photo was taken — all from a single camera moved around the object (or, equivalently, the object rotated in front of a fixed camera). No specialized scanner is required; a consumer camera, controlled lighting, and processing software are enough.

This document is background on photogrammetry as an imaging modality — what it is, how it works, what it is good at, and what it struggles with. It is *not* a step-by-step guide to a particular software. For the hands-on SlicerMorph workflow, see the [SlicerPhotogrammetry extension user guide](https://github.com/SlicerMorph/SlicerPhotogrammetry?tab=readme-ov-file#user-guide) and the related entries in the [Tutorials index](https://github.com/SlicerMorph/Tutorials#slicermorph-photogrammetry).

The capture-and-scaling recommendations and the accuracy findings below draw on the SlicerMorph photogrammetry study:

> Zhang C, Maga AM (2023). An Open-Source Photogrammetry Workflow for Reconstructing 3D Models. *Integrative Organismal Biology* 5(1): obad024. https://doi.org/10.1093/iob/obad024

That study reconstructed 3D models of 15 adult mountain beaver (*Aplodontia rufa*) skulls from photographs and compared them against microCT-derived models of the same specimens. The descriptions of the reconstruction pipeline and of the available software additionally draw on the documentation of the major open-source photogrammetry projects — OpenSfM, OpenDroneMap, COLMAP, OpenMVG/OpenMVS, and AliceVision/Meshroom.

## How photogrammetry compares to other 3D imaging

Photogrammetry sits alongside microCT and surface (structured-light/laser) scanning as one of the common ways to get a 3D digital specimen. The key practical differences:

- **Surface only.** Photogrammetry recovers the visible outer surface — it cannot see inside a specimen the way [microCT](../microCT/README.md) does. There are no cross-sectional slices and no internal anatomy.
- **Real color and texture.** Unlike microCT (which records X-ray attenuation as grayscale) and most surface scanners, photogrammetry captures the specimen's true color and surface pattern and maps it onto the model.
- **Low cost and portable.** The hardware is a camera, some lights, and a turntable — far cheaper and more transportable than a microCT system or a commercial 3D scanner. This makes it attractive for museum collections work, fieldwork, and teaching.
- **A caution about mixing modalities.** Because photogrammetry and microCT recover surfaces in slightly different ways, models from the two methods carry small but systematic differences. The Zhang & Maga study found photogrammetric measurements were accurate to within ~1.8% of microCT on average, which is acceptable for many purposes — but the authors advise *against* combining photogrammetry- and microCT-derived models in the *same* geometric morphometric analysis, especially when the shape differences you are studying are subtle. Pick one modality per dataset, and run a small pilot comparison before trusting photogrammetry for a new kind of measurement.

## How Structure-from-Motion works

SfM turns a pile of overlapping photographs into a textured 3D model through a sequence of steps. Conceptually:

1. **Feature detection and matching.** The software scans each photo for distinctive points (corners, speckles, texture detail), describes each one in a way that survives changes in scale, rotation, and lighting — the long-standing workhorse here is the **SIFT** (Scale-Invariant Feature Transform) detector — and then finds the same physical point appearing across multiple photos. This is why surface texture matters so much: a smooth, featureless, or shiny surface gives the algorithm nothing to latch onto.
2. **Camera pose estimation (sparse cloud).** From those matched features, the software simultaneously solves for where every camera was positioned and oriented, and for the 3D location of the matched points. This joint optimization — refining all camera poses and 3D points together to minimize the overall reprojection error — is called **bundle adjustment**, and it is the mathematical heart of SfM. The result is a *sparse point cloud* plus a recovered position for every camera.
3. **Dense reconstruction (multi-view stereo).** Using the now-known camera positions, the software revisits the photos and computes a much denser set of 3D points, filling in the surface between the sparse feature points.
4. **Meshing and texturing.** The dense cloud is converted into a continuous surface (commonly via **Poisson surface reconstruction**), and the original photographs are projected back onto that surface to produce the color texture.
5. **Scaling.** SfM by itself produces a model in arbitrary units (see *Scale and calibration* below). A final step assigns real-world size, using a scale reference in the photos or a manual measurement.

Two pieces of terminology recur across the different software packages. First, tools differ in *how* they grow the reconstruction in step 2: **incremental** SfM adds images one at a time and re-optimizes as it goes (robust, and the most common approach), while **global** SfM solves for all cameras at once (faster on large, well-connected image sets, but more sensitive to bad matches). Second, the overall architecture is consistent across essentially every package: a **sparse SfM** stage (steps 1–2) that recovers the cameras and a sparse cloud, followed by a separate **dense multi-view-stereo (MVS)** stage (step 3) that fills in the surface. Knowing this split helps when reading any photogrammetry tool's documentation, because most of them name their steps this way.

<figure>
  <!-- TODO: add figure. Suggested filename: sfm_overview.png -->
  <!-- <img src="./sfm_overview.png" alt="Diagram showing many camera positions arranged around a specimen and the resulting point cloud"> -->
  <figcaption><b>Fig.1 Structure-from-Motion overview.</b> A specimen photographed from many overlapping viewpoints (left). By matching shared surface features across photos, the software recovers both the 3D positions of those features and the camera location for every photo (center), then densifies that into a full surface mesh and projects the original photos back onto it as a color texture (right).</figcaption>
</figure>

## Capturing good photographs

The quality of a photogrammetric model is decided almost entirely at the photography stage. The guidance below reflects the setup used in the Zhang & Maga workflow.

### Camera and focus
A high-end camera is not required — the study obtained good results with a low-end DSLR. What matters is that the specimen is **in sharp focus across its whole depth**. Because parts of the specimen nearer and farther from the lens must both be sharp, a small aperture (large f-number) is used to maximize depth of field: **f/13–f/16** worked well, and even f/20–f/22 produced sharp results. A small aperture lets in less light, which is compensated by stronger, controlled lighting and/or longer exposures with the camera held steady.

### Lighting
Use **diffuse, even illumination** — a lightbox or softboxes — so the specimen is well exposed without harsh hotspots. The goal is to avoid glare and specular highlights, which shift position as the specimen rotates and confuse the feature-matching. Soft shadows are tolerable as long as every part of the surface is clearly visible in *some* of the photos.

### Background and masking
A **uniform, plain background** reduces noise and helps the software concentrate on the specimen. Even better, masking out the background (removing everything that isn't the specimen) markedly improves reconstruction quality — the Zhang & Maga workflow uses a deep-learning background-removal step, which is especially helpful for **thin structures** (such as the edges of bone) that otherwise blur into the background. The SlicerPhotogrammetry extension provides tooling for this masking step.

### Number of photos and overlap
Adjacent photos must **overlap substantially** so the same features appear in several frames. As a rule of thumb, capture at least **32 photographs per full rotation** of the specimen. To cover the whole specimen, repeat the rotation at several camera heights/angles and in more than one specimen orientation: the study captured **six sets of photos (~320 images) per specimen**. After a little practice, photographing one specimen took roughly 20–30 minutes.

### Turntable vs. handheld
Two equivalent strategies: move the camera around a stationary specimen, or keep the camera fixed and rotate the specimen on a **turntable**. A remote-controlled turntable inside a lightbox gives the most consistent, repeatable spacing between shots and is the easier route for small museum specimens.

### Specimen positioning
No single orientation sees everything. Different placements expose different features — for a skull, for example, a vertical orientation captures the cranial vault and trunk well, while a horizontal orientation better captures lateral structures like the zygomatic arches. Plan two or more orientations so that, between them, every surface is well covered and well lit.

<figure>
  <!-- TODO: add figure. Suggested filename: capture_setup.png -->
  <!-- <img src="./capture_setup.png" alt="Photo of a specimen on a turntable inside a lightbox with a camera on a tripod"> -->
  <figcaption><b>Fig.2 A typical capture setup.</b> The specimen sits on a remote-controlled turntable inside a lightbox for even, glare-free lighting; the camera is fixed on a tripod. The turntable is advanced in equal steps (at least 32 per full rotation) and the process is repeated at several camera angles and specimen orientations so the entire surface is captured with generous overlap.</figcaption>
</figure>

## Scale and calibration

A photogrammetric reconstruction has **no inherent sense of size** — SfM recovers shape and relative proportions, but the model could be the size of a coin or a car. To make measurements meaningful, you must give the model a real-world scale. The common approaches:

- **Fiducial markers (Aruco markers).** Printed markers with known dimensions are placed around the specimen and photographed along with it. The software detects them automatically and uses their known size to scale the model. In the Zhang & Maga workflow, Aruco markers serve double duty: they provide automated scaling *and* their high-contrast pattern gives the algorithm reliable features that help the textured reconstruction succeed.
- **Scale bars / rulers.** A ruler or calibrated scale bar in the frame lets you set scale by identifying two points a known distance apart.
- **Manual correction.** If a known physical dimension of the specimen has been measured (e.g. with calipers), the model can be scaled to match after the fact.

Always verify the final scale against an independent measurement before using the model quantitatively.

<figure>
  <!-- TODO: add figure. Suggested filename: aruco_scaling.png -->
  <!-- <img src="./aruco_scaling.png" alt="Specimen surrounded by printed Aruco fiducial markers"> -->
  <figcaption><b>Fig.3 Scaling with Aruco markers.</b> Printed fiducial markers of known size are arranged around the specimen and appear in the photographs. The software detects them automatically, both to set the model's real-world scale and to provide stable, high-contrast features that aid reconstruction.</figcaption>
</figure>

## Which specimens work well — and which don't

Because SfM depends on matching surface features across photographs, the specimen's optical properties determine success more than its size or shape.

**Works well:**
- Opaque, **matte** surfaces with visible texture or pattern — dry bone, skulls, teeth, shells, dry plant material, weathered surfaces.
- Rigid specimens that do not move or deform between photos.

**Problematic:**
- **Reflective or glossy** surfaces (wet, varnished, or polished specimens) — specular highlights move with the camera and break feature matching.
- **Transparent or translucent** material (glass-like structures, some preserved tissue).
- **Featureless, uniform** surfaces — a smooth white object gives the algorithm nothing to match.
- **Fur, hair, and very fine filaments**, which are thin and move easily.
- **Deep concavities and occlusions** the camera cannot see into, and **thin sharp edges**, which tend to reconstruct poorly (the study noted small black-polygon artifacts accumulating along thin edges, requiring minor cleanup but not generally impairing landmark placement).
- Anything where you need **internal anatomy** — photogrammetry is surface-only.

## Processing software

The photographs are turned into a model by photogrammetry software. All of the major tools share the two-stage architecture above (sparse SfM, then dense MVS), and several are open-source and free — useful both for reproducibility and because no single package is the sole authority on how this is done. The open-source landscape is worth knowing even if you only ever use one tool:

- **OpenSfM** — a Structure-from-Motion library written in Python, originally from Mapillary (BSD-licensed). It performs the sparse SfM stage — feature detection/matching, incremental reconstruction, and bundle adjustment — can fold in GPS/IMU sensor data, and handles perspective, fisheye, and spherical cameras. Most relevant here: **OpenSfM is the SfM engine inside OpenDroneMap**, so it underlies the SlicerMorph workflow below even though you never invoke it directly.
- **OpenDroneMap (ODM) / WebODM** — an end-to-end open-source pipeline (sparse SfM via OpenSfM, then dense reconstruction, meshing, and texturing). This is the pipeline used in the Zhang & Maga workflow and the one wrapped for use inside Slicer by the **[SlicerPhotogrammetry extension](https://github.com/SlicerMorph/SlicerPhotogrammetry)**. Though built for drone mapping, it works well for turntable specimen capture.
- **COLMAP** — a widely used, general-purpose SfM + MVS pipeline with both GUI and command line (BSD-licensed), and a common reference implementation in the research literature. It uses SIFT features and incremental reconstruction for the sparse stage, and per-image depth maps that are fused and meshed (Poisson or Delaunay) for the dense stage; a global variant (GLOMAP) and Python bindings (PyCOLMAP) also exist. Its methods are documented in Schönberger & Frahm, *Structure-from-Motion Revisited* (CVPR 2016) and Schönberger et al., *Pixelwise View Selection for Unstructured Multi-View Stereo* (ECCV 2016).
- **OpenMVG + OpenMVS** — a classic pairing that makes the two-stage split explicit: **OpenMVG** ("open Multiple View Geometry," MPL-2.0) handles the sparse SfM (both incremental and global), then exports the scene to **OpenMVS** for the dense point cloud, meshing, and texturing.
- **AliceVision / Meshroom** — **AliceVision** is a computer-vision framework implementing the full pipeline; **Meshroom** (MPL-2.0) is its node-based graphical front end, in which you build and run the feature-extraction → matching → SfM → depth-map → meshing → texturing graph visually.

On the commercial side, **Agisoft Metashape** and **RealityCapture** bundle the whole pipeline behind a polished interface — fast and capable, but proprietary.

On a capable machine or cloud server, processing one specimen's image sets through the full open-source pipeline took on the order of **2–3 hours** in the Zhang & Maga study, with multiple specimens processable concurrently.

## Output data formats

Photogrammetry produces a **textured surface mesh**, not a volume. Typical outputs:

- **OBJ** — a mesh file (`.obj`) accompanied by a material file (`.mtl`) and one or more **texture image** files (e.g. `.png`/`.jpg`). The color lives in the separate texture image, so all of these files must be kept together.
- **PLY** — a mesh that can store color either as a texture or as per-vertex colors in a single file.
- **Point clouds** — the dense cloud itself can also be exported.

The reconstructed skull models in the study contained roughly **400,000–700,000 vertices** and were essentially watertight (holes and foramina closed over).

When bringing a model into 3D Slicer, load it through the **Models** module (see the [Models tutorial](../Slicer_Modules/Models/README.md)). A few things to keep in mind:

- Make sure the **texture files travel with the mesh** if you want to see the specimen's color, and that the model has been **scaled to real-world units** before you measure or landmark it.
- A textured mesh is *not* a volume: there are no slices to scroll through and no internal structures to segment. Workflows that expect a volume (volume rendering, segmentation of internal anatomy) do not apply — surface-based tools (landmarking, surface registration, geometric morphometrics on the surface) do.
