_MorphoDepot Tutorial · Part 3 of 10 — Preparing Data: 3D Volume_

[⬅ Overview](./README.md)  ·  [⬅ Prev: Slicer Installation & Setup](./2-slicer-setup.md)  ·  [Next: Preparing Data: Color Table ➡](./4-color-table.md)

---

## **3. Preparing Data for MorphoDepot Repository: 3D Volume**

> [!CAUTION]
> **CRITICAL STEP: get the volume right before you stage the repository.**
> Creating a repository *stages* it: MorphoDepot uploads your scan and creates a **private**
> repository from it. While it is staged you can still correct almost everything — metadata,
> license, color table, baseline segmentation, screenshots — but the **scan itself is frozen**,
> and so are the **voxel dimensions and spacing** read from it. The only way to change the scan
> is to **discard the staged repository and start over**, which means uploading everything again.

### **3.1 Volume Quality Control**

* **Cleaning:** Ensure the scan has minimal noise and no background artifacts (holders, markers, packing peanuts).
* **Orientation:** We suggest aligning the specimen with the standard world axes (Anterior-Posterior, Dorsal-Ventral; see below).
* **Type:** MorphoDepot takes a **scalar volume** (a normal grayscale image). Label maps, vector volumes, and sequences are not offered in the source-volume selector.

### **3.2 Cropping and Posing your Data**

Use the **Crop Volume** module in Slicer to prepare the specific Region of Interest (ROI).

1. Load your volume into Slicer.
2. Open **Crop Volume**.
3. **Reorient:** Use the "Reorient Volume" dropdown to align the specimen using the rotation handles in the 3D viewer. Hit **Apply** when done.
4. **Input ROI:** Create a new ROI.
5. **Crucial:** Set "Fit to Volume" mode to **Align to world axes + Resize**. Then, hit **Fit to Volume** button to expand the ROI to span the new alignment.
6. Drag the ROI box to encase the specimen with a padding of approximately **5%** of image on all sides.
7. **ROI Settings:**
   * Set **Interpolated cropping** to "Enabled".
   * Set **Fill value** to the background intensity (usually 0 for microCT, and -1000 for medical CT. If unsure, check specifics for your dataset using the Volumes module).
   * Click **Apply**. This creates a new resampled volume in the user specified orientation.
8. **Image Metadata:** Check the volume has the correct image spacing (resolution) entered, **in millimeters** — convert microns first (10 µm = 0.010 mm). MorphoDepot reads the spacing and the voxel dimensions off the volume at staging, shows them in the confirmation dialog, and stores them in the repository's metadata. They are **not** re-read afterwards, so a wrong spacing cannot be corrected by editing the staged repository — check it now.
9. **Size:** MorphoDepot saves the volume as a **compressed .nrrd** and checks its size when you stage:

   | Repository type | Where the scan is stored | Size limit |
   | --- | --- | --- |
   | **Personal** (your own account) | GitHub release attachment | **2 GB** |
   | **Archival** (MorphoDepot organization) | Jetstream2 cloud storage | **10 GB** |

   If the volume is over the limit, staging stops with a message telling you which limit you hit. Crop or resample and try again. (See [Part 5](./5-create-repo.md#repository-type-personal-vs-archival) for the difference between the two repository types.)

### **3.3 File Naming**

* Rename the resulting cropped volume node in the **Data** module.
* **Rules:** No spaces. Use letters, digits, periods, underscores, or dashes, and start and end with a letter or a digit (e.g., `Sebastes_caurinus_Skull_01`). MorphoDepot refuses to stage a volume whose node name breaks this rule, because the name becomes a file name in the repository.
* In general, keep the name simple. Repository metadata provides far more information about the specimen and the scan than you can possibly fit into a filename.

> [!TIP]
> The same naming rule applies to the **color table** node ([Part 4](./4-color-table.md)). MorphoDepot checks both names before it uploads anything, so a bad name costs you nothing but a rename — but it is one less thing to fix at the Create tab if you do it now.

### **3.4 What staging freezes, and what stays editable**

You will meet this in detail in [Part 5](./5-create-repo.md#52-what-staging-freezes-and-what-you-can-still-change); it is summarized here because it is what makes this preparation step irreversible.

| Frozen at staging | Editable for as long as the repository is staged |
| --- | --- |
| The **source volume** (the scan itself) | The whole accession form (species, sex, stage, modality, contrast, contents, anatomical areas, specimen record URL) |
| The **voxel dimensions and spacing** read from it | The **license** |
| The **repository name** | The **color table** (replace it with a different one) |
| The **repository type** (Personal or Archival) | The **baseline segmentation** (replace it with a different one) |
| The **owner** — your account or the organization | **Screenshots** (add, delete, re-caption) |

To change anything in the left column you have to discard the staged repository and stage a new one.

---

[⬅ Overview](./README.md)  ·  [⬅ Prev: Slicer Installation & Setup](./2-slicer-setup.md)  ·  [Next: Preparing Data: Color Table ➡](./4-color-table.md)
