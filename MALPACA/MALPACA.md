# Executing Multi-Template ALPACA (MALPACA) 
This tutorial contains instructions for executing the multi-template ALPACA (MALPACA) pipeline for automated landmarking that can accomodate large morphological disparity within a sample. The MALPACA pipeline is essentially performing multiple independent ALPACA runs, each of which based on a single template. It then calculate the median from the results of all these ALPACA runs as the final output of landmark estimates. The same parameter setting of ALPACA applies to MALPACA. For the publication of MALPACA, please see https://doi.org/10.1371/journal.pone.0278035. For tutorials of how to run ALPACA and , please refer to: https://github.com/SlicerMorph/Tutorials/blob/main/ALPACA/README.md. 

Download sample data here: https://github.com/SlicerMorph/mouse_models. Extract the files.

### Step 1. Switch to the ALPACA module in 3D Slicer and choose the Batch Processing tab (red). 
In the `Method` entry (dark blue box in the picture below), select the `Multi-Template (MALPACA)` option from the dropdown menu.

<p align="center">
<img src="./kmeans_MALPACA_images/MALPACA_019.png", width = 700>
<p/>

### Step 2. Select required input and output directories (see the picture above).
* In the `Source model(s)` entry (yellow), select the folder that contains the templates (ply format). 
  * **If you are uncertain about which specimens to be used as templates, you may use the accompanied k-means multi-template selection method (see [kmeans templates selection tutorial](https://github.com/SlicerMorph/Tutorials/blob/main/MALPACA/K-means_templates_selection.md)).**
  * If you have used the K-means method to select templates, you can select the dirctory that contains the output templates. 
* In the `Source landmarks` entry (green), select the folder that contains the manual landmark files for the selected templates. **The file names of the template model and landmark files must be identical (without extensions). For example, a mesh file named `specimen1.ply` should have a corresponding landmark file named `specimen1.mrk.json` or `specimen1.fcsv`.** The format of the landmark files should be either 'mrk.json' or 'fcsv'. The 'mrk.json' format is recommended.
* In the `Target model directory` entry (dark grey), select the folder that contains the target models (ply format). These are the specimens that will be landmarked by the MALPACA pipeline.
* In the `Target output landmark directory` (light blue), select the folder for storing the MALPACA output landmark files (mrk.json format) of the target specimens.
* Optional settings
  * `Skip scaling` (blue arrow): skip scaling for the rigid registration
  * `Skip projection` (green arrow): skip projecting estimated landmarks to the surface of the target models
  * `Mesh Quality Control (QC)`: When enabled (recommended), MALPACA will perform quality control checks on all meshes before processing begins. This QC step detects:
    * Degenerate meshes (meshes with no points or faces)
    * Meshes containing NaN (Not a Number) values in point coordinates
    * Meshes containing infinite values in point coordinates
    * Meshes that failed to load properly
    
    If any mesh fails QC checks, batch processing will stop and display detailed error messages identifying the problematic files. This prevents wasted computation time on invalid data. You can disable this option to proceed anyway, but this is not recommended as it may lead to processing failures or invalid results.
  * `Replication Analysis` (grey arrow): check this option to allow replicating ALPACA/MALPACA using the same setting and sample. When this option is checked, the following entry `Number of Replications` will be enables for inputing the number of replication. Each replication will be saved in a separate folder in the `Target output landmark directory`.

### Step 3. Click the `Run auto-landmarking` button (red arrow) to execute the MALPACA pipeline (see the picture above).
Slicer may appear to be in a “no response” condition. This is because the MALPACA is executing, so do not forcing closing the Slicer program.

### Step 4. See MALPACA output.
Open the target output landmark directory specified in Step 2 that stores the MALPACA output landmark files.
* The `advancedparameters.txt` file stores the MALPACA settings.
* The `individual estimates` folder contains landmarks estimated by each individual template stored in the mrk.json format.

<p align="center">
<img src="./kmeans_MALPACA_images/MALPACA_020.png", width = 700>
<p/>


* Each file name's prefix specifies the target specimen, while its postfix specifies the template that is used for landmarking this specimen. For example, the file name`129X1_SVJ_B6CBAF1` suggests that the estimated landmarks of the specimen 129X1_SVJ is derived from using the template B6CBAF1. See the picture below.


<p align="center">
<img src="./kmeans_MALPACA_images/MALPACA_021.png", width = 700>
<p/>


* The `medianEstimates` folder contains the final output of the MALPACA pipeline (mrk.json format). Each landmark file name has a suffix `_median`, suggesting it is the median of all the landmark estimates derived from each template. As of recent updates, MALPACA also generates geometric median estimates (suffix `_geomedian`), which are more robust to outliers than arithmetic medians.

<p align="center">
<img src="./kmeans_MALPACA_images/MALPACA_022.png", width = 700>
<p/>

## Troubleshooting

### File Name Matching Issues
**Problem:** Error message "Could not find the file corresponding to [filename]"

**Solution:** Ensure that:
- Template mesh files and their corresponding landmark files have identical base names (excluding file extensions)
- For example: `mouse1.ply` must have a landmark file named `mouse1.mrk.json` or `mouse1.fcsv`
- File names are case-sensitive on some operating systems
- There are no extra spaces or special characters in file names

### Mesh Quality Control (QC) Failures
**Problem:** Batch processing stops with QC failure messages

**Possible causes and solutions:**
1. **"Mesh contains NaN values in points"**
   - The mesh file is corrupted or was improperly generated
   - Try re-exporting the mesh from your original source
   - Check the mesh in another 3D viewer to verify it displays correctly

2. **"Mesh contains infinite values in points"**
   - Similar to NaN values, indicates corrupted mesh data
   - Re-export the mesh with proper coordinate bounds

3. **"Mesh has no points" or "Mesh has no faces/cells"**
   - The mesh file is empty or severely corrupted
   - Verify the file size is non-zero
   - Try loading the mesh in 3D Slicer's Data module to confirm it's valid

4. **"Failed to load mesh"**
   - The file format may not be supported or the file is corrupted
   - Supported formats: .ply, .obj, .vtk, .vtp, .stl
   - Try converting the mesh to .ply format using another tool

**Note:** While you can disable Mesh QC to proceed with processing, this is **not recommended** as invalid meshes will likely cause processing failures later in the pipeline or produce incorrect results. It's better to fix the problematic meshes first.

