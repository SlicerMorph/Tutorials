# ANTsPy-I: Data and reference specimen

This part introduces the data and prepares the reference specimen used in the following parts. See the [ANTsPy tutorial overview](README.md) for all parts.

---

## Prerequisites

### Software Requirements

#### 3D Slicer
- Download and install the **latest stable** version of 3D Slicer from [slicer.org](https://download.slicer.org/)

#### SlicerANTsPy Extension 
Install the ANTsPy Extension from the Extension Catalogue

### Hardware Recommendations
We advise to run this tutorial using [MorphoCloudInstances](https://morphocloud.org) as registration operations are memory and compute intensive. Standard MorphoCloud instances (g3.l) provide 60GB of RAM and 16 cores. 
If you plan to use your own computer, we advise using a computer with at least **16G of RAM**. Similarly we advise using a **CPU** with many cores as the registration will benefit from increased parallelism. 
---

## Step 1: Obtaining the Data

### Clone the Repository

Open a terminal (Mac/Linux) or Git Bash (Windows) and run:

```bash
cd ~/Desktop
git clone https://github.com/muratmaga/ANTsamples.git
```

This will create a folder `ANTsamples` with the following structure:

```
ANTsamples/
├── LMs/           # 30 landmark files (.mrk.json)
└── volumes/       # 30 volume files (.nii.gz)
```


### Verify the Data

Check that you have 30 paired files:

```bash
cd ANTsamples
ls volumes/*.nii.gz | wc -l    # Should show: 30
ls LMs/*.mrk.json | wc -l      # Should show: 30
```

### Sample List

The dataset includes these mouse strains (we'll use all 30 for this tutorial):

- **C57BL6_J_** - C57BL/6J (most common laboratory strain)
- **BALB_CJ_** - BALB/cJ 
- **DBA_1J_** - DBA/1J
- **DBA_2J_** - DBA/2J
- **C3H_HEJ_** - C3H/HeJ
- **AKR_J_** - AKR/J
- **129S1_SVLMJ_** - 129S1/SvlmJ
- **FVB_NJ_** - FVB/NJ
- ... and 22 more strains

---

## Step 2: Loading Data into 3D Slicer

### Launch 3D Slicer and Load Sample Volumes

1. Open 3D Slicer
1. Go to `File → Add Data`
2. Click `Choose File(s) to Add`
3. Navigate to `ANTsamples/volumes/`
4. Select these files (hold Ctrl/Cmd to select multiple):
   - `C57BL6_J_.nii.gz`
   - `BALB_CJ_.nii.gz`
   - `DBA_2J_.nii.gz`
5. Click `Open`, then `OK`

### Load Corresponding Landmarks

1. Go to `File → Add Data`
2. Click `Choose File(s) to Add`
3. Navigate to `ANTsamples/LMs/`
4. Select the corresponding landmark files:
   - `C57BL6_J_.mrk.json`
   - `BALB_CJ_.mrk.json`
   - `DBA_2J_.mrk.json`
5. Click `Open`, then `OK`

### Visualize the Data

1. In the **Data** module, you should see:
   - 3 volumes (scalar volumes)
   - 3 markups (fiducial nodes with 45 points each)

2. To view a volume:
   - Click on `C57BL6_J_` in the Data module
   - It will appear in the slice viewers (Red, Yellow, Green windows)
   - Use the mouse wheel to scroll through slices
   - Use the **3D** view to see the full volume

3. To view landmarks:
   - In the **Markups** module, select `C57BL6_J__1` from the dropdown (a landmark file loaded after a volume with the same name gets the suffix `_1`)
   - Landmarks will appear as small spheres on the skull
   - Adjust visibility/size in the Display section if needed


<img src="images/01_loaded_data.png" width="900">


### Understanding the data better

- None of the samples are oriented canonically; slice planes to not correspond to anatomical planes.
- Landmarks are in millimeters (mm)
- Each specimen has 45 anatomical landmarks on the skull

---

## Step 3: Prepare a Reference Specimen (Crop Volume)

Before building the template, we need to prepare a reference specimen that will serve as the initial template. This involves reorienting the specimen to align with anatomical planes and creating an average from rigidly aligned samples.

### Why Use a Reference Specimen?

Using a reference specimen (rather than starting from scratch) has advantages and disadvantages:

**Advantages:**
- Faster convergence during template building
- More anatomically meaningful starting point
- Easier to interpret results in consistent anatomical orientations

**Disadvantages:**
- **Shape bias:** The final template may retain some shape characteristics of the reference specimen
- **Size bias:** Initial size of the reference influences the template
- **Asymmetry bias:** If the reference has asymmetries, these may persist
- **Selection bias:** Choosing one strain over others introduces systematic bias

**Best Practice:** To minimize bias, we'll create an average of rigidly aligned specimens rather than using an actual specimen from our dataset as references. This provides a reasonable starting point while reducing individual specimen bias.

### Reorient a Reference Specimen

We'll use the **NZBWF1_J_** specimen as our initial reference and reorient it so the major axes align with anatomical planes.

#### Load the Reference Specimen

0. Reset the scene to remove other data.
1. `File → Add Data`
2. Select `ANTsamples/volumes/NZBWF1_J_.nii.gz` and `ANTsamples/LMs/NZBWF1_J_.mrk.json`
3. Click `Open`, then `OK`. The landmarks are loaded as `NZBWF1_J__1`, because the volume already has the name `NZBWF1_J_`.

#### Open the Crop Volume Module

1. In the module search bar, type "Crop"
2. Select **Crop Volume**
3. Set **Input volume** to `NZBWF1_J_`, expand **Reorient volume** and click **Initialize**. This creates a transform, `Reorient_NZBWF1_J_`, and rotation handles in the slice and 3D views.
4. Rotate the volume with the handles until the anatomical planes are aligned with the slice views.

For detailed instructions, see the [CropVolume tutorial](https://github.com/SlicerMorph/Tutorials/blob/main/Slicer_Modules/Crop_Volume/Readme.MD#using-cropvolume-to-simulatenously-reorient-and-resample-your-data).

<img src="images/02_reorient_widget.png" width="900">

#### Reorient the Landmarks

**Important:** Do this before you apply the reorientation. Applying it deletes the `Reorient_NZBWF1_J_` transform, and the landmarks must be moved with the same transform.

1. Open the **Transforms** module and select `Reorient_NZBWF1_J_` as the **Active Transform**
2. Under **Apply transform**, move the landmarks (`NZBWF1_J__1`) to the **Transformed** list
3. Click **Harden transform** to make it permanent

#### Apply Reorientation

1. In **Crop Volume**, click **Apply** under **Reorient volume**. The rotation is written into the volume, and the `Reorient_NZBWF1_J_` transform is removed.
2. Resample the volume onto the anatomical axes: set **Input ROI** to **Create new ROI**, open the menu next to **Fit to Volume** and choose **Align to world axes + Resize**, click **Fit to Volume**, then click the main **Apply**.
3. The reoriented volume appears in the scene with the **cropped** suffix (`NZBWF1_J_ cropped`)
4. Verify in slice viewers that anatomical planes are aligned with slice views.
5. Save the result: Right-click `NZBWF1_J_ cropped` → Export to file
   - Save as `ANTsamples/NZBWF1_J_reoriented.nii.gz`
6. Save the landmarks (`NZBWF1_J__1`) as `NZBWF1_J_reoriented.mrk.json` in the same folder.

<img src="images/03_reoriented.png" width="900">

---

Next: [ANTsPy-II: Group-wise tab, rigid alignment to the reference](Groupwise_rigid.md)
