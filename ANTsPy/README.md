# ANTsRegistration Tutorial: Mouse Craniometric Analysis

A comprehensive guide to template building, registration, and Jacobian analysis using the SlicerANTsPy extension.

## Table of Contents
1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Step 1: Obtaining the Data](#step-1-obtaining-the-data)
4. [Step 2: Loading Data into 3D Slicer](#step-2-loading-data-into-3d-slicer)
5. [Step 3: Building a Population Template](#step-3-building-a-population-template)
6. [Step 4: Group-wise Registration to Template](#step-4-group-wise-registration-to-template)
7. [Step 5: Jacobian Analysis](#step-5-jacobian-analysis)
8. [Troubleshooting](#troubleshooting)
9. [Advanced Topics](#advanced-topics)

---

## Introduction

This tutorial demonstrates a complete morphometric analysis workflow using the **ANTsRegistration** module in 3D Slicer. You will:
- Create one canonically oriented sample as a reference
- Build a population-averaged template from mouse microCT scans
- Register individual specimens to the template
- Perform statistical shape analysis using Jacobian determinants
- Compare morphological variation between groups

**Dataset:** Low-resolution mouse head microCT scans from 29 different inbred and hybrid mouse strains, with 45 craniometric landmarks per specimen. Landmarks are necessary to provide an initial alignment, as most automated registration methods fail if the positional difference between two volumes are too large. Most cases you do not need as many landmark to do an approximate alignment, 4-8 are usually enough.

**Biological Context:** Mouse strains exhibit cranial shape variation due to genetic differences. This tutorial shows how to quantify and analyze these differences.

---

## Prerequisites

### Software Requirements

#### 3D Slicer
- Download and install the **latest preview** version of 3D Slicer from [slicer.org](https://download.slicer.org/)

#### SlicerANTsPy Extension 
This tutorial covers the features available in the development version of the SlicerANTsPy extension, which is available for the preview builds of Slicer (r34314 or later). If you are using the current stable (v5.10), and installed ANTsPy, some of the functionality will be slightly different.  To obtain the development branch of the SlicerANTsPy, you will need to manually install it following the instructions below.

If you are using the preview version, just install SlicerANTsPy from the extension catalogue, and skip these steps. 

**Installation Steps:**

1. **Clone the SlicerANTsPy repository:**
   
   Open a terminal (Mac/Linux) or Git Bash (Windows) and run:
   
   ```bash
   cd ~/Desktop  # or your preferred location
   git clone https://github.com/SlicerMorph/SlicerANTsPy.git
   cd SlicerANTsPy
   git checkout main
   ```

2. **Add the extension to Slicer:**
   
   - Launch 3D Slicer
   - Go to `Edit → Application Settings`
   - Click on **Modules** in the left panel
   - Under **Additional module paths**, click the `>>` button to add a new path
   - Click the folder icon and navigate to the `SlicerANTsPy` folder you just cloned
   - Select the folder and click **OK**
   - Click **OK** to close Application Settings
   - **Restart Slicer**

3. **Verify installation:**
   
   - After restarting, search for "ANTsRegistration" in the module search bar
   - You should see the **ANTsRegistration** module with tabs: Pairwise, Template, Group-wise, Average, and Analysis
   - If you don't see these tabs, double-check that you selected the correct folder path

**Note:** Once these features are merged into the main branch and released through the Extension Manager, you can switch to the standard installation method.

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
├── LMs/           # 29 landmark files (.mrk.json)
└── volumes/       # 29 volume files (.nii.gz)
```

### Verify the Data

Check that you have 29 paired files:

```bash
cd ANTsamples
ls volumes/*.nii.gz | wc -l    # Should show: 29
ls LMs/*.mrk.json | wc -l      # Should show: 29
```

### Sample List

The dataset includes these mouse strains (we'll use all 29 for this tutorial):

- **C57BL6_J_** - C57BL/6J (most common laboratory strain)
- **BALB_CJ_** - BALB/cJ 
- **DBA_1J_** - DBA/1J
- **DBA_2J_** - DBA/2J
- **C3H_HEJ_** - C3H/HeJ
- **AKR_J_** - AKR/J
- **129S1_SVLMJ_** - 129S1/SvlmJ
- **FVB_NJ_** - FVB/NJ
- ... and 21 more strains

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
   - In the **Markups** module, select `C57BL6_J_` from the dropdown
   - Landmarks will appear as small spheres on the skull
   - Adjust visibility/size in the Display section if needed

### Understanding the data better

- None of the samples are oriented canonically; slice planes to not correspond to anatomical planes.
- Landmarks are in millimeters (mm)
- Each specimen has 45 anatomical landmarks on the skull

---

## Step 3: Prepare Reference Specimen

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

### Step 3A: Reorient a Reference Specimen

We'll use the **NZBWF1_J_** specimen as our initial reference and reorient it so the major axes align with anatomical planes.

#### Load the Reference Specimen

0. Reset the scene to remove other data.
1. If not already loaded: `File → Add Data`
2. Navigate to `ANTsamples/volumes/`
3. Select `NZBWF1_J_.nii.gz`
4. Click `Open`, then `OK`

#### Open the Crop Volume Module

1. In the module search bar, type "Crop"
2. Select **Crop Volume**

For detailed instructions, see the [CropVolume tutorial](https://github.com/SlicerMorph/Tutorials/tree/main/Slicer_Modules/Crop_Volume#using-cropvolume-to-simulatenously-reorient-and-resample-your-data).


#### Apply Reorientation

1. Click **Apply**
2. The reoriented volume appears in the scene (it will have the **Cropped** suffix)
3. This will also create a new transformation in the scene that start with **Re-orient**
3. Verify in slice viewers that anatomical planes are aligned with slice views.
4. Save the result: Right-click `NZBWF1_J_reoriented` → Export to file
   - Save as `ANTsamples/NZBWF1_J_reoriented.nii.gz`

**Important:** Also reorient the corresponding landmarks:

1. Load `NZBWF1_J_.mrk.json` if not already loaded
2. Apply the same transform `NZBWF1_J_.mrk.json` landmarks and harden the transform to make it permenant.
3. Save as `NZBWF1_J_reoriented.mrk.json` in the same folder where you save the volume from previous step.

### Step 3B: Rigidly Align All Specimens to Reference

Now we'll register all specimens to the reoriented reference using rigid registration. This brings them into a common space without changing their shape. We'll use the **Group-wise** registration tab to process all specimens at once.

#### Open ANTsRegistration Module - Group-wise Tab

1. In the module search bar, type "ANTs"
2. Select **ANTsRegistration**
3. Click the **"Group-wise"** tab

#### Configure Rigid Registration Settings

1. **Template:** Select `NZBWF1_J_reoriented`
   - This is your reoriented reference specimen
   - All other specimens will be aligned to this

2. **Transform Type:** Select `Rigid`
   - Only rotation and translation, no scaling or deformation
   - Preserves the original shape and size of each specimen

3. **Input directory:** Click the folder icon
   - Navigate to `ANTsamples/volumes/`
   - Select the `volumes` folder
   - This will process all .nii.gz files in this directory

4. **Output Directory:** Click the folder icon
   - Create a new folder: `ANTsamples/RigidAligned/`
   - Select it
   - All rigidly aligned volumes will be saved here

5. **Output transforms as:** Select `Composite (single file)` or `Separate files`
   - Either option works for rigid transforms since there will be only one output file.
   
6. **Output:** Check these boxes:
   - ☑ **Forward Transform** - Saves the rigid transformation
   - ☐ **Inverse Transform** - Not needed for this step, as all linear transformations (rigid, similarity, affine) are invertable.
   - ☑ **Transformed Volume** - **REQUIRED** - This is what we need for averaging

#### Configure Landmark-based Initial Transform

To ensure accurate rigid alignment:

1. Check ☑ **"Compute landmark-based RIGID initial transform"**

2. **Template Landmarks:** Select `NZBWF1_J_reoriented` landmarks
   - These are the landmarks for your reoriented reference

3. **Landmarks directory:** Click folder icon
   - Navigate to `ANTsamples/LMs/`
   - Select the `LMs` folder
   - The module automatically matches landmarks to volumes by basename

4. ☑ **Save volume aligned LMs** (REQUIRED)
   - **Must check this** to save transformed landmarks
   - These are essential for creating averaged landmarks in Step 3C
   - Creates files with `-transformed.mrk.json` suffix
   - Without these, you cannot compute the average landmark positions for the averaged image.

#### Run Group-wise Rigid Registration

1. Click **"Register"**

2. **What happens:**
   - Each volume in the input directory is rigidly registered to `NZBWF1_J_reoriented`
   - This will result in every transformed volume having the same orientation and image dimensions and resolution as the reference image. 
   - Landmark-based rigid transform is computed first for initialization
   - Final rigid registration refines the alignment
   - Outputs are saved to `ANTsamples/RigidAligned/`

3. **Progress:**
   - Button text: "Group registration in progress"
   - 29 specimens will be processed sequentially
   - **Time estimate:** 5-15 minutes total (~10-30 seconds per specimen)
   - Much faster than deformable registration!

4. **Completion:**
   - Button returns to "Register"
   - Check the output directory for results

#### Verify Outputs

Navigate to `ANTsamples/RigidAligned/` and you should see:

**For each specimen:**
- `[specimen]_Composite.h5` - Rigid transform file
- `[specimen]-transformed.nii.gz` - **Rigidly aligned volume** (required for averaging)
- `[specimen]-transformed.mrk.json` - **Transformed landmarks** (required for averaging)

**Total:** 29 rigidly aligned volumes + 29 transformed landmark sets ready for averaging

#### Verify Rigid Alignment

1. Load the reference: `File → Add Data → NZBWF1_J_reoriented.nii.gz`
2. Load several rigidly aligned volumes from `ANTsamples/RigidAligned/`:
   - `C57BL6_J_-transformed.nii.gz`
   - `BALB_CJ_-transformed.nii.gz`
   - `DBA_2J_-transformed.nii.gz`

3. In the **Data** module:
   - Toggle visibility between reference and aligned volumes
   - They should be in the same position and orientation
   - Individual shape differences should be clearly visible
   - No warping or deformation (only rotation/translation)

### Step 3C: Create Average Reference Volume

Now we'll create an average of all rigidly aligned specimens. This becomes our unbiased initial reference volume to initialize the iterative template building procedure.

#### Open ANTsRegistration Module - Average Tab

1. In the **ANTsRegistration** module
2. Click the **"Average"** tab

#### Configure Average Settings

1. **Input directory:** Click the folder icon
   - Navigate to `ANTsamples/RigidAligned/`
   - Select the folder containing all rigidly aligned volumes
   - The module will automatically find all .nii.gz files in this directory

2. **Output Volume:** Click the dropdown
   - Select **"Create new Volume"**
   - It will create a node called `Volume`
   - Rename it to `RigidAverage_Template` (click on the name to edit)

#### Run Averaging

1. Click **"Compute Average"**

2. **What happens:**
   - The module loads all .nii.gz files from the input directory
   - Computes the voxel-wise mean across all volumes
   - Creates the output averaged volume
   - **Time estimate:** 1-2 minutes depending on image size

3. **Completion:**
   - The averaged volume `RigidAverage_Template` appears in the Data module
   - Verify it loaded correctly by displaying in slice viewers

#### Save the Average Template

1. Right-click `RigidAverage_Template` in the Data module
2. Select **"Export to file..."**
3. Save as `ANTsamples/RigidAverage_Template.nii.gz`

#### Create Average Landmarks (REQUIRED)

**Critical:** You must average the landmark positions to match your averaged volume. There is no tool in ANTsPy extension to do that. We will use a simple python script to do this in Slicer.

1. **Load all transformed landmark files:**
   - `File → Add Data`
   - Navigate to `ANTsamples/RigidAligned/`
   - Select all 29 `*-transformed.mrk.json` files
   - Click `Open`, then `OK`

2. **Average the landmarks using Python:**
   - Go to **Python Interactor** (View → Python Interactor)
   - Run this script:

```python
import numpy as np
import slicer

# List all loaded landmark nodes (the transformed ones from RigidAligned folder)
# Adjust these names to match what was loaded (typically specimen_-transformed)
landmarkNames = [
    "129S1_SVLMJ_-transformed",
    "129X1_SVJ_-transformed",
    "AKR_J_-transformed",
    "A_J_-transformed",
    "B6AF1_J_-transformed",
    "B6D2F1_J_-transformed",
    "BALB_CBYJ_-transformed",
    "BALB_CJ_-transformed",
    "BTBR_T_Itpr3tf_j_-transformed",
    "C3H_HEJ_-transformed",
    "C3H_HEOUJ_-transformed",
    "C57BL6_J_-transformed",
    "C57BL_10J_-transformed",
    "C57BL_6NJ_-transformed",
    "C57L_J_-transformed",
    "CAF1_J_-transformed",
    "CAST_EIJ_-transformed",
    "CB6F1_J_-transformed",
    "CBA_CAJ_-transformed",
    "CBA_J_-transformed",
    "DBA_1J_-transformed",
    "DBA_2J_-transformed",
    "FVB_NJ_-transformed",
    "MRL_MPJ_-transformed",
    "NOD_SHILTJ_-transformed",
    "NU_J_-transformed",
    "NZBWF1_J_-transformed",
    "NZW_LACJ_-transformed",
    "SJL_J_-transformed",
    "TALLYHO_JNGJ_-transformed"
]

# Get landmark nodes
markupNodes = [slicer.util.getNode(name) for name in landmarkNames]

# Get point arrays
pointArrays = [slicer.util.arrayFromMarkupsControlPoints(node) for node in markupNodes]

# Verify all have same number of points
numPoints = [arr.shape[0] for arr in pointArrays]
if len(set(numPoints)) != 1:
    print(f"ERROR: Not all landmark sets have same number of points: {set(numPoints)}")
else:
    print(f"All {len(pointArrays)} landmark sets have {numPoints[0]} points")
    
    # Compute average
    avgPoints = np.mean(pointArrays, axis=0)
    
    # Create new markup node
    avgMarkups = slicer.mrmlScene.AddNewNodeByClass('vtkMRMLMarkupsFiducialNode', 'RigidAverage_Landmarks')
    
    # Add averaged points
    for i in range(avgPoints.shape[0]):
        avgMarkups.AddControlPoint(avgPoints[i])
    
    print(f"Average landmarks created: RigidAverage_Landmarks with {avgPoints.shape[0]} points")
```

3. Save the averaged landmarks:
   - Right-click `RigidAverage_Landmarks` in Data module
   - Export to file → `ANTsamples/RigidAverage_Landmarks.mrk.json`

**Verify:** The averaged landmarks should have 45 points (same as each individual specimen) and be positioned at the mean location of all specimens' landmarks.

### Discussion: Reference Specimen Bias

**Important considerations:**

1. **Shape bias is reduced but not eliminated:**
   - The rigid average reduces bias from any single specimen
   - However, if your sample is not representative of the full population, bias remains
   - Example: If all specimens are adult males, the template won't represent females/juveniles

2. **The reference affects final template geometry:**
   - Template building is iterative, but initial geometry influences convergence
   - Very different initial references may lead to slightly different final templates
   - For most analyses, this effect is small if you use 2+ iterations

3. **Alternative: No initial template:**
   - Setting "Initial Template" to `None` it uses the first sample in your pool as the reference. We don't advise doing that except for testing purposes. 
 
**For this tutorial:** Using a rigid average provides a good balance between computational efficiency and minimizing bias.

---

## Step 4: Building a Population Template

**Overview:** Template building creates an average shape representing your population using deformable registration. We'll use the original unaligned volumes as the input volumes, and use the landmarks to bring them into alignment with the reference, then run **2 iterations** of template refinement. At the end of the first step module will calculate a new template, and start the second iteration using that template as the reference. 

**TO DO: Explain why we are not using the output of our rigid registration (-transformed volumes) as input to this procedure. Effects of doing image resampling multiple times etc...

### Open the ANTsRegistration Module

1. In the module search bar (top left), type "ANTs"
2. Select **ANTsRegistration**
3. Go to the **Template** tab

### Configure Template Building Settings

#### A. Initial Template

- **Initial Template:** Select `RigidAverage_Template`
  - This is the averaged volume we created from rigidly aligned specimens
  - Using this provides a better starting point than a specific sample.
  - Reduces computational time and improves convergence
  
#### B. Transform Type

- **Transform Type:** Select `SyN` (Symmetric Normalization)
  - This is a deformable registration that can capture shape differences
  
#### C. Number of Iterations

- **Iterations:** Set to `2`
  - Each iteration refines the template
  - More iterations = better template but longer computation time
  - 2-3 iterations are typically sufficient

### Select Input Images

1. Click **"Select input images..."**
2. A file browser opens
3. Navigate to `ANTsamples/volumes/`
4. Select **all 29 .nii.gz files**:
   - Click the first file
   - Scroll down, hold Shift, click the last file
   - Or use Ctrl+A (Cmd+A on Mac) to select all
5. Click **Open**

**View Input Image Paths:**
- Click the **"View input image paths"** collapsible button
- You should see all 29 file paths listed
- Verify the order is correct
- Use **"Remove Selected Path"** or **"Move Path to Top"** if you need to adjust
- **"Clear input path list"** removes all files

### Configure Landmark-based Initial Transform

This ensures specimens are aligned to the reference before template building begins.

1. Check ☑ **"Compute landmark-based RIGID initial transform"**

2. Click **"Select landmark files..."**

3. Navigate to `ANTsamples/LMs/`

4. Select **all 29 .mrk.json files** (same order as volumes)
   - The order must match your volume files!
   - First volume → first landmark file, etc.

5. Click **Open**

**Verify landmark selection:**
- Click **"View landmark file paths"** to expand
- Check that you have 29 landmark files
- Ensure the basenames match the volume files
  - Example: `C57BL6_J_.nii.gz` ↔ `C57BL6_J_.mrk.json`

**Important Note:** The module will validate this for you. If there's a mismatch in count or basenames, you'll get a clear error message when you try to run.

6. **Template Landmarks:** Select `RigidAverage_Landmarks`
   - These are the average landmark positions we created
   - Used if you provide an initial template volume
   - Helps with landmark-based initialization

### Configure Output Settings

1. **Output template:**
   - Click the dropdown
   - Select **"Create new Volume"**
   - It will create a node called `Template`
   - You can rename it if desired (e.g., `MouseCranium_Template`)

2. **Output landmarks:**
   - Click the dropdown
   - Select **"Create new MarkupsFiducial"**
   - Creates `Template_Landmarks` node
   - This will contain the average landmark positions

3. **Output Directory:**
   - Click the folder icon
   - Navigate to `ANTsamples/` (or create a new folder like `ANTsamples/TemplateOutput/`)
   - Select the folder
   - This is where all intermediate files and transforms will be saved

### Run Template Building

1. Click **"Run Template Building"**

2. **What happens:**
   - Initial alignment: Each specimen is rigidly aligned to the first specimen using landmarks
   - Iteration 1: All specimens are registered to the specified template, and then the template is updated
   - Iteration 2 (and subsequent iterations): Re-registrat the samples to the updated template
   - Calculate the final template.
   - Output files are saved to the output directory

3. **Progress monitoring:**
   - Button text changes to "Template building in progress"
   - Console output shows registration progress (View → Python Interactor)
   - **Time estimate:** 10-30 minutes depending on your computer
     - Per specimen: ~30 seconds to 2 minutes
     - 29 specimens × 2 iterations = 58 registrations

4. **Completion:**
   - Button returns to "Run Template Building"
   - Template volume appears in the Data module
   - Template landmarks appear in the Markups module

### Visualize the Template

1. In the **Data** module, find your template volume (e.g., `MouseCranium_Template`)
2. Click the eye icon to show it in the viewers
3. In the **Markups** module, select `Template_Landmarks`
4. The template represents the average cranial shape of all 29 mouse strains
5. Review for anatomical detail. You can try increasing the number of iterations. 

### Save the Template

1. Right-click the template volume in the Data module
2. Select **"Export to file..."**
3. Save as `MouseCranium_Template.nii.gz` in your project folder

4. Do the same for template landmarks:
   - Right-click `Template_Landmarks`
   - Save as `Template_Landmarks.mrk.json`

---

## Step 5: Group-wise Registration to Template

Now we'll register all individual specimens to the template and save the deformation fields (needed for Jacobian analysis). These deformations are going to be used to calculate systematic localized shape difference between groups. 

### Open the Group-wise Tab

Click the **"Group-wise"** tab

### Configure Registration Settings

#### A. Select Template

- **Template:** Select your template volume from the dropdown
  - If you just created it: select `MouseCranium_Template` (or whatever you named it)
  - If you saved and reloaded: use `File → Add Data` to load it first

#### B. Transform Type

- **Transform Type:** Select `SyN`
  - Must match what you saved as the output of the  template building
  - SyN provides deformable registration

#### C. Input Directory

- **Input directory:** Click the folder icon
- Navigate to `ANTsamples/volumes/`
- Select the `volumes` folder
- This will process all .nii.gz files in this directory

#### D. Output Directory

- **Output Directory:** Click the folder icon
- Create a new folder: `ANTsamples/GroupRegistration/` (or similar)
- Select it
- All output transforms and volumes will be saved here

#### E. Output Transform Options

- **Output transforms as:** Select `Separate files`
  - Creates individual forward and inverse transform files
  - **Required for Jacobian analysis** to isolate deformable component
  - Composite transforms include affine component of the deformation that can mask subtle shape differences

**Why separate files?**
- Jacobian analysis should use only the deformable (warp) component
- Composite transforms combine affine + deformable transformations
- The affine component (global scaling/rotation) can dominate and obscure local shape variations
- Separate files allow you to use only the forward warp for statistical analysis

**Understanding the output structure:**

When you select "Separate files", ANTs creates a multi-step transformation for each specimen:

1. **Affine component** (`*0GenericAffine.mat`):
   - Global alignment (rotation, translation, scaling, shearing)
   - Applied first to roughly align specimen to template
   - Small text file (~1-2 KB)
   - Example: `C57BL6_J_0GenericAffine.mat`

2. **Forward deformable warp** (`*1Warp.nii.gz`):
   - Local, non-linear deformations
   - Captures shape differences after global alignment
   - Large image file (same dimensions as input, ~10-50 MB)
   - **This is what we use for Jacobian analysis**
   - Example: `C57BL6_J_1Warp.nii.gz`

3. **Inverse deformable warp** (`*1InverseWarp.nii.gz`) - Optional:
   - Reverses the forward warp
   - Useful for mapping results back to original specimen space
   - Same size as forward warp
   - Example: `C57BL6_J_1InverseWarp.nii.gz`

**Files per specimen:**
- If you check **only Forward Transform**: 2 files (1 affine + 1 forward warp)
- If you check **both Forward and Inverse**: 3 files (1 affine + 1 forward warp + 1 inverse warp)

**For 29 specimens:**
- Forward only: 58 files total (29 affine + 29 forward warps)
- Forward + Inverse: 87 files total (29 affine + 29 forward + 29 inverse)

The numbering (0, 1) indicates the order of application:
- Step 0: Apply affine transform first
- Step 1: Apply deformable warp second

#### F. Select Outputs to Save

Check the boxes for what you want:

- ☑ **Forward Transform** - **REQUIRED** for Jacobian analysis (deformable component)
- ☐ **Inverse Transform** - Optional, but suggested (useful for mapping results back to specimen space)
- ☑ **Transformed Volume** - Optional, but suggested (useful to verify registration quality)

**Important:** You MUST save forward transforms for Jacobian analysis!

### Configure Initial Transform (Landmark-based)

To improve registration accuracy:

1. Check ☑ **"Compute landmark-based RIGID initial transform"**

2. **Template Landmarks:** Select `Template_Landmarks` from the dropdown
   - These are the average landmarks you created during template building

3. **Landmarks directory:** Click folder icon
   - Navigate to `ANTsamples/LMs/`
   - Select the `LMs` folder
   - The module will automatically match landmarks to volumes by basename

4. ☑ **Save volume aligned LMs** (Optional)
   - Check this to save the transformed landmarks
   - Useful for validation and quality control
   - Creates files with `-transformed.mrk.json` suffix

### Run Group Registration

1. Click **"Register"**

2. **What happens:**
   - Each volume in the input directory is registered to the template
   - Landmark-based rigid transform is computed first
   - Then deformable registration (SyN) is applied
   - Outputs are saved to the output directory

3. **Progress:**
   - Button text: "Group registration in progress"
   - 29 specimens will be processed sequentially
   - **Time estimate:** 15-45 minutes total (~30-90 seconds per specimen)

4. **Completion:**
   - Button returns to "Register"
   - Check the output directory for results

### Verify Outputs

Navigate to `ANTsamples/GroupRegistration/` and you should see:

**For each specimen (example for C57BL6_J_):**
- `C57BL6_J_0GenericAffine.mat` - Affine transform component
- `C57BL6_J_1Warp.nii.gz` - **Forward deformable warp** (used for Jacobian analysis)
- `C57BL6_J_1InverseWarp.nii.gz` - Inverse warp (if you enabled inverse transforms)
- `C57BL6_J_-transformed.nii.gz` - Registered volume (if you checked that option)
- `C57BL6_J_-transformed.mrk.json` - Transformed landmarks (if you checked that option)

**Total files:**
- 29 affine transforms (.mat files)
- 29 forward warps (.nii.gz files) - **These are used for Jacobian analysis**
- 29 inverse warps (if selected)
- 29 transformed volumes (if selected)
- 29 transformed landmarks (if selected)

### Quality Control

To verify registration quality:

1. Load the template in Slicer: `File → Add Data → MouseCranium_Template.nii.gz`
2. Load a few transformed volumes: 
   - `C57BL6_J_-transformed.nii.gz`
   - `BALB_CJ_-transformed.nii.gz`
   - `DBA_2J_-transformed.nii.gz`

3. In the **Data** module:
   - Toggle visibility between template and transformed volumes
   - They should be well-aligned
   - Anatomical features should overlap

4. Use the **Markups** module to check landmark alignment:
   - Load template landmarks and a few transformed landmark sets
   - They should be close to each other

---

## Step 6: Create Template Mask for Statistical Analysis

Before performing Jacobian analysis, we need to create a mask that defines the anatomical region of interest. This restricts the statistical analysis to biologically relevant structures (skull and mandible) and excludes background, soft tissue, and other elements.

**Why create a mask?**
- **Reduces computational load:** We don't analyze every voxel in the image
- **Focuses on relevant anatomy:** Only skull and mandible for craniometric analysis
- **Improves statistical power:** Multiple comparison correction is less severe with fewer voxels
- **Excludes artifacts:** Background noise and reconstruction artifacts are excluded
- **Biological relevance:** We're interested in bone shape, not soft tissue or air

### Load the Template

1. If not already loaded: `File → Add Data`
2. Select `MouseCranium_Template.nii.gz`
3. Click `Open`, then `OK`

### Open Segment Editor Module

1. In the module search bar, type "Segment"
2. Select **Segment Editor**

### Create New Segmentation

1. **Segmentation:** Click the dropdown
2. Select **"Create new Segmentation"**
3. A new segmentation node appears in the scene

4. **Source geometry:**
   - Click the dropdown
   - Select `MouseCranium_Template`
   - This ensures the segmentation matches the template geometry

### Segment the Skull and Mandible

We'll use semi-automatic and manual tools to define the cranium and mandible.

#### Method 1: Threshold-based Segmentation (Faster)

1. **Add a new segment:**
   - Click **"Add"** button
   - Name it "Skull_Mandible"
   - Choose a color (e.g., yellow)

2. **Use Threshold effect:**
   - Select **"Threshold"** from the Effects list
   - Adjust the threshold sliders to capture bone:
     - Drag the minimum slider until skull/mandible appears
     - Drag the maximum slider to exclude very bright artifacts
     - **Tip:** Start with automatic threshold, then refine manually
   - Click **"Apply"**

3. **Clean up the segmentation:**
   - **Islands effect:**
     - Select **"Islands"**
     - Choose "Keep largest island"
     - Click **"Apply"**
     - This removes small disconnected pieces
   
   - **Smoothing effect:**
     - Select **"Smoothing"**
     - Choose "Median" method
     - Kernel size: 3-5 mm
     - Click **"Apply"**
     - This reduces noise and stair-stepping

4. **Remove unwanted structures:**
   - If soft tissue, nasal turbinates, or other elements are included:
   - Use **"Scissors"** effect:
     - Select "Scissors"
     - Choose "Erase inside" or "Erase outside"
     - Draw around regions to remove
     - Click **"Apply"**
   
   - Use **"Paint"** effect for manual refinement:
     - Select "Paint"
     - Adjust sphere brush size
     - Hold Shift to erase, regular click to add
     - Paint in each slice view to refine boundaries

#### Method 2: Manual Segmentation (More Control)

If automatic threshold doesn't work well:

1. **Use "Draw" effect:**
   - Select **"Draw"** from Effects
   - Draw around the skull outline in each slice
   - Work through Red, Yellow, and Green slice views
   - Time-consuming but very accurate

2. **Use "Level Tracing":**
   - Select **"Level Tracing"**
   - Click points around the boundary
   - It traces edges between points
   - Faster than manual drawing

3. **Combine with "Fill between slices":**
   - Segment every 5-10 slices manually
   - Use **"Fill between slices"** effect to interpolate
   - Review and refine interpolated slices

### Verify the Segmentation

1. **Toggle visibility:**
   - Click the eye icon next to the segment
   - Verify it covers skull and mandible only
   - Check in all three slice views

2. **Use 3D view:**
   - The segmentation appears as a 3D surface
   - Rotate to check coverage
   - Look for holes or included unwanted structures

3. **Adjust transparency:**
   - In Segment Editor, adjust opacity slider
   - Overlay on the template volume to verify accuracy

### Refine Specific Regions

**Include:**
- ✅ Cranium (all skull bones)
- ✅ Mandible (lower jaw)
- ✅ Zygomatic arches
- ✅ Occipital region

**Exclude:**
- ❌ Nasal turbinates (thin internal structures)
- ❌ Teeth (if analyzing cranial shape only)
- ❌ Soft tissue
- ❌ Background and air
- ❌ Cervical vertebrae (if visible)

**Tip:** The goal is to include the bony structures relevant to your morphometric question while excluding everything else.

### Export Segmentation as Label Map

1. In **Segment Editor**, click **"Segmentations"** button (or go to Segmentations module)

2. In the **Segmentations** module:
   - Select your segmentation
   - Under **"Export/Import"** section
   - Click **"Export to files"**

3. Configure export:
   - **Destination:** Choose `ANTsamples/`
   - **File format:** Select "NRRD" or "NIfTI"
   - **Filename:** `Template_Mask.nrrd` (or `.nii.gz`)
   - Click **"Export"**

**Alternative export method:**
1. Right-click the segmentation in Data module
2. Select **"Export visible segments to binary labelmap"**
3. This creates a labelmap node
4. Right-click the labelmap → Export to file
5. Save as `Template_Mask.nrrd`

### Load the Mask for Analysis

1. `File → Add Data`
2. Select `Template_Mask.nrrd`
3. The mask appears as a labelmap volume
4. Verify it loaded correctly by displaying it

**The mask is now ready to use in Jacobian analysis!**

---

## Step 7: Jacobian Analysis

Jacobian determinant analysis quantifies local volume changes (expansion/contraction) between each specimen and the template. We'll compare two groups of mouse strains, restricting analysis to the skull and mandible mask we just created.

### Prepare Group Assignment

Since you'll provide a CSV file later, we'll use a random split for now.

#### Create a Covariate Table

1. Create a text file: `ANTsamples/covariates.csv`

2. Add this content (random group assignment):

```csv
specimen,group
129S1_SVLMJ_,A
129X1_SVJ_,B
AKR_J_,A
A_J_,B
B6AF1_J_,A
B6D2F1_J_,B
BALB_CBYJ_,A
BALB_CJ_,B
BTBR_T_Itpr3tf_j_,A
C3H_HEJ_,B
C3H_HEOUJ_,A
C57BL6_J_,B
C57BL_10J_,A
C57BL_6NJ_,B
C57L_J_,A
CAF1_J_,B
CAST_EIJ_,A
CB6F1_J_,B
CBA_CAJ_,A
CBA_J_,B
DBA_1J_,A
DBA_2J_,B
FVB_NJ_,A
MRL_MPJ_,B
NOD_SHILTJ_,A
NU_J_,B
NZBWF1_J_,A
NZW_LACJ_,B
SJL_J_,A
TALLYHO_JNGJ_,B
```

3. Save the file

**Note:** When you provide your real CSV file later, replace this with your actual groupings. The CSV must have:
- Column 1: `specimen` (basename without extensions)
- Column 2: `group` (categorical factor)
- Optional: Additional columns for covariates (age, sex, etc.)

### Open the Analysis Tab

1. In **ANTsRegistration** module
2. Click the **"Analysis"** tab

### Configure Jacobian Analysis Inputs

#### A. Registration Output Directory

- **Registration Output Directory:** Click folder icon
- Navigate to `ANTsamples/GroupRegistration/`
- Select the folder containing your registration outputs

#### B. Filename Pattern

- **Filename end pattern:** Enter `1Warp.nii.gz`
  - This matches the forward deformable warp files from group registration
  - The module will find all files ending with this pattern
  - Example: finds `C57BL6_J_1Warp.nii.gz`, `BALB_CJ_1Warp.nii.gz`, etc.
  - **Important:** We use only the deformable component (1Warp), NOT the composite or affine transforms
  - This isolates local shape changes from global scaling/rotation effects

#### C. Template Volume

- **Template Volume:** Select your template from the dropdown
  - `MouseCranium_Template` (or whatever you named it)
  - If not loaded: `File → Add Data` to load it first

#### D. Template Mask (Required)

- **Template Mask:** Select `Template_Mask` from the dropdown
  - This is the skull/mandible segmentation we created in Step 6
  - Restricts analysis to biologically relevant bone structures
  - Excludes background, air spaces, and soft tissue
  - **Critical:** Without a mask, analysis includes irrelevant voxels and wastes computation
  - If you skipped Step 6, go back and create the mask now

### Load Input Images

The module needs to know which transform files correspond to which specimens.

1. Click the **"View Input Image List"** collapsible button

2. The list should auto-populate with files matching your pattern

3. Verify you see 29 files listed

If the list is empty:
- Double-check the directory path
- Verify the filename pattern (`1Warp.nii.gz`)
- Make sure forward warp files exist in that directory
- Ensure you selected "Separate files" (not "Composite") during group registration

### Configure Regression Analysis

Expand the **"Regression"** section.

#### Import Covariates Table

1. Expand **"Import Covariates table"**
2. **Select path to covariates table:** Click the folder icon
3. Navigate to `ANTsamples/covariates.csv`
4. Select the file

The module will read your CSV and extract factor variables.

#### Set Regression Formula

1. **Rformula for regression:** Enter:
   ```
   log_jacobian ~ group
   ```

   **Explanation:**
   - `log_jacobian` - dependent variable (computed by ANTs)
   - `~` - regressed on
   - `group` - your factor variable from the CSV
   - This tests if group A and B differ in local volume

   **Advanced formulas:**
   - `log_jacobian ~ group + age` - Include age as covariate
   - `log_jacobian ~ group * sex` - Test interaction effects
   - `log_jacobian ~ C(group, Treatment('A'))` - Set reference group

2. **Analysis Cache:** Click folder icon
   - Select a location to save results: `ANTsamples/JacobianCache.pkl`
   - This saves the analysis so you can reload it later without recomputing
   - Useful for trying different visualizations

### Run Jacobian Analysis

1. Click **"Run Image Regression"**

2. **What happens:**
   - Jacobian determinant maps are computed from each deformation field
   - Log transformation is applied (log_jacobian)
   - Statistical regression is performed at each voxel
   - Results are saved to the cache file

3. **Progress:**
   - This is computationally intensive
   - **Time estimate:** 5-15 minutes depending on image size and CPU
   - Watch the Python console for progress messages

4. **Completion:**
   - Button becomes available again
   - Results are cached and ready for visualization

### Generate Statistical Maps

Expand the **"Image Generation"** section.

#### Configure Output Image

1. **Q Values Image:** Select **"Create new ScalarVolume"**
   - This will create a volume showing q-values (FDR-corrected p-values)
   - Defaults to name `QValuesImage`
   - Rename if desired (e.g., `MouseCranium_QValues_GroupComparison`)

2. **Analysis Cache:** Click folder icon
   - Select `ANTsamples/JacobianCache.pkl` (the file you just created)
   - This loads the pre-computed analysis

3. **Factor for QValue computation:** Select `group`
   - This is the factor you want to visualize
   - If you had multiple factors, you'd choose which one to map

#### Generate Images

1. Click **"Generate Output Images"**

2. **What happens:**
   - Q-value map is created showing where groups differ significantly
   - False Discovery Rate (FDR) correction is applied
   - Output volume appears in the Data module

3. **Completion:**
   - Q-value image appears in slice viewers
   - Lower q-values (darker) = more significant differences
   - Higher q-values (brighter) = less significant

### Visualize Results

#### View Q-value Map

1. In the **Volumes** module:
   - Select your Q-value volume (e.g., `QValuesImage`)
   
2. Adjust the **Window/Level** to enhance visualization:
   - In the slice viewers, click the link icon (top left)
   - Drag to adjust brightness/contrast
   - Or use the Volume Rendering module for 3D visualization

3. **Interpretation:**
   - **Dark regions** (low q-values, e.g., < 0.05): Significant group differences
   - **Bright regions** (high q-values, e.g., > 0.05): No significant difference
   - Q-values are FDR-corrected for multiple comparisons

#### Create a Threshold Map

To see only significant regions:

1. Go to **Segment Editor** module
2. Create a new segmentation
3. Use **Threshold** effect:
   - Set range: 0.0 to 0.05 (for q < 0.05)
   - Click "Apply"
4. This creates a binary mask of significant regions

#### Overlay on Template

1. In **Data** module:
   - Load both template and Q-value image
2. In **Volumes** module:
   - Set template as background
   - Set Q-value as foreground
   - Adjust foreground opacity slider

#### Generate 3D Visualization

1. Go to **Volume Rendering** module
2. Select your Q-value volume
3. Click the eye icon to enable rendering
4. Adjust **Shift** slider to set threshold
5. Change colormap to highlight significant regions

### Export Results

Save your analysis results:

1. **Q-value volume:**
   - Right-click in Data module → Export to file
   - Save as `MouseCranium_QValues_GroupAvsB.nii.gz`

2. **Analysis cache:**
   - Already saved as `JacobianCache.pkl`
   - Can reload for future analyses

3. **Screenshots:**
   - Use Slicer's screenshot feature: Ctrl+Shift+G (Cmd+Shift+G on Mac)
   - Or module menu → Screen Capture

### Interpretation Guidelines

**Jacobian Determinant Values:**
- **J > 1**: Local expansion relative to template
- **J = 1**: No volume change
- **J < 1**: Local contraction relative to template

**Log-Jacobian (used in regression):**
- **log(J) > 0**: Expansion
- **log(J) = 0**: No change
- **log(J) < 0**: Contraction

**Q-values:**
- **< 0.05**: Statistically significant difference (5% FDR)
- **< 0.01**: Highly significant (1% FDR)
- **> 0.05**: Not significant after multiple comparison correction

**Example Biological Interpretation:**
If group A shows significant expansion (positive log-Jacobian, low q-value) in the frontal region compared to group B, this suggests group A has relatively larger frontal bones.

---

## Troubleshooting

### Issue: "Number of images doesn't match number of landmarks"

**Cause:** Mismatch between selected image files and landmark files.

**Solution:**
1. Check the "View input image paths" and "View landmark file paths" lists
2. Ensure you have the same number of files in each list (e.g., 29 and 29)
3. Verify the order matches - first image corresponds to first landmark file
4. Check basenames match (e.g., `C57BL6_J_.nii.gz` ↔ `C57BL6_J_.mrk.json`)

### Issue: "Landmark file mismatch: Missing matching file for X"

**Cause:** Basename mismatch between volumes and landmarks.

**Solution:**
1. Check the error message - it tells you which file is missing a match
2. Verify landmark file exists with matching basename
3. Check for typos in filenames
4. Ensure consistent naming (underscores, capitalization, etc.)

### Issue: Template building runs but produces poor results

**Possible causes:**
- Initial alignment failed (try checking landmark-based initialization)
- Transform type too restrictive (try SyN instead of Rigid)
- Too few iterations (increase to 3-4)
- Image quality issues

**Solution:**
1. Verify landmarks are correctly placed on all specimens
2. Check that initial rigid alignment worked (use Python console output)
3. Try increasing iterations
4. Visualize intermediate templates in the output directory

### Issue: Registration is very slow

**Causes:**
- Large image dimensions
- High-resolution scans
- Limited CPU resources

**Solutions:**
1. Reduce image resolution: Use **Crop Volume** or **Resample Scalar Volume** modules
2. Close other applications to free up RAM
3. Process fewer specimens initially to test settings
4. Consider using a faster transform type (e.g., Affine before SyN)

### Issue: Jacobian analysis fails or produces unexpected results

**Causes:**
- Incorrect transform file format
- Missing files
- Wrong filename pattern

**Solutions:**
1. Verify all `*1Warp.nii.gz` files exist in the directory
2. Check the filename pattern matches your files exactly (`1Warp.nii.gz`)
3. Ensure you used "Separate files" output (not "Composite") during registration
4. Ensure covariates CSV has correct specimen names (match basenames)
5. Load cache file if previously completed successfully

### Issue: Q-value image is all white/blank

**Causes:**
- No significant differences found
- Threshold too restrictive
- Analysis hasn't run successfully

**Solutions:**
1. Check if analysis completed (look for cache file)
2. Adjust Window/Level in Volumes module
3. Try different statistical threshold (e.g., q < 0.1 instead of 0.05)
4. Verify group assignments in CSV are correct
5. Check sample size is adequate (need at least 3-5 per group)

### Issue: "Transform could not be loaded" error

**Cause:** Incompatible transform file format or corrupted file.

**Solution:**
1. Check that registration completed successfully
2. Verify warp files (`*1Warp.nii.gz`) are not corrupted (check file sizes > 0)
3. Ensure you selected "Separate files" not "Composite" during group registration
4. Re-run group registration if needed
5. Ensure you're using the correct ANTsPy version

---

## Advanced Topics

### A. Refining Your Template Mask

You can create additional masks for region-specific analysis:

**Example: Neurocranium only (exclude facial bones)**

1. Load the template in Slicer
2. Go to **Segment Editor** module
3. Load your existing skull mask
4. Use **Scissors** effect to remove facial region
5. Export as `Template_Mask_Neurocranium.nrrd`
6. Use this mask in Jacobian analysis to focus on braincase only

**Example: Left vs Right side analysis**

1. Create two masks by splitting the existing mask at midline
2. Use **Scissors** with "Erase outside" to keep only one side
3. Export as `Template_Mask_Left.nrrd` and `Template_Mask_Right.nrrd`
4. Run separate analyses to compare asymmetry between groups

**Example: Regional masks**

Create masks for specific bones:
- Frontal bone only
- Parietal bones only  
- Zygomatic region only
- Mandible only (exclude cranium)

This allows region-specific hypothesis testing.

### B. Multi-level Covariates

For more complex experimental designs:

```csv
specimen,group,sex,age
C57BL6_J_,A,M,8
BALB_CJ_,A,F,10
DBA_2J_,B,M,9
...
```

Regression formula examples:
```
log_jacobian ~ group + sex + age
log_jacobian ~ group * sex
log_jacobian ~ C(group) + age + sex
```

### C. Batch Processing Scripts

For processing large datasets, consider Python scripting.

#### Batch Rigid Alignment Script
Below is a script to rigidly align all specimens to the reference. You will need to manipulate some of the parameters (such as volumesDir) to match paths on your system. The easist way to edit and use these scripts, is installing the `ScriptEditor` extension to Slicer and copy and pasting the code into it. Then you can change things directly in the Slicer and directly execute the code with a simple keystrokes (CMD/CTRL+Enter). [See the tutorial for ScriptEditor for more details](https://github.com/SlicerMorph/Tutorials/blob/main/ScriptEditor/README.MD)

```python
import os
import slicer

# Configuration
volumesDir = "/path/to/ANTsamples/volumes"
landmarksDir = "/path/to/ANTsamples/LMs"
outputDir = "/path/to/ANTsamples/RigidAligned"
referenceVolume = "NZBWF1_J_reoriented"
referenceLandmarks = "NZBWF1_J_reoriented_landmarks"

# Get list of volume files
volumeFiles = [f for f in os.listdir(volumesDir) if f.endswith('.nii.gz')]

# Get module logic
logic = slicer.modules.antsregistration.widgetRepresentation().self().logic

# Get reference nodes
fixedVolume = slicer.util.getNode(referenceVolume)
fixedLandmarks = slicer.util.getNode(referenceLandmarks)

for volFile in volumeFiles:
    basename = os.path.splitext(os.path.splitext(volFile)[0])[0]
    
    # Skip reference
    if basename == "NZBWF1_J_reoriented":
        continue
    
    print(f"Processing {basename}...")
    
    # Load moving volume
    movingVolPath = os.path.join(volumesDir, volFile)
    movingVolume = slicer.util.loadVolume(movingVolPath)
    
    # Load moving landmarks
    landmarkFile = basename + ".mrk.json"
    landmarkPath = os.path.join(landmarksDir, landmarkFile)
    movingLandmarks = slicer.util.loadMarkups(landmarkPath)
    
    # Create output nodes
    outputTransform = slicer.mrmlScene.AddNewNodeByClass('vtkMRMLTransformNode', 
                                                          f'{basename}_rigidTransform')
    outputVolume = slicer.mrmlScene.AddNewNodeByClass('vtkMRMLScalarVolumeNode',
                                                       f'{basename}_rigidlyAligned')
    
    # Set up registration parameters
    # (This is pseudo-code - actual API may differ)
    params = {
        'fixedVolume': fixedVolume.GetID(),
        'movingVolume': movingVolume.GetID(),
        'outputTransform': outputTransform.GetID(),
        'outputVolume': outputVolume.GetID(),
        'transformType': 'Rigid',
        'useInitialTransform': True,
        'initialTransformType': 'landmarks',
        'fixedLandmarks': fixedLandmarks.GetID(),
        'movingLandmarks': movingLandmarks.GetID()
    }
    
    # Run registration
    # logic.runRegistration(params)  # Actual method depends on module implementation
    
    # Save outputs
    outputVolPath = os.path.join(outputDir, f'{basename}_rigidlyAligned.nii.gz')
    slicer.util.saveNode(outputVolume, outputVolPath)
    
    # Clean up
    slicer.mrmlScene.RemoveNode(movingVolume)
    slicer.mrmlScene.RemoveNode(movingLandmarks)
    
    print(f"  Saved: {outputVolPath}")

print("Batch processing complete!")
```

**Note:** The exact API for programmatic ANTs registration may vary. Check the module documentation or examine the module's Python code for the correct method signatures.

### D. Exporting Results for External Analysis

You can export Jacobian maps for analysis in R, Python, or FSL:

1. Save Q-value and Jacobian volumes as NIfTI (.nii.gz)
2. Extract voxel values using **Quantification** modules
3. Export to CSV for statistical software
4. Use template mask to extract ROI values

### E. Visualization with Volume Rendering

Create publication-quality 3D visualizations:

1. **Volume Rendering** module
2. Select Q-value volume
3. Adjust preset: "CT-AAA" or create custom
4. Modify color/opacity transfer functions
5. Use **ROI** to crop display
6. Adjust view angle and lighting
7. **Screen Capture** for figures

### F. Validating Registration Accuracy

Use landmarks to compute registration error:

```python
import numpy as np

# Get fixed and moving landmarks (after registration)
fixedPoints = slicer.util.arrayFromMarkupsControlPoints(fixedMarkups)
movingPoints = slicer.util.arrayFromMarkupsControlPoints(movingMarkups)

# Compute Euclidean distances
errors = np.sqrt(np.sum((fixedPoints - movingPoints)**2, axis=1))

print(f"Mean error: {np.mean(errors):.2f} mm")
print(f"Max error: {np.max(errors):.2f} mm")
```

### G. Alternative Registration Strategies

**For different biological questions:**

- **Rigid only:** For specimens with same shape, different orientation
- **Affine:** For size normalization without shape changes  
- **SyNRA:** Rigid + Affine + SyN (comprehensive)
- **SyNCC:** SyN with cross-correlation metric (for similar intensities)

**Landmark-based registration only:**
- Use **Fiducial Registration Wizard** module for TPS or affine
- Faster but less detailed than image-based

---

## Summary

You've completed a full morphometric analysis pipeline:

1. ✅ **Obtained data** from GitHub repository (29 mouse specimens)
2. ✅ **Prepared reference specimen** by reorienting to anatomical axes
3. ✅ **Created rigid average** from all specimens to minimize bias
4. ✅ **Built a population template** using landmark-initialized, iterative registration
5. ✅ **Registered all specimens** to the template with deformable transforms
6. ✅ **Created anatomical mask** to focus analysis on skull and mandible
7. ✅ **Performed Jacobian analysis** to identify regions of significant group differences
8. ✅ **Visualized results** as statistical maps and 3D renderings

### Key Files Created

- `NZBWF1_J_reoriented.nii.gz` - Reference specimen aligned to anatomical axes
- `RigidAverage_Template.nii.gz` - Average of rigidly aligned specimens
- `RigidAverage_Landmarks.mrk.json` - Average landmark positions
- `MouseCranium_Template.nii.gz` - Final population-averaged template
- `Template_Mask.nrrd` - Skull and mandible mask for statistical analysis
- `GroupRegistration/*0GenericAffine.mat` - Affine transform components
- `GroupRegistration/*1Warp.nii.gz` - Deformable warp fields (used for Jacobian analysis)
- `GroupRegistration/*-transformed.nii.gz` - Registered volumes
- `JacobianCache.pkl` - Cached statistical analysis
- `QValuesImage.nii.gz` - Statistical significance map

### Next Steps

- Replace random groupings with your real experimental design CSV
- Create anatomical masks for region-specific analysis
- Test different covariates and interactions
- Export data for external statistical analysis
- Generate publication-quality figures

### Resources

- **SlicerANTsPy Documentation:** [GitHub](https://github.com/SlicerMorph/SlicerANTsPy)
- **ANTs Documentation:** [ANTs Wiki](https://github.com/ANTsX/ANTs/wiki)
- **3D Slicer Training:** [Slicer Documentation](https://slicer.readthedocs.io/)
- **SlicerMorph:** [slicermorph.github.io](https://slicermorph.github.io/)

### Citation

If you use this workflow in your research, please cite:

- **3D Slicer:** Fedorov A., et al. (2012). 3D Slicer as an image computing platform for the Quantitative Imaging Network. *Magnetic Resonance Imaging*, 30(9), 1323-1341.
- **ANTs:** Avants B.B., et al. (2011). A reproducible evaluation of ANTs similarity metric performance in brain image registration. *NeuroImage*, 54(3), 2033-2044.
- **SlicerMorph:** Rolfe S., et al. (2021). SlicerMorph: An open and extensible platform to retrieve, visualize and analyse 3D morphology. *Methods in Ecology and Evolution*, 12(10), 1816-1825.

---

**Tutorial Version:** 1.0  
**Last Updated:** November 21, 2025  
**Questions?** Open an issue on the SlicerANTsPy GitHub repository
