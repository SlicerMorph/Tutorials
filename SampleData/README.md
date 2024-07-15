## Sample Data Module
`Sample Data` is a core Slicer module that lets you download sample datasets bundled with Slicer. Extensions, such as SlicerMorph, can register their own sample datasets with `Sample Data` module. 

Go to **File->Download Sample Data** (or search for Sample Data in Module finder) and review the displayed datasets. First 17 datasets are bundled with core Slicer. Go ahead and load some of these into your scene by simply clicking on them.

These core sample datasets are very useful while learning Slicer and its modules' functionalities. Furthermore, when you encounter a **functionality problem** in Slicer that you wish to report as a **software bug**, one of the first responses from the community is to try to replicate the issue with a sample data. If the issue persists and can be reproduced by using a sample dataset (along with your instructions), it is easier for developers to replicate it exactly and find a solution for it. If your issue does not replicate with the sample dataset, then it might be specific to your computer (perhaps different settings), or specific to your dataset/workflow, and you are likely need to provide more information.  

<img src="SampleData.png" width="600px"/>

Extensions like SlicerMorph can register their own custom sample dataset, hence what you see as sample data (beyond the first 17 datasets) will be dependent on what extensions you have installed to your Slicer.  

Slicer's core sample data and SlicerMorph's sample datasets behave differently. As you have seen above, Slicer's core datasets are immediately loaded into the active scene. On the contrary, almost all SlicerMorph sample data do not automatically load into the active scene, but are saved onto the disk. You will be prompted a **dialog box** in which you will specify where to download the sample file. We advise creating a `SlicerMorph_SampleData` folder on your desktop (or some other convenient location on your computer), and then save them into this folder. After that, you need to navigate to this folder, and unzip them (for the compressed ones). After that you can manually add those datasets to the scene, or provide them as input to SlicerMorph modules as needed. The only exception to this is the sample data associated with the `FastModelAlign` module, which is automatically loaded into the scene. 

### Description and contents of SlicerMorph datasets

1. **Gorilla Skull Landmarks only:** This archive contains fcsv (landmark) files from 23 Gorilla gorilla skulls housed at the Smithsonion Institution National Museum of Natural History. Each fcsv file contains 41 LMs and labeled with USNM specimen number. You can drag and drop these files directly into Slicer. 

2. **Gorilla Skull Reference Model:** This contains a 3D model (*Gor_template_low_res.ply*) of a gorilla skull averaged from a number of male and female gorilla skulls. It also contains the associated skull LM for the specimen (*Gorilla_template_LM1.fcsv*). We use this sample for interactive visualization of shape deformation and other tutorials. 

3. **Mouse Skull Landmarks only:** This archive contains 3D coordinates of 55 LMs from 126 C57Bl6/J laboratory mice in fcsv format. 

4. **Mouse Skull Reference Model:** This sample data contains a 3D model of one of the skulls from the previous dataset(*4074_skull.vtk*). It also contains the associated skull LM for the specimen (*4074_S_lm1.fcsv*). We use this sample pair for interactive visualization of shape deformation and other tutorials. 

5. **Bruker/Skyscan microCT Recon sample:** This archive contains a PNG stack of a mouse skull scanned via Bruker/Skyscan microCT system. There are 488 PNG files and a reconstruction log file saved by Bruker Nrecon software. We use this sample data in `ImageStacks`, `SkyscanReconImport` and various other tutorials. 

6. **Auto3DM sample data:** This archive contains five 3D models of primate molars in PLY format. 

7. **Gorilla semiLM patches:** This archive contains pairs of fixed (anatomical) and semi-landmark files from 4 Gorilla gorilla skulls housed at the Smithsonion Institution National Museum of Natural History. Anatomical LMs are the same ones in sample data #1 (contains 41 LMs). Patches are created from Sample Data #2 using the [`CreateSemiLMPatches` module](link) and transferred to those  subjects using the [`PlaceSemiLMPatches`](link) module. When unzipped, this archive creates a folder called **Gorilla patch semi-landmarks**. Top level folder contains the template fixed and SemiLM pairs and the subfolder **sample_batch** contains the files to be used in `MergeMarkups` tutorial. 

8. **Large Apes Skulls LMs:** This archive contains 3D coordinates of skull landmarks from a sample of large apes consisting of gorilla (gor), chimpanzee (pan) and orangutan (pon). The first thre letters of the filename indicates the species designation. We use this dataset as an example in MALPACA (multi-template ALPACA) tutorial and elsewhere. 
