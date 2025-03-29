## Sample Data Module
`Sample Data` is a core Slicer module that lets you download sample datasets bundled with Slicer. Extensions, such as SlicerMorph, can register their own sample datasets with `Sample Data` module and the increase the number of datasets shown. 

Go to **File->Download Sample Data** (or search for Sample Data in Module finder) and review the displayed datasets. First 17 datasets are bundled with core Slicer. Go ahead and load some of these into your scene by simply clicking on them.

These core sample datasets are very useful while learning Slicer and its modules' functionalities. Furthermore, when you encounter a **functionality problem** in Slicer that you wish to report as a **software bug**, one of the first responses from the community is to try to replicate the issue with a sample data. If the issue persists and can be reproduced by using a sample dataset (along with your instructions), it is easier for developers to replicate it exactly and find a solution for it. If your issue does not replicate with the sample dataset, then it might be specific to your computer (perhaps different settings), or specific to your dataset/workflow, and you are likely need to provide more information.  

<img src="./SampleData.png" width="600px"/>

Extensions like SlicerMorph can register their own custom sample dataset, hence what you see as sample data (beyond the first 17 datasets) will be dependent on what extensions you have installed to your Slicer.  

Slicer's core sample data and SlicerMorph's sample datasets behave differently. As you have seen above, Slicer's core datasets are immediately loaded into the active scene. On the contrary, almost all SlicerMorph sample data **do not automatically load into the active scene**, but are saved onto the disk. You will be prompted a **dialog box** in which you will specify where to download the sample file. We advise creating a `SlicerMorph_SampleData` folder on some other convenient location on your computer (e.g., MyData folder if your using MorphoCloud instances), and then save them into this folder. After that, you need to navigate to this folder, and unzip them (for the compressed ones). After that you can manually add those datasets to the scene, or provide them as input to SlicerMorph modules as needed. The only exception to this is the sample data associated with the `FastModelAlign` module, which is automatically loaded into the scene. 

### Loading data from internet

If trying to use a data that is :
1. In a format that can be read into Slicer directly (e.g., NRRD, PLY, mrk.json) without having to the import steps
2. Data has a publicly accessible URL (i.e., none of the data on MorphoSource has a directly accessible URL). 

You can use the SlicerMorph extensions `ImportFromURL` module to load it directly into the Slicer. For example these three datasets are some of our datasets on github:

* https://github.com/SlicerMorph/SampleData/blob/master/IMPC_sample_data.nrrd (A mid-gestation mouse embryo)
* https://github.com/SlicerMorph/SampleData/blob/master/Gor_template_low_res.ply (3D model of a gorilla skull in PLY format)
* https://github.com/SlicerMorph/SampleData/blob/master/Gorilla_template_LM1.json (some cranial landmarks associated with the gorilla skull above)

To load any of these data into Slicer:

1. Hit CTLR+F and search for `ImportFromURL` or go to `SlicerMorph->Input and Output->ImportFromURL` from the module selector dropdown menu.
2. In the `ImportFromURL` module panel, copy and paste one of the URLs from above into the URL field. The other two fields will be auto-populated once you enter URL.
3. Hit `Import Data`

If the data you have loaded is a 3D volume (like the mouse embryo above), it will also automatically volume rendered. Other datasets (such as 3D surface models, markups) are automatically shown in the 3D view.

Remember, this functionality only works for data formats that you can directly load into Slicer (like dragging and dropping into the application window). It will not work with raw image stacks. For that you will have to use the `ImageStacks` functionality of SlicerMorph.

