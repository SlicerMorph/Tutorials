# ANTsPy-VIII: Troubleshooting and advanced topics

Previous: [ANTsPy-VII: Pair-wise tab, registering one image to another](Pairwise.md)

---

## Troubleshooting

### Issue: "Number of images doesn't match number of landmarks"

**Cause:** Mismatch between selected image files and landmark files.

**Solution:**
1. Check the "View input image paths" and "View landmark file paths" lists
2. Ensure you have the same number of files in each list (e.g., 30 and 30)
3. Verify the order matches - first image corresponds to first landmark file
4. Check basenames match (e.g., `C57BL6_J_.nii.gz` ↔ `C57BL6_J_.mrk.json`)

### Issue: "Landmark file mismatch: Missing matching file for X"

**Cause:** Basename mismatch between volumes and landmarks.

**Solution:**
1. Check the error message - it tells you which file is missing a match
2. Verify landmark file exists with matching basename
3. Check for typos in filenames
4. Ensure consistent naming (underscores, capitalization, etc.)

### Issue: Template building runs but produces poor results

**Possible causes:**
- Initial alignment failed (try checking landmark-based initialization)
- Transform type too restrictive (try SyN instead of Rigid)
- Too few iterations (increase to 3-4)
- Image quality issues

**Solution:**
1. Verify landmarks are correctly placed on all specimens
2. Check that initial rigid alignment worked (use Python console output)
3. Try increasing iterations
4. Visualize intermediate templates in the output directory

### Issue: Registration is very slow

**Causes:**
- Large image dimensions
- High-resolution scans
- Limited CPU resources

**Solutions:**
1. Reduce image resolution: Use **Crop Volume** or **Resample Scalar Volume** modules
2. Close other applications to free up RAM
3. Process fewer specimens initially to test settings
4. Consider using a faster transform type (e.g., Affine before SyN)

### Issue: Jacobian analysis fails or produces unexpected results

**Causes:**
- Incorrect transform file format
- Missing files
- Wrong filename pattern

**Solutions:**
1. Verify all `*-0forwardWarp.nii.gz` files exist in the directory
2. Check the filename pattern matches your files (`forwardWarp.nii.gz`)
3. Ensure you used "Separate files" output (not "Composite") during registration
4. Ensure covariates CSV has correct specimen names (match basenames)
5. Load cache file if previously completed successfully

### Issue: Effect image is empty (all 0)

**Causes:**
- No voxel is significant: expected when the groups do not differ, as with the random grouping of this tutorial after FDR correction
- Analysis hasn't run successfully

**Solutions:**
1. Check the number of significant voxels reported in the status bar after **Generate Output Images**
2. Check if analysis completed (look for cache file)
3. Verify group assignments in CSV are correct
4. Check sample size is adequate (need at least 3-5 per group)
5. Uncheck **Apply FDR correction** to see the uncorrected result, but do not interpret it: see [Why the FDR Correction Matters](Analysis.md#why-the-fdr-correction-matters)

### Issue: "Registration failed with error code 1" when using an existing initial transform

**Cause:** ANTs could not read the transform it was given to start from. Earlier versions of the module wrote every transform node as an ITK `.h5` file, including thin plate spline nodes, whose type the ITK build inside ANTs does not recognize. ANTs reports only the exit code, not the reason.

**Solution:**
1. Update the extension - non-linear transforms are now converted to a displacement field automatically, and an unreadable transform is reported with the actual ITK error instead of an exit code
2. If a transform is still refused, the error message names the file and the reason; check that the selected node still holds a transform (**Data** module)
3. Manual workaround on an older version: **Transforms** module → **Convert to grid transform** using the fixed image as reference, then select the resulting node as the initial transform

### Issue: An existing initial transform aligns the specimens less than expected

**Cause:** A transform node that has a parent carries only part of the alignment on its own. Selecting a node in the middle of a chain initializes the registration with that link alone.

**Solution:**
1. Open the **Data** module and find the transform chain
2. Select the last node of the chain, which carries the complete alignment - for FastModelAlign output that is `<name>_deformable`
3. See [ANTsPy-VII: Pair-wise tab](Pairwise.md#using-an-existing-transform-as-the-initial-transform)

### Issue: "Transform could not be loaded" error

**Cause:** Incompatible transform file format or corrupted file.

**Solution:**
1. Check that registration completed successfully
2. Verify warp files (`*-0forwardWarp.nii.gz`) are not corrupted (check file sizes > 0)
3. Ensure you selected "Separate files" not "Composite" during group registration
4. Re-run group registration if needed
5. Ensure you're using the correct ANTsPy version

---

## Advanced Topics

### A. Refining Your Template Mask

You can create additional masks for region-specific analysis:

**Example: Neurocranium only (exclude facial bones)**

1. Load the template in Slicer
2. Go to **Segment Editor** module
3. Load your existing skull mask
4. Use **Scissors** effect to remove facial region
5. Export as `Template_Mask_Neurocranium-label.nrrd`
6. Use this mask in Jacobian analysis to focus on braincase only

**Example: Left vs Right side analysis**

1. Create two masks by splitting the existing mask at midline
2. Use **Scissors** with "Erase outside" to keep only one side
3. Export as `Template_Mask_Left-label.nrrd` and `Template_Mask_Right-label.nrrd`
4. Run separate analyses to compare asymmetry between groups

**Example: Regional masks**

Create masks for specific bones:
- Frontal bone only
- Parietal bones only  
- Zygomatic region only
- Mandible only (exclude cranium)

This allows region-specific hypothesis testing.

### B. Multi-level Covariates

For more complex experimental designs:

```csv
specimen,group,sex,age
C57BL6_J_,A,M,8
BALB_CJ_,A,F,10
DBA_2J_,B,M,9
...
```

Regression formula examples:
```
log_jacobian ~ group + sex + age
log_jacobian ~ group * sex
log_jacobian ~ C(group) + age + sex
```

### C. Batch Processing Scripts

For processing large datasets, consider Python scripting.

#### Batch Rigid Alignment Script
Below is a script to rigidly align all specimens to the reference. You will need to manipulate some of the parameters (such as volumesDir) to match paths on your system. The easist way to edit and use these scripts, is installing the `ScriptEditor` extension to Slicer and copy and pasting the code into it. Then you can change things directly in the Slicer and directly execute the code with a simple keystrokes (CMD/CTRL+Enter). [See the tutorial for ScriptEditor for more details](https://github.com/SlicerMorph/Tutorials/blob/main/ScriptEditor/README.MD)

```python
import os
import slicer

# Configuration
volumesDir = "/path/to/ANTsamples/volumes"
landmarksDir = "/path/to/ANTsamples/LMs"
outputDir = "/path/to/ANTsamples/RigidAligned"
referenceVolume = "NZBWF1_J_reoriented"
referenceLandmarks = "NZBWF1_J_reoriented_landmarks"

# Get list of volume files
volumeFiles = [f for f in os.listdir(volumesDir) if f.endswith('.nii.gz')]

# Get module logic
logic = slicer.modules.antspyregistration.widgetRepresentation().self().logic

# Get reference nodes
fixedVolume = slicer.util.getNode(referenceVolume)
fixedLandmarks = slicer.util.getNode(referenceLandmarks)

for volFile in volumeFiles:
    basename = os.path.splitext(os.path.splitext(volFile)[0])[0]
    
    # Skip reference
    if basename == "NZBWF1_J_reoriented":
        continue
    
    print(f"Processing {basename}...")
    
    # Load moving volume
    movingVolPath = os.path.join(volumesDir, volFile)
    movingVolume = slicer.util.loadVolume(movingVolPath)
    
    # Load moving landmarks
    landmarkFile = basename + ".mrk.json"
    landmarkPath = os.path.join(landmarksDir, landmarkFile)
    movingLandmarks = slicer.util.loadMarkups(landmarkPath)
    
    # Create output nodes
    outputTransform = slicer.mrmlScene.AddNewNodeByClass('vtkMRMLTransformNode', 
                                                          f'{basename}_rigidTransform')
    outputVolume = slicer.mrmlScene.AddNewNodeByClass('vtkMRMLScalarVolumeNode',
                                                       f'{basename}_rigidlyAligned')
    
    # Set up registration parameters
    # (This is pseudo-code - actual API may differ)
    params = {
        'fixedVolume': fixedVolume.GetID(),
        'movingVolume': movingVolume.GetID(),
        'outputTransform': outputTransform.GetID(),
        'outputVolume': outputVolume.GetID(),
        'transformType': 'Rigid',
        'useInitialTransform': True,
        'initialTransformType': 'landmarks',
        'fixedLandmarks': fixedLandmarks.GetID(),
        'movingLandmarks': movingLandmarks.GetID()
    }
    
    # Run registration
    # logic.runRegistration(params)  # Actual method depends on module implementation
    
    # Save outputs
    outputVolPath = os.path.join(outputDir, f'{basename}_rigidlyAligned.nii.gz')
    slicer.util.saveNode(outputVolume, outputVolPath)
    
    # Clean up
    slicer.mrmlScene.RemoveNode(movingVolume)
    slicer.mrmlScene.RemoveNode(movingLandmarks)
    
    print(f"  Saved: {outputVolPath}")

print("Batch processing complete!")
```

**Note:** The exact API for programmatic ANTs registration may vary. Check the module documentation or examine the module's Python code for the correct method signatures.

### D. Exporting Results for External Analysis

You can export Jacobian maps for analysis in R, Python, or FSL:

1. Save the effect image as NIfTI (.nii.gz)
2. Extract voxel values using **Quantification** modules
3. Export to CSV for statistical software
4. Use template mask to extract ROI values

### E. Visualization with Volume Rendering

Create publication-quality 3D visualizations:

1. **Volume Rendering** module
2. Select the effect image
3. Adjust preset: "CT-AAA" or create custom
4. Modify color/opacity transfer functions
5. Use **ROI** to crop display
6. Adjust view angle and lighting
7. **Screen Capture** for figures

### F. Validating Registration Accuracy

Use landmarks to compute registration error:

```python
import numpy as np

# Get fixed and moving landmarks (after registration)
fixedPoints = slicer.util.arrayFromMarkupsControlPoints(fixedMarkups)
movingPoints = slicer.util.arrayFromMarkupsControlPoints(movingMarkups)

# Compute Euclidean distances
errors = np.sqrt(np.sum((fixedPoints - movingPoints)**2, axis=1))

print(f"Mean error: {np.mean(errors):.2f} mm")
print(f"Max error: {np.max(errors):.2f} mm")
```

### G. Alternative Registration Strategies

**For different biological questions:**

- **Rigid only:** For specimens with same shape, different orientation
- **Affine:** For size normalization without shape changes  
- **SyNRA:** Rigid + Affine + SyN (comprehensive)
- **SyNCC:** SyN with cross-correlation metric (for similar intensities)

**Landmark-based registration only:**
- Use **Fiducial Registration Wizard** module for TPS or affine
- Faster but less detailed than image-based

---

Previous: [ANTsPy-VII: Pair-wise tab, registering one image to another](Pairwise.md)
