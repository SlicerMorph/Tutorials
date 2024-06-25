## SkyscanReconImport
This is a convenience function to import an imagestack from Bruker's Skyscan microCT scanners. The only input the user needs to provide is the reconstruction log file saved by the Bruker's Nrecon software. It is possible to batch import a large number of reconstructions as well as automatically downsample them and save to different locations. 


To use the `SkyscanReconImport` module in SlicerMorph, first go to the `Sample Data` module and download the *Bruker/Skyscan mCT Recon Sample.* If you are not familiar with the `Sample Data` module or how to find where Slicer downloads files, please review the tutorials for [`Sample Data`] and [`SlicerMorph Preferences`]. You can skip this step if you already have a dataset reconstructed by Bruker Skyscan software.

Then find the `SkyscanReconImport` under **SlicerMorph->Input and Output** menu folder and simply point out the file browser to the scanner log (**left_side_damaged__rec.log**) provided in the folder. Module will read the correct image spacing, filename prefix provided in the log and automatically populate these fields, so no additional user intervention is required. 


<img src="SkyscanReconImport.png">

If you have a large number of scans that you would like to convert, you can use the **Batch Mode**. The only requirement is that all scans to be converted are under a single directory tree. The user needs to point out to the root of the directory tree, and hit **Find Log Files**. This will parse all sub folders and output all the Nrecon reconstruction log files found in table. Then, the user can specify which wants to convert by checking the box next to log file path (red box). You need to specify output locations for full-resolution volumes, and if selected for downsampled volumes. You can specify 1/2 or 1/4 downsampling ration (per axis)

### About specimen transformation. 

For this transformation to hold, the specimen should be oriented in the scanner bed such that the anterior of the specimen should be closest to the scanner, and operator should be looking down to the dorsal (top) surface (see the specimen picture above). We only tested this with Bruker 1076. So if you are Bruker users, we appreciate if you can provide further feedback about whether this transformation brings your specimen to the correct anatomical orentation. 
### About performance and comparison to ImageStacks functionality
`SkyscanReconImport` doesn't offer any of the ROI functionality of the `ImageStacks` module. It is also a bit memory hungry. However, its main benefit is to correct metadata parsing and ability to convert large number of scans in a batch mode. 
