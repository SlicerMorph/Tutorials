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

For proper functionality, you need a Slicer revision 33050 or higher (October 2024 or later). After you make sure you have the right revision number, go to the extension manager, search for **ScriptEditor** and install the extension. Restart Slicer for changes to take affect. Once Slicer is restarted search for ScriptEditor (CTRL/CMD +F). When initialized first time , ScriptEditor is going to install a few dependencies. So it may take a minute for your module pane to get activated.

Once the ScriptEditor is activated it should look like this:

<img src="./editor1.png">

Go ahead and click on the **Script Node** selector and choose **Create New Text as** and then name the object as **MyScript**

Then copy and paste the following code into the editor:

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
<img src='./editor2.png'>

You can expand the width of the editor window, by dragging the module pane to the right.  To save this script into the into the Slicer scene tree (Data module) click 'Save' button. (You will use the regular Slicer Save as or Export File As functionality to save the file to disk). 

You can highlight any section of the code by clicking the left mouse button and dragging the text, then use the right mouse click to bring the context menu (e.g., choose Copy) and then paste it into the Python console. If your python console is not visible in the application window, you can make it visible by pressing `CTRL+3` (on macs `CMD+3`). 

To copy the entire code press the `Copy` button on the top of the Editor

<img src="./editor3.png">
 
When you paste the full code into the python console, it should download the MRHead sample file, and apply a smoothing filter to the image. Your Slicer window should look like this:

<img src="./editor4.png">

To save the MyScript python script onto the disk, go to `Data` module, and right-click on MyScript object (it should have the Python icon to distinguish it from other text objects). Then, in the pop-up dialog box, chose the directory to be saved, and set the Export format to **Python Script (.py)** from the default .txt. 

<img src="./editor5.png">


