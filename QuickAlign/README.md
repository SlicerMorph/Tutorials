# QuickAlign
In research 3D imaging, often there is no fixed orientation in which samples are scanned consistently. For comparative purposes, it is often easier to bring these 3D specimen into approximately similar anatomical orientation to compare. If precision is required and necessary, this can be done by using the automated registration tools in Slicer and SlicerMorph, depending on whether the data is a volume or a 3D model. However, for simpler side-by-side comparison of two objects, **QuickAlign** module can provide a convenient alternative to align volumes, models and landmarks (pointLists). 

Additionally, **QuickAlign** provides linking two landmarks sets in this orientation, so that point-to-point correspondences of dense landmarksets can be easily compared by selecting specific points, which then gets highlighted in the linked dataset as well. 

Using sample data from SlicerMorph, this tutorial explains how to align two specimens that might be in different orientation and size side-by-side. To follow the rest of the tutorial, download both **Gorilla Skull Reference Model** and **Mouse Skull Reference Model** from the `Sample Data` module of Slicer. Save the downloaded zip files somewhere you can easily find, and unzip them. 

1. Drag and drop both two models into Slicer (**4074_skull.vtk** is the mouse model, and **Gor_template_low_res.ply** is the gorilla model.)
2. Find the `QuickAlign` module in the module finder (CTRL+F or CMD+F in MacOS) and switch to it
3. Assign Mouse model as **Object 1** and gorilla model as **Object 2**
4. Hit **Initialize View** button. This will change your Slicer layout to a 2x2 grid showing four 3D views:
   - **View 1** (top-left): Object 1 superior view (looking down from top)
   - **View 2** (top-right): Object 2 superior view
   - **View 3** (bottom-left): Object 1 lateral/side view
   - **View 4** (bottom-right): Object 2 lateral/side view
5. Both objects are automatically centered at the origin. Adjust the views such that you are looking at the dorsal surface of the skulls, with snouts pointing upwards in the superior views. The zoom levels of superior and side views are automatically synchronized for each object.
6. Hit the **Link** button to synchronize Object 1 and Object 2. The layout will switch to a dual horizontal view showing only Views 1 and 2 (the two superior views).
7. Use your mouse to rotate/zoom/pan one of the models, and notice that the same movement is applied to the other. 
8. When done, hit the **Unlink** button to de-synchronize the objects. You can change your layout back to whatever it was using the **Layout Manager** module. 

## Working with Landmarks

QuickAlign can also be used to align and jointly edit landmark sets. Landmark sets can be specified in two ways:

### Option 1: Using Landmark Files as Objects

You can directly use landmark point lists as Object 1 and Object 2. When both objects are fiducial markups, joint editing is automatically enabled.

### Option 2: Using Separate Landmark Selectors

You can align models or volumes as objects, and separately specify landmark point lists for joint editing using the **Landmarks 1** and **Landmarks 2** selectors (these selectors become available after you hit the **Link** button).

**Important:** For joint editing to work, both landmark sets must have the same number of landmarks.

### Tutorial: Linking Landmark Sets

We can't use the landmarks that came with gorilla and mouse datasets because they have different numbers of points. Download the **Large Apes Skull LMs** sample data, save it to a convenient location, and unzip its contents.

1. Reset your Slicer scene (CTRL+W or CMD+W in MacOS)
2. Drag and drop these two landmark files into Slicer: **gorUSNM252577_LM1.mrk.json** and **ponUSNM145309_LM1.mrk.json** 
3. Switch to QuickAlign module and assign gorilla to **Object 1** and pongo to **Object 2**
4. Hit **Initialize View** button. The layout switches to the 2x2 grid view with four 3D views
5. Arrange the point lists such that they have similar orientations in the superior views (Views 1 and 2)
6. Hit the **Link** button. The layout switches to dual horizontal view, and the landmarks are aligned and synchronized
7. The **Joint Editing** checkbox should be automatically checked (it auto-enables when objects are fiducial markups)
8. Interact with the landmarks and observe they retain your alignment and zoom levels
9. In the right viewer (pongo dataset), hover your mouse over one of the points. This should turn its color to yellow (active state). While still yellow, right-click and from the popup menu, choose **Toggle select control point**. This will toggle the control point state from selected to unselected, or vice versa (review the Markups tutorial to refresh your memory about control point states and their colors: select, unselect, active)
10. Notice that its color will change to light blue, and the corresponding landmark in the gorilla dataset will also change its state and color automatically
11. If you wish to select more than one point at a time, use the MarkupEditor functionality (see the tutorial)
12. When done, hit **Unlink** to end the synchronized session

**Note:** If you're using separate landmark selectors, the landmark files cannot be the same as Object 1 or Object 2. The module will automatically filter them out from the landmark selector dropdowns. 
