# Generalized Procrustes Analysis (GPA) Module III: Example R Analysis using SlicerMorphR

## Exporting data for Analysis in R using the SlicerMorphR R package
`SlicerMorphR` package contains functions for conveniently importing landmark files in the format of `ply` and `json` as well as the output of the GPA module of `SlicerMorph` extension.

Now, we will go through a R markdown document that will use the `parser()` function in  import data into R and fit an allometric regression model to both raw coordinates as well and GPA aligned procrustes coordinates. To do the next steps in tutorial, you have to complete these two steps:


### Step 1. Download sample data and run the GPA module. 
Use the GPA module to run GPA/PCA analysis on the Gorilla skull landmark dataset `Gorilla Skull Landmarks Only` available in the `Sample Data` module. You can refer to the instructions in [GPA I: Basics of GPA Fitting](../GPA_1/README.md) for details on using this module. The sample data should be stored in `path/to/cache/Gorilla_Skull_LMs`. You can cehck the cache path of Slicer in `Edit/Application Settings/Cache`. Make a note of the output folder you specified. You will need to enter this into the R. 


### Step 2. Install and load SlicerMorph and geomorph R packages.
```
install.packages("geomorph")
library(geomorph)
devtools::install_github('SlicerMorph/SlicerMorphR') #Install SlicerMorphR
library(SlicerMorphR)
```


### Step 3. Point to the location of the analysis.log file that was saved by SlicerMorph's GPA module. 
We can do this either via coding the path OR interactively.

If you are knitting the document, make sure to hard code the path. 
```{r load log file}
# coding the path to log file example 
# SM.log.file="C:\\temp\\RemoteIO\\gorilla_patches\\merged\\2021-04-05_20_51_17\\analysis.log"

# interactively choose log file
SM.log.file <- file.choose()
SM.log <- parser(SM.log.file, forceLPS = TRUE)
```
* The `SM.log.file` contains pointers to all relevant data files. These pointers are parsed by `parser()` and stored in` SM.log`. For details, please see the helper file of `parser()` using `?parser()`.
```
> head(SM.log)
$input.path
[1] "C:/Users/czhan2/Documents/Slicer_download/Gorilla_Skull_LMs"

$output.path
[1] "C:/Users/czhan2/Documents/Slicer_download/2022-05-17_15_11_55"

$files
 [1] "USNM174715.fcsv" "USNM174722.fcsv" "USNM176209.fcsv" "USNM176211.fcsv" "USNM176216.fcsv" "USNM176217.fcsv" "USNM176219.fcsv" "USNM220060.fcsv" "USNM220324.fcsv" "USNM252575.fcsv"
[11] "USNM252577.fcsv" "USNM252578.fcsv" "USNM252580.fcsv" "USNM297857.fcsv" "USNM582726.fcsv" "USNM590942.fcsv" "USNM590947.fcsv" "USNM590951.fcsv" "USNM590953.fcsv" "USNM590954.fcsv"
[21] "USNM599165.fcsv" "USNM599166.fcsv" "USNM599167.fcsv"

$format
[1] ".fcsv"

$no.LM
[1] 41

$skipped
[1] FALSE

```
* Note that a parameter `forceLPS` in parser(). In the older version of 3D Slicer, markup coordinates were written in `RAS` (Right-Anterior-Superior) coordinate system assumption. More recently, it has been saved as `LPS` (Left-Posterior-Superior). 
* If forceLPS set to FALSE (default), the contents of the file will be read as it is, ignoring the header information, where the coordinate system is stored.
* If forceLPS is set to TRUE, the `forceLPS` parameter is then passed to the `read.markups.fcsv()` function (`parser()` uses this function to read fcsv files). The `read.markups.fcsv()` function will check for the coordinate system definition in the FCSV header, and will negate the x,y coordinate values if the coordinate system is defined as RAS. If the coordinate system definition is LPS, file will be read without conversion. In this tutorial, we set up forceLPS = TRUE to make the loaded sample landmark sets consistent with the output of the GPA module.
* For details of the forceLPS parameter, please see the helper file of `read.markups.fcsv `using `?read.markups.fcsv`.


### Step 4. Read the OutputData.csv and pcScores.csv output by GPA module into a data matrix using the pointers stored in SM.log.
```
SM.output <- read.csv(file = paste(SM.log$output.path, 
                                SM.log$OutputData, 
                                sep = "/"))
                                
SlicerMorph.PCs <- read.table(file = paste(SM.log$output.path, 
                                        SM.log$pcScores, 
                                        sep="/"), 
                              sep = ",", header = TRUE, row.names = 1)
```
* `SM.output` stores all the data from `OutputData.csv `output by the GPA module. These data include Procrustes distances to the mean shape, centroid sizes, and GPA aligned landmark coordinates. 
* `SlicerMorph.PCs` stores all PC scores from `pcScores.csv` output by the GPA module 
```
head(SM.output)
head(SM.log)
```


### Step 5. Check the number of landmarks used in the analysis and extract Procrustes distances to the mean shape
```
PD <- SM.output[,2]
if (!SM.log$skipped) {
  no.LM <- SM.log$no.LM 
  } else {
    no.LM <- SM.log$no.LM - length(SM.log$skipped.LM)
  }
PD
no.LM
```
* `SM.log$skipped` stores whether there is landmark skipped. SM.log$skipped.LM stores the number of landmarks skipped. This tutorial is based on skipping no landmarks. 
* `no.LM <- SM.log$no.LM` stores the number of landmarks. Since no landmark is skipped, `no.LM` should print out `41`. 
* Procrustes distances are located at the second column of the `OutputData.csv`, now stored in `PD <- SM.output[,2]`.


### Step 6. Reformat the GPA aligned coords into 3D LM array and apply sample names
```
Coords <- arrayspecs(SM.output[, -c(1:3)], 
                    p = no.LM, 
                    k = 3)

dimnames(Coords) <- list(1:no.LM, 
                        c("x","y","z"),
                        SM.log$ID)
```
* `Coords <- arrayspecs(SM.output[, -c(1:3)], p = no.LM, k = 3)` accessed all GPA aligned landmark coordinates of all specimens and transformed them into a 3D array `Coords`


### Step 7. Running a regression model between GPA aligned coordiantes and centroids output by the GPA module
We first construct a geomorph data frame withe data imported from SlicerMorph and fit a model to SlicerMorph's GPA aligned coordinates and centroid sizes
```
gdf <- geomorph.data.frame(Size = SM.output$centeroid, Coords = Coords)
fit.slicermorph <- procD.lm(Coords ~ Size, data = gdf, print.progress = FALSE)
```
* `SM.output$centeroid` stores the centroid sizes


### Step 8. Read the raw landmark coordinates using the file pointers parsed by `parser()` and stored in `SM.log`, aligns them with `gpagen()`, and applies PCA
```
gpa <- gpagen(SM.log$LM)
pca <- gm.prcomp(gpa$coords)
geomorph.PCs <- pca$x
```
* `parser()` accessed the pointers to the raw landmark file locations in the GPA-output `analysis.log` and stored them in `SM.log$LM`.
* geomorph.PCs stores all PC scores


### Step 9. Build the same allometric regression model based on the coordinates aligned in Step 8 and then compares it to the results in Step 7.
```
gdf2 <- geomorph.data.frame(size = gpa$Csize, coords = gpa$coords)
fit.rawcoords <- procD.lm(coords ~ size, data = gdf2, print.progress = FALSE)
```

Due to arbitrary rotations, we cannot compare procrustes coordinates directly, instead we look centroid sizes, procD, PC scores, and allometric regression model summary. 

### Step 10. Calculate each sample's Procrustes distance to the consensus shape because geomorph does not report each sample's procD to the consensus shape.
```
pd <- function(M, A) sqrt(sum(rowSums((M-A)^2)))
geomorph.PD <- NULL
for (i in 1:length(SM.log$files)) 
  geomorph.PD[i] <- pd(gpa$consensus, gpa$coords[,, i])
```
* `geomorph.PD` stores each sample's PD to the consensu shape based on geomorph GPA aligned coordinates

### Step 11. Now we can start to compare procrustes variables.
**1. Centroid Size**
```
plot(gpa$Csize, SM.output$centeroid, 
     pch = 20, ylab = 'SlicerMorph', 
     xlab = 'geomorph', main = "Centroid Size")
cor(gpa$Csize, SM.output$centeroid)
```
The results should be:
<p align="center">
<img src="images/GPA3_001.png" width = 600>
</p>

```
> cor(gpa$Csize, SM.output$centeroid)
[1] 1
```

**2. Procrustes Distance of sample to their respective mean**
```
plot(geomorph.PD, SM.output[,2], 
     pch = 20, ylab = 'SlicerMorph', 
     xlab = 'geomorph', main = "Procrustes Distance")
cor(geomorph.PD, SM.output[,2])
```
The results should be:
<p align="center">
<img src="images/GPA3_002.png" width = 600>
</p>

```
> cor(geomorph.PD, SM.output[,2])
[1] 0.9999999
```

**3. We only plot the first two PCs but correlations reported up to 10.** Keep in mind that PCA signs are arbitrary. 
```
plot(geomorph.PCs[,1], SlicerMorph.PCs[,1], 
     pch = 20, ylab = 'SlicerMorph', 
     xlab = 'geomorph', main = "PC1 scores")

plot(geomorph.PCs[,2], SlicerMorph.PCs[,2], 
     pch = 20, ylab = 'SlicerMorph', 
     xlab = 'geomorph', main = "PC2 scores")

for (i in 1:10) print(cor(SlicerMorph.PCs[, i], 
                            geomorph.PCs[, i]))
```
The results should be:
<p align="left">
<img src="images/GPA3_003.png" width = 500>
<img src="images/GPA3_004.png" width = 500>
</p>

```
> for (i in 1:10) print(cor(SlicerMorph.PCs[, i], 
+                           geomorph.PCs[, i]))
[1] 0.9999999
[1] 1
[1] 1
[1] -1
[1] -0.9999997
[1] 0.9999863
[1] -0.9999906
[1] -0.9999935
[1] -0.999997
[1] -0.9999972
```


**4. Finally, compare allometric regression models from SLicerMorph aligned coordinates to the one that used raw LM coordinates and gpa alignment from geomorph.**
```
summary(fit.slicermorph)

summary(fit.rawcoords)
```
This should print out nearly identical results.
