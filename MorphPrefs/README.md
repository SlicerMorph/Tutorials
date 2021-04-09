## SlicerMorph Preferences

SlicerMorph preferences provides a convenient way for users to opt-in SlicerMorph specific customizations. Additional user specific customizations can be incorporated by editing the provided **SlicerMorphRC.py** file. For more information about how to customize your slicer via start up scripts, see the [Slicer Script Repository](https://www.slicer.org/wiki/Documentation/Nightly/ScriptRepository).

Navigate to **Edit->Application Settings->SlicerMorph** tab and chech the box _Use SlicerMorph Customizations._ Then, click the **Load now** button next to the Customization file location. Restart Slicer for changes to take effect. 

**Download Directory:** Overwrites the **TEMP** (Edit->Application Settings->Modules) and **CACHE** (Edit->Application Settings->Cache) settings of Slicer to be the specified location. All files downloaded via the `SampleData` module can be found in this folder. 

If you want to revert to Slicer's default Application Settings, uncheck the _Use SlicerMorph Customizations_ button and then hit "Restore Defaults" button and restart your Slicer session. 

<img src="./application_settings.png"> 

### List of Keyboard Shortcuts provided by SlicerMorph Customization

* **p:** Place a fiducial at the voxel under the mouse icon (both in 2D and 3D)
* **`:** Move the active segment effect to right (Segment Editor) 
* **~:** Move the active segment effect to the left (Segment Editor)
* **b:** Change the layout to be Red Slice View Only
* **n:** Change the layout to be Yellow Slice View Only
* **m:** Change the layout to be Green Slice View Only
* **,:** Change the layout to be Four Up View 
* **t:**  Toggle Persistent Mode on/off for Markups 
* **l:** Toggle Markups Lock on/off    

### List Other Customizations applied by SlicerMorph
* Make Data module the default home module (instead of Welcome)
* Volume Interpolation disabled (Volumes)
* Default Volume Rendering methods set to GPU Raycasting (Volume Rendering)
* Default Volume Rendering quality set to Normal (Volume Rendering)
* Orthographic projection is used for 3D rendering (3D viewer)
* Rulers are enabled for 2D and 3D viewers
* Data Probe tab collapse
* Volume and segmentation files are saved uncompressed by default (user can choose to compress via Save As)
* Default 3D model save format is set to PLY.
* Allow overlap between segments during segmentation (Segment Editor)


