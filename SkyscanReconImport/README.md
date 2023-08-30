## SkyscanReconImport
This is a convenience function to import an imagestack from Bruker's Skyscan microCT scanners. The only input the user needs to provide is the reconstruction log file saved by the Bruker's Nrecon software. 


To use the `SkyscanReconImport` module in SlicerMorph, first go to the `Sample Data` module and download the *Bruker/Skyscan mCT Recon Sample.* If you are not familiar with the `Sample Data` module or how to find where Slicer downloads files, please review the tutorials for [`Sample Data`] and [`SlicerMorph Preferences`]. You can skip this step if you already have a dataset reconstructed by Bruker Skyscan software.

Then find the `SkyscanReconImport` under **SlicerMorph->Input and Output** menu folder and simply point out the file browser to the scanner log (**left_side_damaged__rec.log**) provided in the folder. Module will read the correct image spacing, filename prefix provided in the log and automatically populate these fields, so no additional user intervention is required. 


<img src="SkyscanReconImport.png">

### About specimen transformation. 

For this transformation to hold, the specimen should be oriented in the scanner bed such that the anterior of the specimen should be closest to the scanner, and operator should be looking down to the dorsal (top) surface (see the specimen picture above). We only tested this with Bruker 1076. So if you are Bruker users, we appreciate if you can provide further feedback about whether this transformation brings your specimen to the correct anatomical orentation. 
### About performance and comparison to ImageStacks functionality
`SkyscanReconImport` doesn't offer any of the ROI or downsampling functionality of the `ImageStacks` module. As such it reads the entire image stack (as provided by the Nrecon software) into the memory. While convenient, this may not be optimal for large datasets with many gigavoxels, where the entire scanning volume is constructed (e.g., in contains multiple specimens packaged in the same scan). In that case, we advise you to use the `ImageStacks` module. For more information, [see the tutorial for `ImageStacks`.] 

