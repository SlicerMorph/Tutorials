# Animator


### Setting up the animator
1. Load the "Bruker/Skyscan mCT Recon sample" dataset from Sample Data module using the ImageStacks or SkyscanReconImport as described in their respective tutorials.
2. Go to Volume Rendering module, and enable the volume rendering for the mouse volume. Choose  "Mouse_CT" from the preset. Note that Mouse_CT preset is only available if you enabled the SlicerMorph customizations in Application Settings.
3. If you want to alter the volume property (VP) to your liking (or optimize it for your dataset) do it now. When done, go to "Save Data" and save the Mouse_CT.vp. 

### Volume Property action

4. Reload the Mouse_CT.vp into Slicer twice from where you saved. They will up show as Mouse_CT_1 and Mouse_CT_2 under the Properties section of Volume Rendering module. Use the Rename option to change their names to Mouse_CT_Start and Mouse_CT_End. At this point all three VPs are identical. Adjust the Mouse_CT_End to be a flat line by clicking on the control points on the Scalar Opacity curve and reducing their Opacity (O) value to 0.
5. Switch to the Animator module. Create a new animation, click "Add Action" choose "Volume Property Action", then click "Edit". Set the Start VP to Mouse_CT_Start, End VP to Mouse_CT_End, and Animated VP Mouse_CT. 
6. Go to the Volume Rendering module and review that the Property is set to "Mouse_CT", not "Mouse_CT_Start" or "Mouse_CT_End".  That's because Start and End are static, reference values. The Animated VP (i.e., Mouse_CT) is what's get calculated based on the Start and End VPs and where the animation is in the timeline for each frame rendered. Unless you set the Property of your Volume Rendering module to the one designated as Animated VP in Animator module, your animation won't work. 
7. Go back to the Animator, and hit the play button. You should see the mouse skull gradually becoming transparent and eventually disappearing. 
IMPORTANT: For the volume property action of the Animator to interpolate properly, the Start and End volume properties have identical number of control points on the Scalar Opacity Map curve. Otherwise, interpolation will fail. The position, opacity values, color assignments of the control points can be modified, as long as there are identical number of control points in Start and End VPs. That's why it is easier to define the initial VP carefully, save and reload/rename it so that you can modify the position, opacity and color of those control points for your End volume property.

### Region of Interest action
Now, you are ready to add Region of Interest (ROI) action, which can be used to gradually slice the specimen in 3D view.

1. Switch to four up view (if you are not already in that). 
2. Go to to the Data module and right click the existing Annotation ROI and choose "Clone". Do it one more time. 
3. Rename the cloned ROIs as Start_ROI and End_ROI (right click -> Rename). 
4. Turn the visibility of for the Start_ROI. Then click the End_ROI and set it to a region that is smaller than the full data extend. Turn off its visibility
5. Switch to Animator module, click "Add Action" and choose "ROI Action". Then hit Edit. Similar to Volume Property action, you will set the Start ROI to Start_ROI, End ROI End_ROI, and Animated ROI to AnnotationROI (or whatever it was called when you cloned it in step #2). Again similar to Volume Property you need to make sure the ROI setting of the Volume Rendering module is set to the node specified in the Animated ROI (in this case AnnotationROI), or otherwise your animation won't work.
6. Hit the play button. You should see your specimen being cut through in the plane you chose, while the volume properties are also being changed. 

At this point you have two different actions happening simultaneously, you can play with the time track of individual actions, so you set the place where they begin and end.  

### Rotation Action


