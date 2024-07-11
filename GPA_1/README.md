# Generalized Procrustes Analysis (GPA) Module I


## Introduction
This tutorial will walk you through using the SlicerMorph's GPA module to run GPA/PCA on a sample dataset. GPA is a complex module with multiple tabs that accomplish different tasks related to geometric morphometrics. This first tutorial will introduce the interface and explain the basics of the setting up the analysis (such data import, analysis options, output directory etc..) and executing the analysis. The other tutorials on GPA will show:

1. How to create visualizations of the PC vectors and variance at the landmark points and use of covariates to plot factors 
2. How to create interactive 3D visualizations and heatmaps of deformed PC models
3. How to interface output from SlicerMorph's GPA module with R. 

### Setup Analysis
1. Download the **Mouse Skull Landmarks** from the `Sample Data` module and unzip its contents somewhere on your Desktop folder. Quickly review the folder contents and note that there are 126 files that contains landmark coordinates (one file per specimen). You can drag'n'drop one of the landmark files into Slicer and see that it contains 55 landmarks (you may need to hit the center field of view button in 3D rendering window). Once you review the of files, hit `CTRL + W` to empty your Slicer scene. We always recommend running the GPA module in an empty Slicer scene to avoid complications that may arise from having other data loaded into the Slcier. 

2. Search for `GPA` module in Module Finder (CTRL +F) or use the Module Selector dropdown menu and navigate to **SlicerMorph->Geometric Morphometrics->GPA**. You layout will switch to a custom layout with two 3D rendering windows, a single slice view, a chart view and a table view. We are ready to choose our study sample. 

3. Click on the `Select Landmarks..` button next to the `Landmark Folder`. In the file browser pop-up, navigate to folder you downloaded in step 1. GPA module supports both the legacy **fcsv** and the new **mrk.json** markup format (although we advise saving all your markups in mrk.json format). Select all the files in the folder. Click the `Open` button to confirm the selection. 

4. In the GPA module, expand the `View selected landmark files` tab. This table will display the full paths of all files that will be included in the analysis. **Note:** If you want to exclude some samples from your analysis, simply repeat the selection process and do not choose the specimen(s) you wish to exclude from the analysis.

5. Now specify the output directory by clicking the `..` button next to it. A time-stamped subfolder will be automatically created inside this folder when you execute the GPA. Each time you execute the GPA a new time-stamped subfolder will be created, so that results aren't overwritten. 

6. Leave the **Use Boas Coordinates for GPA** option unchecked. If checked, this option will reintroduce the size for PCA decomposition by multiplying the procrustes aligned coordinates with the centroid size of the specimen. Otherwise PCA decomposition is done on procrustes aligned coordinates that are scaled to unit size.

<!--- 7. Next tab is called **Load Optional Covariates table** and allows the user to specify additional covariates in the analysis (e.g., sex, genotype, locomotor mode etc.). You need to specify the name(s) of the covariate(s) you want to include with your data. If you have more than one covariate, separate them by a comma (,). After you enter the labels, hit the `Generate Template` button for Slicer to create an empty csv file with the subject ID and factors labels prepopulated. No missing values are allowed in the covariate table.----> 

7. Hit `Execute GPA + PCA`.

<img src="./images/Picture4.png" width="900">

### Viewing the Output Data
8. To view the output from the GPA module, click the `View Output Files` button. This will open the timestamped results subfolder created from this GPA run. Note the analysis.json (analysis.log in older versions) file and multiple CSV files containing the eigenvalues, eigenvectors, mean shape, PC scores, and combined output that contains new procrustes aligned coordinates, centroid sizes and Procrustes distances from the output. If you want to do more specific analysis, these will be files you will import into R/geomorph or other shape analysis packages.

<img src="./images/Picture5.png" width="900">

### Generating Plots and Visualizations
8. At this point your Plot and Table window should populate with a histogram-like bar plot of Procrustes Distances, and a table that indicates specimen names sorted by Procrustes Distance. This is a good time to see some unique features of these two windows (such as interaction, being able to zoom in into a plot etc).

<img src="./images/Picture6.png" width="500">

9. Scroll down to the bottom of the Table view to see that there are four specimen with very very high PD values. The shape of the PD plot also indicates there is something very different about these four specimens. 

10. Likewise PCA Scatter plot options displays a suspiciously high variance (~98%) for PC1. Go ahead and choose PC1 for x-axis, PC2 for y-axis and hit `Scatter Plot` button. 

11. Note that plot window now changed to `PCA Scatter Plot` from Procrustes Distance plot. You can enlarge this plot by selecting the "Plot only" layout from the layouts menu. If you expand the Plot window toolbar, you will see a field called Plot Chart that will let you go back and forth between plots created. Go and try switching back to Procrustes Distance plot and back to PCA scatter plot. (Note that if the default data label size is too small for your screen, you can use the Slicer's `Plots` module to modify the aesthetics of the plot. Also the plot window is interactive, you can zoom in and out using the mouse wheel.) 

<img src="./images/Picture7.png" width="500">

12. Hover over the four data points on the right-hand side of the PC1 axis and note that they are the same four specimens with the highest PD (you will need to zoom in to see them individually). So what is going on?

13. To access further data visualization tools, switch to the `Explore Data` tab of the GPA module. To visualize the PC1 vectors, go to the `Lollipop Plot Options` section and set the Vector One to PC1, then hit the `Lollipop Vector Plot` button. This set will automatically enable landmarks for the estimated mean shape and place eigenvector associated with PC1. By convention, this indicates how mean shape will change along the positive values of the selected PC. You should see that no other vector apart from LM25 is visible, thus PC1 (and almost all shape variation in this dataset) is influenced by LM25. This is a sign off trouble with this dataset. (HINT: If you toggle the `mean shape visibility` on and off, you will be able to see the other vectors.). Interact with the 3D window and note that you can zoom in/out as usual, and rotate. At this point, you can use either Viewport #1 or #2. 

<img src="./images/Picture8.png" width="900">

14. To further convince ourselves of the issue, you can get a sense of variance associated with each landmark by plotting them as either spheres (averaged across three dimensions), or ellipse (all axes are independently calculated). You can see that LM25 has variance orders of magnitude larger than the others, and it is pancake like appearance suggest that it is confined to two dimensions. 

<img src="./images/Picture9.png" width="900">


Plotting the point cloud from the landmark variance shows that four points from the landmark 25 cluster were placed at the origin. Clearly LM25 in these four specimens have an issue, and is better left out of the analysis. 

<img src="./images/Picture10.png" width="500">

### Excluding points from the Analysis
15. To repeat the analysis without LM25, first we need to clear our scene. Hit the `Reset Scene` button at the bottom of the GPA module and note that everything created by this module is removed. 

16. Now return to the  `Setup Analysis` tab of the module and repeat the step #1 above. It should remember the last folder you open. This time you will enter 25 to `exclude landmarks` field to conduct the analysis without LM25. 

17. Review the outputs and confirm that the Procrustes distance bar plot and PC1xPC2 scatter plot have been updated and no longer show the outliers observed in the previous run. 

<img src="./images/Picture11.png" width="500">

Try things like projecting the results in 2D Slice viewer, which you can switch the plane like in normal slice view you have been viewing. Unfortunately, it doesn't automatically center the field of view like scans we have been loading, thus you need to use your `left` and `middle` mouse clicks to zoom in/out and pan the field of view manually. Trying having more than one PC plotted as lollipop plots etc...


For more information on using the GPA tools, continue with the tutorial: [GPA II](../GPA_2/README.md)

### Other Resources
Videos of GPA tool functionality on the SlicerMorph youtube channel. Note that these show slightly older user interface, but the functionality is identical. 
* [Basic Functionality](https://www.youtube.com/watch?v=FCeZ2J5Uvcw)
* [Realtime exploration of MorphoSpace](https://www.youtube.com/watch?v=hMMR9GChek8&t=2s)
* [Recording the PC warps](https://www.youtube.com/watch?v=gtHqhqaKeCU)
