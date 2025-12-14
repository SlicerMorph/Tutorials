# The `FastModelAlign` module

The `FastModelAlign` module in SlicerMorph is for 3D model registration using a unified workflow approach. It performs scaling, rigid (RANSAC and ICP), affine, and deformable registration steps. Users can select any combination of registration steps via checkboxes to create custom registration pipelines. The rigid registration involves RANSAC and ICP steps from downsampled pointclouds derived from 3D models. Affine and deformable registrations use Coherent Point Drift (CPD) algorithms. [For more technical implementation details, see the related ALPACA automated landmarking publication](https://doi.org/10.1111/2041-210X.13689). 

## 1. Download sample data
We provided two partial models of the same mountain beaver skull derived from two separate photogrammetric runs as sample data: `partial_photogram_model1.obj` is a partial skull with no bottom, and `partial_photogram_model2.obj` is a partial skull with no top. After installing the SlicerMorph extension, they can be downloaded from the `Sample data module`: clicking the `partial photogrammetry models` button under the `FastModelAlign` tab will download the two sample models to the Slicer cache folder and load them into Slicer automatically (also downloadable here: [SlicerMorph sample data](https://github.com/SlicerMorph/SampleData). The following tutorial are based on these two sample models.

<p align="center">
<img src="images/Figure_2.png" width = 500>
</p>

## 2. Specify source, target, and output models
If using the `Sample Data` module, the two partial models should be loaded into Slicer automatically. If downloaded manually, drag and drop the downloaded sample models into 3D Slicer. The two models have distinct sizes because no size reference was provided for the two photogrammetric runs. 

Switch to the `FastModelAlign` module. Select `partial_photogram_model2` from the `Source Model` drop-down menu and `partial_photogram_model1` from the `Target Model` drop-down menu. Also create a model from the `Output registered model` drop-down menu. In this tutorial, the output registered model is named as `test_registration`. 

### Select Registration Steps
The module now uses a unified workflow with four registration step options:
- **Scaling**: Scale the source model to approximate the size of the target model
- **Rigid**: Apply rigid registration (rotation and translation) using RANSAC and ICP
- **Affine**: Apply affine transformation using CPD (allows shearing and non-uniform scaling)
- **Deformaegistration
After the source and target models are selected and at least one registration step is checked, the `Testing pointcloud subsampling` and `Run Registration` buttons are enabled. 

<p align="center">
<img src="images/Figure_4.png" width = 500>
</p>


(Optional) Click `Testing pointcloud subsampling` button to preview the density of the downsampled source and target point clouds. Adjust the `Point density adjustment` slider to control the number of points in each point cloud. Usually, 4,000 to 6,000 points per point cloud would be optimal.

<p align="center">
<img src="images/Figure_5.png" width = 500>
</p>

Click the `Run Registration` button to execute the selected registration steps in sequence. For example, if both `Scaling` and `Rigid` are checked, the process first scales the source model to approximate the size of the target model, then rigidly registers the scaled source to the target. After the process is finished, the user-specified output model `test_registration` (red) is generated and represents the source model transformed according to the selected registration steps.


(Optional) Click `Testing pointcloud subsampling` button can show the density of the downsampled source and target point clouds. Adjusting the `Point density adjustment` bar to adjust the number of points in each point cloud. Usually, 4,000 to 6,000 points per point cloud would be optimal.

<p align="center">
<img src="images/Figure_5.png" width = 500>
</p>

Click the `Run rigid registration` button to rigidly register the source `partial_photogram_model2` to the target model `partial_photogram_model1`. If `Skip scaling` is unchecked, the process first scale the source model to approximate the size of the target model, then rigidly registered the source to the target.After the process is finished, the user specified output model `test_registration` (red) is generated and represents the source model rigidly registered to the target model (yellow). 

<p align="center">
<img src="images/Figure_6.png" width = 900>
</p>


## 4. Investigate and visualize the transformation process
After running registration, the module generates transform nodes based on the selected steps:
- **Scaling transform**: Named as `[source_model_name]_scaling` (if scaling was selected)
- **Rigid transform**: Named as `[source_model_name]_rigid` (if rigid was selected)
- **Affine transform**: Named as `[source_model_name]_affine` (if affine was selected)
- **Deformable transform**: Named as `[source_model_name]_deformable` (if deformable was selected, and fast option is unchecked)

The transform nodes are organized hierarchically according to the registration sequence. For example:
- If **Scaling** and **Rigid** are selected: The scaling node is nested under the rigid transformation node
- If **Scaling**, **Rigid**, and **Affine** are selected: The scaling node is under the rigid node, which is under the affine node
- If **Deformable** is included: All previous transforms are nested under the deformable transform node

This hierarchical structure ensures transforms are applied in the correct sequence: Scaling → Rigid → Affine → Deformable.

<p align="center">
<img src="images/Figure_8.png" width = 600>
</p>

To repeat and visualize the transformation process, users can drag the original source model `partial_photogram_model2.obj` (blue) to the appropriate transform node in the Transform hierarchy.

<p align="center">
<img src="images/Figure_9.png" width = 900>
</p>

Switch to the Transform hierarchy tab to visualize the hierarchical transformation of the source model node through the selected registration steps.egistration node.

<p align="center">
<img src="images/Figure_9.png" width = 900>
</p>

Switch to the Transform hierarchy, users can visualize the hierarchical transformation of the source model node, which is first scaled by the scaling node, then rigidly registered by the rigid transformation node. 
Affine registration
When the **Affine** checkbox is selected, an affine transformation is applied using Coherent Point Drift (CPD) algorithm. The affine transformation allows for shearing and non-uniform scaling in addition to rotation and translation. This step is useful when the source and target models have different proportions or slight distortions.

The affine transformation is applied to the previously registered model (after any selected scaling and/or rigid steps) and directly modifies the model vertices. An affine transform node is created for reference and to maintain the transform hierarchy
<img src="images/Figure_10.png" width = 500>
</p>

## 5. Run affine registration
After running a rigid transformation, the `Run affine registration` button will be enabled. Click this button will perform an affine deformable transformation for the scaled (if `Scaling optin` is left unchecked) and rigidly registered source model. Therefore, the user defined output model (`test_registration` (red)) is then undergone an affine transformation to match with the target (yellow).

<p align="center">
<img src="images/Figure_11.png" width = 400>
</p>
<p align="center">
<img src="images/Figure_12.png" width = 500>
</p>Deformable registration
When the **Deformable** checkbox is selected, non-rigid deformable registration is applied using Coherent Point Drift (CPD) algorithm. This allows for local, non-linear deformations to match the source model to the target model more precisely. The deformable registration offers two modes:

### Fast Mode (Direct Vertex Deformation)
When **Fast Mode** is enabled, the CPD deformation field is applied directly to the model vertices using Radial Basis Function (RBF) interpolation with thin plate spline kernel. This mode is faster and produces a deformed model node named `[source_model_name]_deformed` (displayed in green). The deformation is hardened into the model vertices, and no transform node is created.

### Grid Transform Mode (default)
When **Fast Mode** is disabled, a grid transform is created that defines a smooth deformation field. This mode:
- Creates a vtkMRMLGridTransformNode named `[source_model_name]_deformable`
- Produces a hardened deformed model named `[source_model_name]_deformed`
- The grid resolution is controlled by the `Grid spacing` slider in Advanced Settings (1 = finest ~256³ grid, 10 = coarsest ~64³ grid)
- Uses RBF interpolation to create a smooth displacement field on a regular grid

The deformed model (green) represents the final registered result with local shape variations matching the target (yellow).

### Deformable Registration Parameters
Key parameters for deformable registration (adjustable in Advanced Settings):
- **Alpha**: Controls the smoothness of the deformation (higher = smoother, default: 2.0)
- **Beta**: Controls the regularization of the deformation (higher = less deformation, default: 2.0)
- **CPD Iterations**: Maximum iterations for CPD optimization (default: 100)
- **CPD Tolerance**: Convergence tolerance for CPD (default: 0.001)
- **Grid Spacing**: Controls grid density for grid transform mode (1-10, only for grid mode)

<p align="left">
<img src="images/Figure_13_1.png" width = 500> <img src="images/Figure_13.png" width = 500>
</p>

To repeat and visualize the full registration sequence, users can drag the original untransformed source model `partial_photogram_model2.obj` (blue) into the appropriate transform node for the complete sequence of transformations

To repeat and visualize the affine registration, users can drag the original untransformed source model `partial_photogram_model2.obj` (blue) into the scaling node `partial_photogram_model2_scaling` for a full sequence of scaling, rigid registration, and affine transformation.

<p align="left">
<img src="images/Figure_14.png" width = 600> <img src="images/Figure_15.png" width = 400>
</p>


## 7. Parameter settings
Users can adjust the parameter settings under the `Advanced Settings` tab before running registration. The parameters are organized by registration type:

### Point Cloud Subsampling Parameters
- **Point Density**: Controls the density of subsampled point clouds (default: 1.0)
- **Poisson Subsample**: If checked, uses Poisson disk sampling for more uniform point distribution
- **Normal Search Radius**: Radius for normal estimation (default: 2.0)

### Rigid Registration Parameters
For detailed information about rigid registration parameters, please refer to the [ALPACA publication](https://doi.org/10.1111/2041-210X.13689).
- **FPFH Search Radius**: Search radius for Fast Point Feature Histogram (FPFH) features (default: 5.0)
- **FPFH Neighbors**: Number of neighbors for FPFH computation (default: 100)
- **Maximum CPD Threshold**: Distance threshold for correspondence rejection (default: 3.0)
- **Max RANSAC Iterations**: Maximum RANSAC iterations (default: 1,000,000)
- **ICP Distance Threshold**: Distance threshold for Iterative Closest Point refinement (default: 1.5)

### Deformable Registration Parameters
- **Alpha**: Smoothness parameter for CPD deformation (default: 2.0)
  - Higher values produce smoother deformations
  - Lower values allow more local variation
- **Beta**: Regularization parameter for CPD (default: 2.0)
  - Higher values reduce the amount of deformation
  - Lower values allow more aggressive deformation
- **CPD Iterations**: Maximum iterations for CPD optimization (default: 100)
- **CPD Tolerance**: Convergence tolerance for stopping criterion (default: 0.001)
- **Grid Spacing**: Controls grid resolution for grid transform mode (1-10)
  - 1 = finest resolution (~256 grid points per dimension)
  - 10 = coarsest resolution (~64 grid points per dimension)
  - Only applicable when Fast Mode is disabled
- **Fast Mode**: When enabled, applies deformation directly to vertices (faster, no grid transform created)

<p align="center">
<img src="images/Figure_16.png" width = 400>
</p>
