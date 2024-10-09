# Script Editor

## Slicer and Python
You can use the internal python console of  Slicer to:
  * Automate things (run the same things over a number of specimens)
  * Implement customization (e.g., assign certain keys as shortcuts to your favorite tools)
  * Extend the functionality of Slicer by implementing new 
  * Document the workflow for posterity
  
A good place to start with simple scripting and programming in Slicer is the [Slicer Script Repository](https://slicer.readthedocs.io/en/latest/developer_guide/script_repository.html), where you can find code examples for specific modules, as well as examples of how to put together functions to do complex tasks. From here you can copy the code and paste into the Slicer's python console directly to execute. However, to apply these examples to your data or project, you often need to change the file path, file names or variables, which can be done using any text editor. However, Python is pretty strict with the intentations and how whitespaces (space or tab) is used to nest functions or loops. In such cases it is preferable to use a coding editor, which typical offer additional benefits such as color highlighting or even auto-completion. There are many out there that you can explore. The primary issue with using a separate application, you end up having a file (your python script) independent of your data in Slicer.

Slicer's ScriptEditor provides a such simple programming editor inside the Slicer application. Your scripts end up objects inside the Slicer scene, much like your volumes, models or markups. You can save them with your Slicer scene or package them along with the data using MRB format of the Slicer. You can even use it to make short texts or readmes for your future self. ScriptEditor is a separatee extension. 

## Getting Started

For proper functionality, you need a version of Slicer newer than October 2024 or revision 33050 or higher. After you make sure you have the right revision number, go to the extension manager, search for ScriptEditor and install the extension. Restart Slicer for changes to take affect. Once Slicer is restarted search for ScriptEditor (CTRL/CMD +F). When switch to the ScriptEditor the first time it is going to install a few dependencies it will. So it may take a minute for your module pane to get activated.

```Python
import SampleData
import sitkUtils
import SimpleITK as sitk

# Load sample volume
sampleVolume = SampleData.SampleDataLogic().downloadMRHead()

# Apply Gaussian smoothing
inputImage = sitkUtils.PullVolumeFromSlicer(sampleVolume)
smoothedImage = sitk.SmoothingRecursiveGaussian(inputImage, sigma=2.0)

# Push the result back to Slicer
smoothedVolume = sitkUtils.PushVolumeToSlicer(smoothedImage, name='SmoothedVolume')
slicer.util.setSliceViewerLayers(background=smoothedVolume) 
```