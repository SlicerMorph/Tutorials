# HiResScreenCapture Module for SlicerMorph
![HRSCScreenCap.png](HRSCScreenCap.png)
This module is a part of the SlicerMorph extension for 3D Slicer, designed to capture high-resolution screenshots of 3D views within the application. This module is only available for Slicer versions 5.7 or higher, as it relies on some  changes in core Slicer functionality.

## Usage

1. **Open SlicerMorph:** Launch 3D Slicer and open the SlicerMorph module.

2. **Configure 3D Viewer:** Load the data you would like to render in high-resolution, adjust color, transparency and all other visual properties (i.e, hide the bounding box, or orientation labels, decide to present the scale bar or not) and the orientation you would like to capture. 
   
3. Find the **HiResScreenCapture** from the Module selector `SlicerMorph->Utilities->HiResScreenCapture` or use CTRL/CMD+F and search for it.

4. **Configure Settings:**
   - ***Output File**: Specify the path to save the screenshot. You can type the path directly or click `Select Output File` to browse.
   - ***Resolution Scaling Factor**: Use the spin box to set the scaling factor. The range is from 0.1 to 100.0. 
   - ***3D Viewer Size**: Displays the current X and Y dimensions of the 3D viewer. You can modify these by changing the Slicer application window size, maximizing the 3D viewer pane, or adjust its height.  
   - ***Screenshot Output Resolution**: Displays the resolution of the screenshot based on the scaling factor and the 3D viewer Size. For example, if you would like the rendered image to be 10" wide with 600DPI, you need to have the X dimension of the Screenshot Output Resolution to be 6000px. Use the Resolution Scaling Factor and the 3D Viewer size to get to your requested output dimensions.  
 
5. **Capture Screenshot:** Click `Export Screenshot` to capture the screenshot with the specified settings.

6. **Example Output:**

[Molar, 14530x12560 px](https://js2.jetstream-cloud.org:8001/swift/v1/SampleData/Rendering/molar.png)

[Mouse Turbinates, 12200x12928 px](https://js2.jetstream-cloud.org:8001/swift/v1/SampleData/Rendering/Turbinates.png)

7. **Known Issue:** HiResScreenCapture only works with single 3D layouts. Also occassionally dimensions of the output file might be slightly different than displayed value. 