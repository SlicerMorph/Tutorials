# Generalized Procrustes Analysis (GPA) Module III

This tutorial demonstrates how one can use SlicerMorphR package to import landmark data and the output of the GPA module of SlicerMorph into R.

## Install and load SlicerMorphR
Use `devtools::install_github('SlicerMorph/SlicerMorphR')` to install the stable version

Functions in the SlicerMorphR are:
* `read.markups.fcsv()` : Reads the 3D Slicer Markups file saved in fcsv format
* `read.markups.json()`:  Reads the 3D Slicer Markups file saved in json format.
* `write.markups.fcsv()`: Writes a matrix of 3D LM coordinates in 3D Slicer's Markups format as fcsv. 
* `write.markups.json()`: Writes a matrix of 3D LM coordinates in 3D Slicer's in mrk.json format.

* `parser()` : Reads and parses the analysis.log file as output by the SlicerMorph's GPA module. This log format is deprecated. The function is provided for historical reason only.   
* `parser2()` : Reads and parses the analysis.json file as output by the SlicerMorph's GPA module.  

**Important Note:** Keep in mind that, while markup files can be saved in FCSV format, this is an older format with potential room for data loss, particularly if you are using some of the new features of Markups module in Slicer. For any recent version of Slicer (version >5.6), you really should be using mrk.json format to store your landmarks (aka pointLists) and other types of Markups in Slicer. We (both Slicer and SlicerMorph) will eventually stop supporting fcsv format. 


## Exporting data for Analysis in R
You can get your coordinate data from SlicerMorph into R in two ways using functions from SlicerMorphR:

1. You can use the original (raw) landmark coordinates in a 3D array and execute your own GPA in R. 
2. You can use the SlicerMorphs GPA aligned coordinates directly into your allometric regression model or any other shape analysis.

Provided that you used identical settings across different GPA sofware (e.g., gpagen in geomorph), both options should (and will) give you identical results (within machine precision). Below is an exercise that demonstrates it using one of the sample datasets from SlicerMorph. 

To begin, use the GPA module to run GPA/PCA analysis on the Gorilla skull landmark dataset. Remember, `Gorilla Skull Landmarks Only` is  available through the `Sample Data` module. Click on its icon to download, and unzip the contents to a convenient location (e.g., your desktop folder). You can refer to the instructions in [GPA I: Basics of GPA Fitting](../GPA_1/README.md) for details on how to execute the GPA. Please make a note of the output folder you specified in GPA module. This is where the **analysis.json** file is saved. You will need to enter this into the R later. 


### Directly import original landmark coordinates into R

SlicerMorphR allows reading both fcsv and json format markup files. All you have to do is to provide the path of the coordinate file. 


```
library(SlicerMorphR)

lm = read.markup.json(file = 'path/to/cache/Gorilla_Skull_LMs/USNM174715.mrk.json') # Loads one mrk.json file into variable lm.
# If you have files in the older fcsv format, use the read.markup.fcsv function instead. 

```

This function will save the original landmark coordinates of a single specimen in a 2D matrix called lm. You can write simple for loop function to read all your specimens and construct the 3D coordinate array R libraries like geomorph or morpho expects. 

**NOTE:** If you are using fcsv format, please see the read.markups.fcsv documentation,`?read.markups.fcsv`, for an explanation of forceLPS parameter and when to use it with older fcsv files. This is not relevant for markups saved in json format. 


### Reading the GPA module output file using parser2()
Additionally, we provide another convenience function to parse the log output (**analysis.json** file) from GPA module output. Parser saves everything necessary, including the output files as well as raw coordinates, in a large R object. You can find the function here:

```
library(SlicerMorphR)
logFile = parser2('path/to/analysis.json')
```
This will save `analysis.json` into `logFile` list and will provide following R objects:

Specifically, it provides:
  * $input.path = Unix style path to input folder with landmark files.
  * $output.path = Unix stype path to the output folder created by GPA
  * $files = files included in the analysis
  * $format = format of landmark files ("fcsv" or "mrk.json")
  * $no.LM = number of landmarks original
  * $skipped = If any landmark is omitted in GPA (TRUE/FALSE) 
  * $skippedLM = Indices of skipped LMs (created only if $skipped=TRUE)
  * $Boas = is PCA conducted on procrustes coordinates rescaled by size (TRUE/FALSE)
  * $MeanShape = filename that contains mean shape coordinates calculated by GPA (csv format)
  * $eigenvalues = filename that contains eigenvalues as calculated by PCA in the SlicerMorph GPA (csv format)
  * $eigenvectors = filename that contains eigenvectors as calculated by PCA in the SlicerMorph GPA (csv format)
  * $OutputData = filename that contains procrustes distances, centroid sizes and procrustes aligned coordinates as calculated by the SlicerMorph GPA (csv format)
  * $pcScores = filename that contains individal PC scores of specimens as calculated by PCA in the SlicerMorph GPA (csv format)
  * $ID = list of specimen identifiers (obtained from file names)
  * $LM = 3D landmark array that contains the 3D raw coordinates as inputed to the SlicerMorph GPA module. 
  * $semi = If any landmarks are tagged as semi-landmarks (TRUE/FALSE)
  * $semiLM = indices of LMs tagged as semi-landmarks (created only if $semi==TRUE)
  * $CovariatesFile = the covariates passed to the GPA module at the time of analysis in csv format.
  
in this example, logFile$LM object is what you would pass to R libraries like geomorph and morpho, if you would like to execute our own GPA superimposition and it is equivalent to the 3D coordinate array you would construct from reading the individual files one by one. 

**NOTE:** In the older version of SlicerMorph's GPA module, the output is written as simple text file called analysis.log. If you still have one of those, and would like to import that into R, use the older parser() function instead. 

## Example R analysis
Now, we will go through an example that will use the parser2() function above to import data into R and fit an allometric regression model to both raw coordinates as well and SlicerMorph's GPA aligned procrustes coordinates. To do the next steps in tutorial, you have to complete these two steps:

[Continue following the instructions in the tutorial.](./parser_and_sample_R_analysis.md) 

