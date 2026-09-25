# SlicerMorph Tutorials
These tutorials show how to use SlicerMorph (and other relevant Slicer) modules in digital morphology and morphometrics related tasks.

## Tutorial descriptions

### Imaging Modalities
1. #### [**What is a microCT:** A short review of what microCT imaging is, and how it compares to medical CT and some additional details](https://github.com/SlicerMorph/Tutorials/blob/main/microCT/README.md)
2. #### [**What is photogrammetry:** Background on Structure-from-Motion photogrammetry for biological specimens — how it works, capture/scaling, and what it is good at](https://github.com/SlicerMorph/Tutorials/blob/main/photogrammetry/README.md)
3. #### [**What is surface 3D scanning:** Background on structured-light and laser surface scanning of specimens — how they work, capture considerations, and outputs](https://github.com/SlicerMorph/Tutorials/blob/main/surfaceScanning/README.md)

### Data Import/Export, Downsampling,  Transformations
1. #### [**Getting Data:** Shows how to use the Slicer's `Sample Data` and SlicerMorph's `ImportFromURL` modules to download and access sample data to be used in tutorials](https://github.com/SlicerMorph/Tutorials/blob/main/SampleData/README.md)
3. #### [**ImageStacks:** Tutorial on how to import non-DICOM imagestacks easily into Slicer.](https://github.com/SlicerMorph/Tutorials/blob/main/ImageStacks/README.md)
4. #### [**SkyscanReconImport:** How to import output from Bruker/Skyscan MicroCT](https://github.com/SlicerMorph/Tutorials/blob/main/SkyscanReconImport/README.md)
5. #### [**GEVolImport:** A utility module import 3D volumes from GE/Phoenix scanners with PCR/VOL combination](https://github.com/SlicerMorph/SlicerMorph/tree/master/GEVolImport#gevolimport)
7. #### [**DICOM**](https://github.com/SlicerMorph/Tutorials/blob/main/Slicer_Modules/DICOM/README.md)
8. #### [**MorphoSourceImport:** How to query and retrieve data from MorphoSource using SlicerMorph](https://github.com/SlicerMorph/Tutorials/blob/main/MorphoSourceImport/README.md)
2. #### [**CropVolume**](https://github.com/SlicerMorph/Tutorials/blob/main/Slicer_Modules/Crop_Volume/Readme.MD)
3. #### [**Volumes**](https://github.com/SlicerMorph/Tutorials/blob/main/Slicer_Modules/Volumes/Readme.MD)
9. #### [**Models**](https://github.com/SlicerMorph/Tutorials/blob/main/Slicer_Modules/Models/README.md)
8. #### [**Transforms**](https://github.com/SlicerMorph/Tutorials/blob/main/Slicer_Modules/Transforms/README.md)
9. 

### Visualization
1. #### [**Volume Rendering**](https://github.com/SlicerMorph/Tutorials/blob/main/Slicer_Modules/Volume_Rendering/README.MD)
5. #### [**Lights**](https://github.com/SlicerMorph/Tutorials/blob/main/Slicer_Modules/Lighting/Lights.md)
7. #### [**Animator:** Keyframe animations of 3D scenes (camera, volume rendering, cropping, exploded views) exported as video](https://github.com/SlicerMorph/Tutorials/blob/main/Animator/README.md)
8. #### [**HiResScreenCapture:** How to generate highDPI images](https://github.com/SlicerMorph/Tutorials/blob/main/HiResScreenCapture/readme.md)
9. #### [**Colorize Volume:** Create Colored Volume Rendering from segmentations](https://github.com/SlicerMorph/Tutorials/blob/main/ColorizeVolume/README.md)
10. #### [**QuickAlign:** Allows to approximately align two objects and sync their 3D renderings](https://github.com/SlicerMorph/Tutorials/blob/main/QuickAlign/README.md)

### Markups Related
1. #### [**Markups-I:** Introduction markup types, UI, settings:](https://github.com/SlicerMorph/Tutorials/blob/main/Markups_1/README.md)
9. #### [**Markups-II:** Resampling Semi-landmarks on curves](https://github.com/SlicerMorph/Tutorials/blob/main/Markups_2/README.md)
10. #### [**Markups-III:** Landmark Template Creations](https://github.com/SlicerMorph/Tutorials/blob/main/Markups_3/README.md) 
13. #### [**MarkupEditor:** How to subset/edit dense landmark sets](https://github.com/SlicerMorph/Tutorials/blob/main/MarkupsEditor/README.md)

### Segmentations Related
1. #### [**Overview of Segmentation modules and related data types**](https://github.com/SlicerMorph/Tutorials/blob/main/Segmentation/Segmentation.md)
2. #### [**Creating custom color tables with terminologies**](https://github.com/SlicerMorph/Tutorials/blob/main/Segmentation/colors-and-terms/README.md)
7. #### [**Segmentation tutorial**](https://github.com/SlicerMorph/Tutorials/blob/main/Segmentation/README.md)
10. #### [**Editing 3D Models via Surface Toolbox and Dynamic Modeler**](https://github.com/SlicerMorph/Tutorials/blob/main/Slicer_Modules/Surface_Toolbox/README.md)
11. #### [**Creating Solid Models from Segmentations**](https://github.com/SlicerMorph/Tutorials/blob/main/WaterTightModels/Readme.MD)

### Geometric Morphometrics Related (SlicerMorph & DeCA)
1. #### [**Grid-based Semi-Landmarking:**](https://github.com/SlicerMorph/Tutorials/blob/main/GridBasedLandmarking/README.md)
13. #### [**Creating a template of Pseudo-landmarks via PseudoLMGenerator:**](https://github.com/SlicerMorph/Tutorials/blob/main/PseudoLMGenerator/README.md)
15. #### [**ProjectSemiLMs:** allows you to transfer a semiLM template to new samples using fixed LMs and TPS ](https://github.com/SlicerMorph/Tutorials/blob/main/ProjectSemiLM/README.md)
14. #### [**MergeMarkups:** Merging different kinds of LMs for analysis](https://github.com/SlicerMorph/Tutorials/blob/main/MergeMarkups/README.md)
16. #### [**GPA-I:** Basics of GPA: setting up the analysis (including covariates), exploring the morphospace](https://github.com/SlicerMorph/Tutorials/blob/main/GPA_1/README.md)
17. #### [**GPA-II:** 3D interactive visualization of morphospace and exporting animations](https://github.com/SlicerMorph/Tutorials/blob/main/GPA_2/README.md)
18. #### [**GPA-III:** Linear models with geomorph (procD.lm), from within Slicer](https://github.com/SlicerMorph/Tutorials/blob/main/GPA_3/README.md)
19. #### [**GPA-IV:** Sliding semi-landmarks, and when not to](https://github.com/SlicerMorph/Tutorials/blob/main/GPA_4/README.md)
20. #### [**GPA-V:** Visualizing PCA results as heatmaps](https://github.com/SlicerMorph/Tutorials/blob/main/heatmaps/README.MD)
21. #### [**DeCaL:** Automated semi and fixed landmarking tutorial](https://github.com/SlicerMorph/Tutorials/blob/main/DeCAL/README.md)
22. #### [**DeCA-I:** Dense Surface Correspondence Analysis](https://github.com/SlicerMorph/Tutorials/blob/main/DeCA_1/README.md)
23. #### [**DeCA-II**: Symmetry Analysis](https://github.com/SlicerMorph/Tutorials/blob/main/DeCA_2/README.md)

### ALPACA: Automated Landmarking
1. #### [**ALPACA-I:** Automated landmarking with a single template (single alignment and batch processing)](https://github.com/SlicerMorph/Tutorials/blob/main/ALPACA/README.md)
2. #### [**ALPACA-II:** Building a consensus atlas for unbiased template selection](https://github.com/SlicerMorph/Tutorials/blob/main/MALPACA/Consensus_atlas.md)
3. #### [**ALPACA-III:** Selecting templates with K-means (shape or form space, with or without groups)](https://github.com/SlicerMorph/Tutorials/blob/main/MALPACA/K-means_templates_selection.md)
4. #### [**ALPACA-IV:** Multi-template landmarking (MALPACA)](https://github.com/SlicerMorph/Tutorials/blob/main/MALPACA/MALPACA.md)
5. #### [**ALPACA-V:** Advanced settings, tuning for your data, and BCPD acceleration](https://github.com/SlicerMorph/Tutorials/blob/main/ALPACA/Advanced_settings.md)

### SlicerMorph Photogrammetry
1. #### [**User Guide for the Photogrammetry Extension (previous version, v5.10)**](https://github.com/SlicerMorph/SlicerPhotogrammetry?tab=readme-ov-file#user-guide)
2. #### [**PhotoMasking**](https://github.com/SlicerMorph/SlicerPhotogrammetry/blob/master/docs/PhotoMasking.md)
3. #### [**VideoMasking**](https://github.com/SlicerMorph/SlicerPhotogrammetry/blob/master/docs/VideoMasking.md)
4. #### [**OpenDroneMap**](https://github.com/SlicerMorph/SlicerPhotogrammetry/blob/master/docs/ODM.md)
5. #### [**ClusterPhotos**](https://github.com/SlicerMorph/SlicerPhotogrammetry/blob/master/docs/ClusterPhotos.md)
6. #### [**Photogrammetry module video tutorial**](https://www.youtube.com/watch?v=YRHlb0dGyNc&t=9s) 

### MorphoCloud 
1. #### [**MorphoCloud:** Explains what MorphoCloud is and how you can get started with it](https://github.com/SlicerMorph/Tutorials/blob/main/MorphoCloud/README.md)
2. #### [**NNInteractive:** How to setup AI-assisted interactive segmentation on MorphoCloud](https://github.com/SlicerMorph/Tutorials/blob/main/MorphoCloud/NNInteractive.MD)

### MorphoDepot
1. #### [**MorphoDepot Overview:** How does collabration segmentation workflows work in a nutshell ](https://github.com/SlicerMorph/Tutorials/blob/main/MorphoDepot/README.md#0-conceptual-overview-the-morphodepot-workflow)
2. #### [**Configuration:** Sections 1-2](https://github.com/SlicerMorph/Tutorials/blob/main/MorphoDepot/README.md#1-prerequisites--system-configuration)
3. #### [**Preparing Data for a MorphoDepot Repo:** Sections 3-4](https://github.com/SlicerMorph/Tutorials/blob/main/MorphoDepot/README.md#3-preparing-data-for-morphodepot-repository-3d-volume)
4. #### [**Creating the Repository:** Section 5](https://github.com/SlicerMorph/Tutorials/blob/main/MorphoDepot/README.md#5-creating-the-repository)
5. #### [**Project Management:** Section6, covers both student and repo owner tasks](https://github.com/SlicerMorph/Tutorials/blob/main/MorphoDepot/README.md#6-project-management--assignments)
6. #### [**Review and Merging Submissions:** Covers how repo owners review and send feedback to students?](https://github.com/SlicerMorph/Tutorials/blob/main/MorphoDepot/README.md#7-reviewing--merging-submissions)
7. #### [**Search:** How to find MorphoDepot repositories](https://github.com/SlicerMorph/Tutorials/blob/main/MorphoDepot/README.md#8-search--discovery)

### Image and Surface Model Registration
1. #### [**FastModelAlign:** Fast Model registration via point-clouds (for stable version)](https://github.com/SlicerMorph/Tutorials/blob/main/FastModelAlign/README.md)
2. #### [**FastModelAlign:** Fast Model registration with deformation (for preview version)](https://github.com/SlicerMorph/Tutorials/blob/main/FastModelAlign/README_preview.md)

### ANTsPy: Volumetric Registration, Templates and Jacobian Analysis ([overview](https://github.com/SlicerMorph/Tutorials/blob/main/ANTsPy/README.md))
1. #### [**ANTsPy-I:** Data and reference specimen](https://github.com/SlicerMorph/Tutorials/blob/main/ANTsPy/Data_and_reference.md)
2. #### [**ANTsPy-II:** Group-wise tab, rigid alignment to the reference](https://github.com/SlicerMorph/Tutorials/blob/main/ANTsPy/Groupwise_rigid.md)
3. #### [**ANTsPy-III:** Average tab, an average reference volume](https://github.com/SlicerMorph/Tutorials/blob/main/ANTsPy/Average.md)
4. #### [**ANTsPy-IV:** Template tab, building a population template](https://github.com/SlicerMorph/Tutorials/blob/main/ANTsPy/Template.md)
5. #### [**ANTsPy-V:** Group-wise tab, registering all specimens to the template](https://github.com/SlicerMorph/Tutorials/blob/main/ANTsPy/Groupwise_template.md)
6. #### [**ANTsPy-VI:** Analysis tab, template mask and Jacobian analysis](https://github.com/SlicerMorph/Tutorials/blob/main/ANTsPy/Analysis.md)
7. #### [**ANTsPy-VII:** Pair-wise tab, registering one image to another](https://github.com/SlicerMorph/Tutorials/blob/main/ANTsPy/Pairwise.md)
8. #### [**ANTsPy-VIII:** Troubleshooting and advanced topics](https://github.com/SlicerMorph/Tutorials/blob/main/ANTsPy/Troubleshooting.md)

### Deep-Learning based segmentation models
1. #### [**MEMOS:** A pre-trained segmentation model for E15 fetal mouse scans](https://github.com/SlicerMorph/SlicerMEMOS)
2. #### [**MonaiLabel:** a server-client system that facilitates segmentation by using AI ](https://github.com/Project-MONAI/MONAILabel/tree/main/plugins/slicer) 
3. #### [**MonaiLabel:** How to train new AI based segmentation models from scratch](https://www.youtube.com/watch?v=3HTh2dqZqew) 

### Others
1. #### [**ScriptEditor:** An extension to write and embed python code in Slicer scenes](https://github.com/SlicerMorph/Tutorials/blob/main/ScriptEditor/README.MD)
1. #### [**SlicerMorph preferences:** Explains what customizations can be done to Slicer as well as how to set download directory](https://github.com/SlicerMorph/Tutorials/blob/main/MorphPrefs/README.md)
14. #### [Dr. Jaimi Gray's Quick Guide for various 3D Slicer modules. Icons are clickable and will take you to her YT videos:](http://www.graysvertebrateanatomy.com/__static/c0e61a322fc1f771762f9a4d60fbf7ee/gray-3d-slicer-quick-guide.pdf)


