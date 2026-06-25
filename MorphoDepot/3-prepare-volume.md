_MorphoDepot Tutorial · Part 3 of 9 — Preparing Data: 3D Volume_

[⬅ Overview](./README.md)  ·  [⬅ Prev: Slicer Installation & Setup](./2-slicer-setup.md)  ·  [Next: Preparing Data: Color Table ➡](./4-color-table.md)

---

## **3. Preparing Data for MorphoDepot Repository: 3D Volume**

> [!CAUTION]
> **CRITICAL STEP:** Data must be cleaned, posed, and padded *before* uploading. Once a repository is created, the source volume cannot be changed without restarting the project.

### **3.1 Volume Quality Control**

* **Cleaning:** Ensure the scan has minimal noise and no background artifacts (holders, markers, packing peanuts).  
* **Orientation:** We suggest aligning the specimen with the standard world axes (Anterior-Posterior, Dorsal-Ventral; see below).

### **3.2 Cropping and Posing your Data**

Use the **Crop Volume** module in Slicer to prepare the specific Region of Interest (ROI).

1. Load your volume into Slicer.  
2. Open **Crop Volume**.  
3. **Reorient:** Use the "Reorient Volume" dropdown to align the specimen using the rotation handles in the 3D viewer. Hit **Apply** when done.   
4. **Input ROI:** Create a new ROI.
5. **Crucial:** Set "Fit to Volume" mode to **Align to world axes \+ Resize**. Then, hit **Fit to Volume** button to expand the ROI to span the new alignment.   
6. Drag the ROI box to encase the specimen with a padding of approximately **5%** of image on all sides.  
7. **ROI Settings:**  
   * Set **Interpolated cropping** to "Enabled".  
   * Set **Fill value** to the background intensity (usually 0 for microCT, and \-1000 for medical CT. If unsure, check specifics for your dataset using the Volumes module).  
   * Click **Apply**. This creates a new resampled volume in the user specified orientation.  
8. **Image Metadata:** Check the volume has correct image spacing (resolution) entered.   
9. **Size:** The final .nrrd file **must be under 2 GB** when saved on disk (with compression enabled). 

### **3.3 File Naming**

* Rename the resulting cropped volume node in the **Data** module.  
* **Rules:** No spaces. Use alphanumerics, underscores, or dashes only (e.g., Sebastes\_caurinus\_Skull\_01). In general, keep the filename simple. Repository metadata provides far more information about the specimen and the scan that you can possibly include in the filename.

---

[⬅ Overview](./README.md)  ·  [⬅ Prev: Slicer Installation & Setup](./2-slicer-setup.md)  ·  [Next: Preparing Data: Color Table ➡](./4-color-table.md)
