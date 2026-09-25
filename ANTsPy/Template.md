# ANTsPy-IV: Template tab, building a population template

Previous: [ANTsPy-III: Average tab, an average reference volume](Average.md) | Next: [ANTsPy-V: Group-wise tab, registering all specimens to the template](Groupwise_template.md)

---

**Overview:** Template building creates an average shape representing your population using deformable registration. We'll use the original unaligned volumes as the input volumes, and use the landmarks to bring them into alignment with the reference, then run **2 iterations** of template refinement. At the end of the first step module will calculate a new template, and start the second iteration using that template as the reference. 

**TO DO: Explain why we are not using the output of our rigid registration (-transformed volumes) as input to this procedure. Effects of doing image resampling multiple times etc...

## Open the ANTsPyRegistration Module

1. In the module search bar (top left), type "ANTsPy"
2. Select **ANTsPyRegistration**
3. Go to the **Template** tab

## Configure Template Building Settings

### A. Initial Template

- **Initial Template:** Select `RigidAverage_Template`
  - This is the averaged volume we created from rigidly aligned specimens
  - Using this provides a better starting point than a specific sample.
  - Reduces computational time and improves convergence
  
### B. Transform Type

- **Transform Type:** Select `SyN` (Symmetric Normalization)
  - This is a deformable registration that can capture shape differences
  
### C. Number of Iterations

- **Iterations:** Set to `2`
  - Each iteration refines the template
  - More iterations = better template but longer computation time
  - 2-3 iterations are typically sufficient

## Select Input Images

1. Click **"Select input images..."**
2. A file browser opens
3. Navigate to `ANTsamples/volumes/`
4. Select **all 30 .nii.gz files**:
   - Click the first file
   - Scroll down, hold Shift, click the last file
   - Or use Ctrl+A (Cmd+A on Mac) to select all
5. Click **Open**

**View Input Image Paths:**
- Click the **"View input image paths"** collapsible button
- You should see all 30 file paths listed
- Verify the order is correct
- Use **"Remove Selected Path"** or **"Move Path to Top"** if you need to adjust
- **"Clear input path list"** removes all files

## Configure Landmark-based Initial Transform

This ensures specimens are aligned to the reference before template building begins.

1. Check ☑ **"Compute landmark-based RIGID initial transform"**

2. Click **"Select landmark files..."**

3. Navigate to `ANTsamples/LMs/`

4. Select **all 30 .mrk.json files** (same order as volumes)
   - The order must match your volume files!
   - First volume → first landmark file, etc.

5. Click **Open**

**Verify landmark selection:**
- Click **"View landmark file paths"** to expand
- Check that you have 30 landmark files
- Ensure the basenames match the volume files
  - Example: `C57BL6_J_.nii.gz` ↔ `C57BL6_J_.mrk.json`

**Important Note:** The module will validate this for you. If there's a mismatch in count or basenames, you'll get a clear error message when you try to run.

6. **Template Landmarks:** Select `RigidAverage_Landmarks`
   - These are the average landmark positions we created
   - Used if you provide an initial template volume
   - Helps with landmark-based initialization

## Configure Output Settings

1. **Output template:**
   - Click the dropdown
   - Select **"Create new Volume"**
   - It will create a node called `Template`
   - You can rename it if desired (e.g., `MouseCranium_Template`)

2. **Output landmarks:**
   - Click the dropdown
   - Select **"Create new MarkupsFiducial"**
   - Creates `Template_Landmarks` node
   - This will contain the average landmark positions

3. **Output Directory:**
   - Click the folder icon
   - Navigate to `ANTsamples/` (or create a new folder like `ANTsamples/TemplateOutput/`)
   - Select the folder
   - This is where all intermediate files and transforms will be saved

## Run Template Building

1. Click **"Run Template Building"**

2. **What happens:**
   - Initial alignment: Each specimen is rigidly aligned to the first specimen using landmarks
   - Iteration 1: All specimens are registered to the specified template, and then the template is updated
   - Iteration 2 (and subsequent iterations): Re-registrat the samples to the updated template
   - Calculate the final template.
   - Output files are saved to the output directory

3. **Progress monitoring:**
   - Button text changes to "Template building in progress"
   - Console output shows registration progress (View → Python Interactor)
   - **Time estimate:** 10-30 minutes depending on your computer
     - Per specimen: ~30 seconds to 2 minutes
     - 30 specimens × 2 iterations = 60 registrations

4. **Completion:**
   - Button returns to "Run Template Building"
   - Template volume appears in the Data module
   - Template landmarks appear in the Markups module

## Visualize the Template

1. In the **Data** module, find your template volume (e.g., `MouseCranium_Template`)
2. Click the eye icon to show it in the viewers
3. In the **Markups** module, select `Template_Landmarks`
4. The template represents the average cranial shape of all 30 mouse strains
5. Review for anatomical detail. You can try increasing the number of iterations. 


<img src="images/06_template.png" width="900">


## Save the Template

1. Right-click the template volume in the Data module
2. Select **"Export to file..."**
3. Save as `MouseCranium_Template.nii.gz` in your project folder

4. Do the same for template landmarks:
   - Right-click `Template_Landmarks`
   - Save as `Template_Landmarks.mrk.json`

---

Previous: [ANTsPy-III: Average tab, an average reference volume](Average.md) | Next: [ANTsPy-V: Group-wise tab, registering all specimens to the template](Groupwise_template.md)
