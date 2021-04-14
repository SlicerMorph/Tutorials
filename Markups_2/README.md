## Resampling Curves
This tutorial explains how to resample a curve using the `Markups` module. In this tutorial we use the Gorilla Skull Reference Model from the SlicerMorph tab of the `Sample Data` module (you will need the SlicerMorph extention installed to see this option in the menu).

----

### Resampling a curve on the surface of a model to create Semi Landmarks

1. Create a new open curve on the surface of your mesh (blue points and cuve in the example below) using the `Markups` module. 
  * Note that the control points are snapped to the mesh, but the curve itself may lie above or below the mesh surface. 
<img src="./images/curveOnMesh.png"/>

2. In `Markups` module, select the open curve node that you just created and expand the **Resample** Menu. 
  * Select *Create a new markups curve* from the Output node dropdown menu and choose the number of resampled points (in this example we use 50). 
  * In the *Constrain points to surface* dropdown menu, select the mesh you are using to place landmarks. 
  * Before resampling, confirm that the curve to be resampled is selected as the active node in the node list from `Markups` module. 
<img src="./images/resampleOptions.png"/>

3. Click the **Resample curve** button to generate a new open curve with 50 points constrained to the mesh surface. This results in a curve that is closer to the actual surface curvature than the original. 
<img src="./images/newCurve.png"/>

4. Check the lengths of new curve and the original curve using Measurements menu.
<img src="./images/measures.png" width="600px"/>
