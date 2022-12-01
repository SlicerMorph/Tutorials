# Executing Multi-Template ALPACA (MALPACA) 
This tutorial contains instructions for executing multi-template ALPACA (MALPACA) to accomodate large morphological disparity within a sample. The MALPACA pipeline is essentially performing multiple independent ALPACA runs, each of which based on a single template. It then calculate the median from the results of all these ALPACA runs as the final output of landmark estimates. The same parameter setting of ALPACA applies to MALPACA. For tutorials of how to run ALPACA and , please refer to: https://github.com/SlicerMorph/Tutorials/blob/main/ALPACA/README.md. 

Download sample data here: https://github.com/SlicerMorph/mouse_models. Extract the files.

### Step 1. Switch to the ALPCA module in 3D Slicer and choose the Batch Processing tab (red). 
In the `Method` entry (dark blue), open the drop-down menu to select Mu`lti-Template (MALPACA)` option from the dropdown menu.

### Step 2. Select required input and output directories.
* In the “Source model(s)” entry (yellow), select the folder that contains .ply files of the templates. 
  * **If you are uncertain about which specimens to use as templates, you may use the accompanied k-means mult-template selection method (see [kmeans templates selection tutorial](https://github.com/SlicerMorph/Tutorials/blob/main/MALPACA/K-means_templates_selection.md)).**
  * If you have finished the steps in the above tutorial, you can select the dirctory that contains the k-means selected templates. 
* In the “Source landmarks” entry (green), select the folder that contains manual landmark files for the templates. The names of the template model and landmark files must be identical. **The format of the landmark files should be either fcsv or mrk.json**
* In the “Target model directory” entry (dark grey), select the folder that contains .ply files of the target meshes. These are the specimens to be landmarked by MALPCA.
* In the “Target output landmark directory” (light blue), select the folder for MALPACA output.

### Step 3. Click the `Run auto-landmarking` button (red arrow) to execute MALPACA.
Slicer may appear to be in the “no response” condition (Fig. S21). This is because MALPACA is running.

<p align="center">
<img src="./kmeans_MALPACA_images/MALPACA_019.png">
<p/>


### Step 4. See MALPACA output.
Open the target output landmark directory specified in Step 7 that stores the MALPACA output 
* The `advancedparameters.txt `file stores the MALPACA settings.
* The `individual estimates` folder contains landmarks estimated by each individual template stored in the fcsv format (Fig. S22 and Fig. S23). F

<p align="center">
<img src="./kmeans_MALPACA_images/MALPACA_020.png">
<p/>


* or each file name, the postfix is the template that generate this estimated landmark file. For example, `129X1_SVJ_B6CBAF1` suggests that the estimated landmarks of the specimen 129X1_SVJ is derived from using the template B6CBAF1.


<p align="center">
<img src="./kmeans_MALPACA_images/MALPACA_021.png">
<p/>


* The `medianEstimates` folder contains the final output of MALPACA storing in fcsv format (Fig. S22 and Fig. S24). Each landmark file name has a suffix `_median`, suggesting it is the median of the estimates derived from using all templates.

<p align="center">
<img src="./kmeans_MALPACA_images/MALPACA_022.png">
<p/>

