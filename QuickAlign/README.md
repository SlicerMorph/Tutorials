# QuickAlign
In research 3D imaging, often there is no fixed orientation in which samples are scanned consistently. For comparative purposes, it is often easier to bring these 3D specimen into approximately similar anatomical orientation to compare. If precision is required and necessary, this can be done by using the automated registration tools in Slicer and SlicerMorph, depending on whether the data is a volume or a 3D model. However, for simpler side-by-side comparison of two objects, **QuickAlign** module can provide a convenient alternative to align volumes, models and landmarks (pointLists). 

Additionally, **QuickAlign** provides linking two landmarks sets in this orientation, so that point-to-point correspondences of dense landmarksets can be easily compared by selecting specific points, which then gets highlighted in the linked dataset as well. 

Using sample data from SlicerMorph, this tutorial explains how to align two specimens that might be in different orientation and size side-by-side. To follow the rest of the tutorial, download both **Gorilla Skull Reference Model** and **Mouse Skull Reference Model** from the `Sample Data` module of Slicer. Save the downloaded zip files somewhere you can easily find, and unzip them. 

1. Drag and drop both two models into Slicer (**4074_skull.vtk** is the mouse model, and **Gor_template_low_res.ply** is the gorilla model.)
2. Find the `QuickAlign` module in the module finder (CTRL+F or CMD+F in MacOS) and switch to it
3. Assign Mouse model as Object 1 and gorilla model as Object 2, then hit initialize view
4. This will change your Slicer layout to Dual 3D view mode, in which each model is shown in different viewers. Adjust models such that you are looking at the dorsal surface of the skull, and snouts of the specimens are pointing upwards. You can also adjust the zoom levels. 
5. Hit the **Start Sync** button to synchronize the orientation and movements of the specimens. (Disregard the random position change after hitting the button)
6. Use your mouse to rotate/zoom/pan one of the models, and notice that same movement is applied to the other. 
7. When done, hit the **End Sync** button to de-synchronize the objects. You can change your layout back to whatever it was, using the **Layout Manager** module. 

You can use the `QuickAlign` module to link landmarksets. However, for that to work, you need to have same number of landmarks in both landmarksets. So we can't use the landmarks that came with gorilla and mouse datasets. You will need to download another sample data, **Large Apes Skull LMs**. Similarly save this file to a convenient location, and unzip its contents. 

1. Reset your Slicer scene (CTRL+W or CMD+W in MacOS)
2. Drag and drop these two landmark files into Slicer: **gorUSNM252577_LM1.mrk.json** and **ponUSNM145309_LM1.mrk.json** 
3. Switch to QuickAlign module and assign gorilla to Object 1 and pongo to Object 2, then hit **Initialize View** button.
4. Arrange the pointLists such that they have similar orientations. Then hit, **Start Sync**
5. Interact with the landmarks and observe they retain your alignment and zoom levels. 
6. Go to the viewer for pongo dataset (should be Viewer #2) and hover your mouse over one of the points. This should turn its color to yellow (active state), while still yellow, right click your mouse and from the popup menu, choose **Toggle select control point** option. This will toogle the state of the control point from select to unselect, or unselect to select, depending on which state it was before (You may wish to review the Markup tutorial to refresh your memory about control point states and their colors. There are three: select, unselect, active). Notice that its color will change to light blue, and the corresponding landmark in gorilla dataset will also change its state and color. 
7. If you wish to select more than one point at a time, use the MarkupEditor functionality (see the tutorial). 
