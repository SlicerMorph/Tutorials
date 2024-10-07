### Crop Volume
Regardless of whether you are only interested in a small subset of your volume, in Slicer the entire volume (and volumes if you are working with more than one dataset in the same scene) is kept in the memory (RAM). During segmentation, memory consumption may increase 10X (e.g. if you have 1GB dataset, at times you may need 10GB of available RAM). You can greatly reduce your memory footprint during segmentation, by creating a new volume that only contains the subset. Alternatively, if you have to work with the full extend of the data, but encounter memory issue during subsequent operations, you may want to downsample your volume.  `CropVolume` is the easiest way of achieving downsampling (or alternatively supersampling) your volumes. 

1. Load the Sample MicroCT data as shown in the `ImageStacks` module

2. Enter the settings shown below. If the ROI creates is too small, hit the **Fit ROI to Volume** button and hit Apply. Then choose to create a new volume that will contain the output of the operation. In my case I called it **left_side_damaged_reduced**, which will be reduced by 50% (Spacing Factor is 2) in each axis. Accordingly, the image spacing will doubled. Compare the reported input and output volume dimensions and spacing. This operation will result in 8 folds reduction in the data volume, at the expense of image detail. This is because spacing scale of 2 is applied to each axis, and since we have 3 axes, the resultant data is reduced 2^3 times, or 8 folds.

<img src="CropVolume1.png" width="800px"/>

2. Now, repeat the procedure one more time, but this time set the limits of the ROI by modifying the small circular handles in slice views. I set my ROI to contain only the nasal region. Choose to create a new output volume (mine was called **Nasal_septum_and_turbinates**), and disable the interpolation. We do not need to interpolate the voxel intensity values, because we simply removing the data outside of our ROI and not doing anything else to the rest. Compare the resultant image dimensions and spacing of input and output volumes. Hit Apply. This procedure also reduces the data volume approximately 10 folds, but there is no reduction in the image quality. 

<img src = "CropVolume2.png">


**Note:** you don't have to use integer values as the Spacing Scale. For example, if you enter 1.25, you reduce memory usage by 2 (1.953 to be more precise) because 1.25^3 is approximately 2. You only degraded your data by 25% in this case.  


More detail about [`CropVolume` can be found here.](https://slicer.readthedocs.io/en/latest/user_guide/modules/cropvolume.html)