## ImageStacks
This is a SlicerMorph specific utility to import non-DICOM image sequences (TIF/PNG/JPG/BMP) into 3D Slicer. It provides additional features such as only loading a subset of the data using ROI, downsampling, skipping slice(s) along the Z plane, and reverse the stack order. You can also specify the voxel spacing for your dataset at the import time. `ImageStacks` always produces a ScalarVolume (single channel), so that volumes can be immediately visualized or can be processed with `Segment Editor`.

To use the `ImageStacks` module in SlicerMorph, first go to the `Sample Data` module and download the *Bruker/Skyscan mCT Recon Sample.* If you are not familiar with the `Sample Data` module or how to find where Slicer downloads files, please review the tutorials for [`Sample Data`] and [`SlicerMorph Preferences`]. 

Then find the `ImageStacks` under **SlicerMorph->Input and Output** menu folder and:

1. Click the **Browse...** button and select a *PNG* file in the folder you just unzipped.

<img src="ImageStacks1.png">

2. Image spacing in this dataset is provided in the accompanying left_side_damaged__rec.log file as 35.28 micron. Enter this value in millimeters as 0.03528 for all three axes. Millimeters is the default unit in Slicer. 
3. You leave the **Output Volume** blank, which will use the filename prefix. Or you can choose to create a new volume name. 
4. To preview a low resolution version of your file make sure the **preview** quality option is selected and click **Load Files** and see that all three slices viewers contain a low resolution version of our data.
5. To visualize what the specimen looks like, go to `Data` module and drag and drop  **left_side_damaged__rec** into the 3D viewer (Note: We will cover `Volume Rendering` module in great detail tomorrow. For the time being, this is all you need). You may need to hit the cross hairs and push pin directions (red boxes) to center your rendered image in the 3D widow.

<img src="Data_Volume_Rendering.png">

Notice that the resultant rendering show the damage to the zygomatic arch in the **right side** of the specimen. Curiously, the specimen is named **Left side damaged**. 

Indeed you can check what the real specimen looks like by going to the link below, and confirm that it is indeed the left zygomatic arch that is missing. 

<a href="https://app.box.com/s/zvs162oja7tzszesmygnqs15t631y15m/file/701653679714"> **Picture of the specimen in the sample stack** </A>

This is a standard problem of using image stacks to convey volumetric data. There is no convention of what the **top** of the stack versus **bottom** of the stack is. It is all relative and depends on the scanner vendors' convention. To mitigate this issue, `ImageStacks` offers a **Reverse** option, which basically flips the stack ordering and -in this case- corrects this mirroring.

<img src="ImageStacks2.png">

This is a common problem across 3D visualization programs, when image sequences are used to present 3D data. Because of this, it is very important to have independent confirmation of import procedure. Look for asymmetrical structures on your specimen and confirm that they appear on the correct side in the 3D rendering. After a successful import (correct specimen size, orientation) you should immediately save your data in a proper 3D volume format that will retain this information (which will be our next topic). 


Once you have reversed your image and you want to visualize the full resolution version of your entire file you can go back to the `ImageStacks` module, change the quality from **preview** to **full resolution**

### Using ROI with Image Stacks

Let's say you have a large file and you only want to visualize a portion of the total scanned volume at full resolution, such as only segmenting the braincase of the mouse. Before loading in the full resolution volume, we can do that using a region of interest on the preview resolution image we imported in the previous step. First in the `Data` module click the eye ball button to view the **AnnotationROI** assoicated with your reversed image (Mine is called AnnotationROI_1) and use the colored circles so that the ROI excludes the mouse rostrum (nose). Now go to the `ImageStacks` modlue and select the same **AnnotationROI** for the Region of interest and create a new output volume (I've named mine mouseBraincase), change the Quality to **full resolution** and click **Load Files**


<img src="ImageStacksROI.png">

Notice that the slice views show only the volume in the region of interest and are now full resolution. If you have an articulated specimen (e.g., a full body scan of a fish), and you want to segment only the skull (or a specific region), this is a trick you can use to reduce the memory consumption. Segmentation may require 6-10X more memory compared to your volume size (if `ImageStacks` reports estimated memory usage is 2GB, then you may need up to 20GB in memory to segment the data in full resolution). 

### Further exploration

1. Try importing the same volume with different ROI under different names. Observe that they retain their correct spatial relationships with respect to the full volume. That's because slicer retains the correct spacing and necessary offset values for the origin, so that these partial datasets still line up with the full image. 
2. Try drag and dropping one slice from the folder where the data sits into Slicer. (HINT: When asked whether you want to use ImageStacks to import data, say yes)
3. Try using the `ImageStacks` tool to import any data you might have or downloaded from the MorphoSource. 
