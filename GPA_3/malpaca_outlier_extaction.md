# Extract outlier estimates from MALPACA output using SlicerMorphR

One of the benefits of multi-template pipeline is the ability to assess confidence in the estimates. Here we demonstrate a pipeline based on a simple heuristic `(mean RMSE + 2.SD RMSE) ` to flag individual estimates as potential outliers. It should be noted that because MALPACA is using median to obtain final landmark positions, it is robust to an occasional outlier (i.e., removal of the outlier and re-estimate of the position should have no appreciable effect on final LM estimate). However, in cases where an ill-chosen template produces consistently poor results, diagnostic plots like the one shown below would be helpful to identify the problem (abundance of LMs above threshold for that template) and re-run the MALPACA pipeline by replacing the template by another sample, or simply re-estimating without the outlier. 

This tutorial demonstrates how to use functions in the SlicerMorphR R package to load MALPACA output into R and function and extract and remove outliers from estimates of individual templates to achieve a new set of median estimates.

## Step 1. Install and load the development version of SlicerMorphR
The functions used in this tutorial is in a developer's own repository, so please re-install the SlicerMorph using the following script:
```
devtools::install_github(‘chz31/SlicerMorphR')
library(SlicerMorphR)
```
The functions used in this tutorial include:
```
read.malpaca.estimates()
extract.outliers()
get.medians()
rmse()
```

## Step 2. Download Sample data
The downloading and unzipping may takes a minute.
```{r download data}
#Set up a path for saving downloaded data
save.dir = "path/for/downloading/sampleData/"
#
zip_url <- "https://github.com/SlicerMorph/Mouse_Models/archive/refs/heads/main.zip"
zipFilePath = paste(save.dir, "Mouse_Models.zip", sep = "/")
download.file(url = zip_url, destfile = zipFilePath)
unzip(zipfile = zipFilePath, exdir = save.dir)
```
* The `ape_malpaca` folder in the `Mouse_Models_main` contains the ape MALPACA output from the paper https://www.biorxiv.org/content/10.1101/2022.01.04.474967v1. There are three species in the sample: chimpanzee, gorilla, and orangutan. There are six templates used for landmarking 46 specimens.
  * TThe individualEstimates folder contains all estimates of each template.
  * The medianEstimates folder contains all MALPACA median estimates.
  * The templates_LMs folder contains the manual landmark sets of the six templates.    
```
#set up paths to downloaded MALPACA output
MAL_outDir_git<- paste(save.dir, 'Mouse_Models-main/ape_malpaca', sep="/") #MALPACA output dir
templates_path_git<- paste(save.dir, 'Mouse_Models-main/ape_malpaca/templates_LMs', sep='/') #templates folder dir
```

## Step 3. Read all median estimates
```{r Read MALPACA estimates}
#Read all MALPACA individual and median estimates
LMs <- read.malpaca.estimates(MALPACA_outputDir = MAL_outDir_git, templates_Dir = templates_path_git)

#Stores median and indvidual estimates into separate arrays
allEst <- LMs$allEstimates #Store all individual estimates in a 4D array
MAL_medians <- LMs$MALPACA_medians #Store all median estimates in a 3D array
```
* `read.malpaca.estimates()` stores all individual estimates in to a 4D array `$allEstimates` [i, j, k, n]. i = number of templates; j = dimension; k = number of templates; n = sample size. The `LMs$allEstimates` is then passed to `allEst`. These dimensions are also named by landmark numbers, x, y, z coordinates, template file names, and target file names.
```
> dim(allEst)
[1] 41  3  6 46
> dimnames(allEst)
[[1]]
 [1] "1"  "2"  "3"  "4"  "5"  "6"  "7"  "8"  "9"  "10" "11" "12" "13" "14" "15" "16" "17" "18" "19" "20" "21" "22" "23" "24" "25" "26" "27" "28" "29" "30" "31" "32" "33" "34" "35" "36" "37" "38" "39"
[40] "40" "41"

[[2]]
[1] "x" "y" "z"

[[3]]
[1] "USNM084655-Cranium_merged_1" "USNM142185-Cranium"          "USNM153830-Cranium"          "USNM176236-Cranium_merged_1" "USNM590953_CRANIUM"          "USNM599167_CRANIUM"         

[[4]]
 [1] "USNM142188-Cranium"          "USNM142189-Cranium"          "USNM142194-Cranium"          "USNM145300-Cranium"          "USNM145302-Cranium"          "USNM145303-Cranium"         
 [7] "USNM145307-Cranium"          "USNM145308-Cranium"          "USNM145309-Cranium"          "USNM153805-Cranium"          "USNM153806-Cranium"          "USNM153822-Cranium"         
[13] "USNM153824-Cranium"          "USNM174701-Cranium_merged_1" "USNM174703-Cranium_merged_1" "USNM174704-Cranium_merged_1" "USNM174707-Cranium_merged_1" "USNM174710-Cranium_merged_1"
[19] "USNM174715-Cranium_1"        "USNM174722-Cranium"          "USNM176209-Cranium"          "USNM176211-Cranium"          "USNM176216-Cranium"          "USNM176217-Cranium"         
[25] "USNM176219-Cranium"          "USNM176228-Cranium_merged_1" "USNM197664-Cranium"          "USNM220060-Cranium"          "USNM220062-Cranium_merged_1" "USNM220063-Cranium_merged_1"
[31] "USNM220065-Cranium_merged_1" "USNM220324-Cranium"          "USNM252575-Cranium"          "USNM252577-Cranium"          "USNM252578-Cranium"          "USNM252580-Cranium"         
[37] "USNM297857-Cranium"          "USNM399047-Cranium"          "USNM582726-Cranium"          "USNM588109-Cranium"          "USNM590942_CRANIUM"          "USNM590947_CRANIUM" 
```
* For example, you can fetch Landmark 10 to Landmark 12 in the 20th specimen estimated by the 4th template:
```
> allEst[10:12, , 4, 20]
           x         y         z
10 -124.9081 -231.6945 -155.1798
11  -80.0126 -236.2920 -156.5660
12 -122.5329 -247.1557 -148.1924
```
* `read.malpaca.estimates()` stores all median estimates in to a 3D array [i, j, n]. i = number of templates; j = dimension; n = sample size. 
```
> dim(MAL_medians)
[1] 41  3 46
```
* For example, you can fetich Landmark 3 to Landmark 5 of the target specimen 3 and 4:
```
> MAL_medians[3:5, , 3:4]
, , USNM142194-Cranium

         x        y        z
3 2.437980 447.8076 206.7258
4 2.140611 476.8763 194.1904
5 1.191707 481.3070 185.4908

, , USNM145300-Cranium

         x        y        z
3 2.900802 454.4485 200.2380
4 2.650484 478.9051 193.4880
5 2.161222 484.3778 185.5545
```

## Step 4. Use the `extract.outliers()` function in SlicerMorphR to find and remove outliers from estimates derived from individual templates. 
The `extract.outliers()` function calculates the Euclidian distance between each landmark estimated by an individual template to its corresponding MALPACA median estimate. If the distance exceeds a threshold, then this estimate is marked as an outlier. By default, the heuristic threshold is `(2 (standard deviation) + mean) of the pooled distances for all landmarks for each specimen`. `NA` is then assigned to its coordinates in the 4D array that stores all estimates of individual templates. For the parameters:
*	`allEstimates` is the 4D array that stores all estimates of individual templates, generated by ‘read.malpca.estimates()’
*	`MALPACA_medians` is the 3D array that stores MALPACA median estimates generated by ‘read.malpaca.estimates()’.
*	`outputPath` specifies the output directory for storing output the pdf files that contains the boxplots of distances between each individual estimate to its corresponding MALPACA median estimate.
*	`ZScore` specify the threshold for determining outliers. The default value is 2. This means that the threshold is (2 (standard deviation) + mean) of pooled distances between all pairs of individual estimates and the corresponding MALPACA median estimates. 

For the ape sample data, run:
`outliers <- extract.outliers(allEstimates = allEst, MALPACA_medians = MAL_medians, outputPath = outPath)`

* The `extract.outliers()` function exports a pdf file to the `outputPath`. Each page of the pdf file has a boxplot. 
  * Each boxplot demonstrates the Euclidean distances between every individual landmark estimate and its corresponding MALPACA median output for one ape specimen. 
  * Each column of a boxplot has six dots that represent distances between estimates of the six ape templates to the corresponding median estimate for one landmark. 
  * The red horizontal line represents the heuristic threshold, which is `(2 (standard deviation) + mean) of the pooled distances for all landmarks for each specimen`. 
  * A dot that lie above the red line means that the distance between a landmark estimate of a template and its corresponding MALPACA median estimate exceeds the threshold. Therefore, this landmark is an outlier. `NA` is then assigned to its coordinates.
 
<image>

* In addition to the boxplot, the results of `extract.outliers() `are also stored in the list `outliers`.
  * `$estimates_no_out` is a 4D array [i, j, k, n] that stores all landmark estimates from individual templates and marks the outliers as `NA`. i = number of templates; j = dimension; k = number of templates; n = sample size. 
  * `$outliers$outlier_info` stores indices for outlier landmarks generated by each template for each specimen. `NULL` means no outlier is detected. 
```
> outliers$estimates_no_out[18:23, , 2, 7]
            x        y        z
18   3.123262 441.5676 226.7014
19   1.254022 417.1126 206.4035
20  -2.445040 385.6000 193.9190
21         NA       NA       NA
22 -17.194445 457.1618 238.1673
23  28.504499 455.5570 234.8030

> outliers$outlier_info[[7]]
$`USNM084655-Cranium_merged_1`
NULL

$`USNM142185-Cranium`
[1] 21 39

$`USNM153830-Cranium`
[1] 21

$`USNM176236-Cranium_merged_1`
NULL

$USNM590953_CRANIUM
[1] 19 36 38

$USNM599167_CRANIUM
[1] 19 25 36 37
```
* `outliers$estimates_no_out[18:23, , 2, 7]` fetches Landmark 18 to 23 for the 7th target specimen estimated by template 2 from the 4D array that stores all estimates of individual templates. 
  * Note that the coordinates of Landmark 21 are marked as `NA` because it is an outlier.
* `outliers$outlier_info[[7]]` fetches and lists the outlier landmark indicies generated by each of the six ape templates for the 7th target ape specimen. For example:
  * ``$USNM084655-Cranium_merged_1`` returns `NULL`, meaning that the template USMN084655 generates no outlier estimate for the 7th target specimen. 
  * `$`USNM142185-Cranium`` returns `[1] 21 39` shows that it generates outlier estimates for landmark 21 and 39. 
 
## Step 5. Use the `get.medians()` function from SlicerMorphR to generate new median estimates from the results of `extract.outliers()`. 
`medians <- get.medians(AllLMs = outliers$estimates_no_out, outlier.NA = TRUE)`
For parameters: 
* `AllLMs` = input 4D array that stores all estimates of individual templates.
* `outlier.NA` by default is False. Setting up ‘True’ specifies there is NA in the input 4D array. This allows to passing the 4D array `outliers$estimates_no_out` created by `extract.outliers()` to the `get.medians()` function.

The result `medians` is a new 3D array [i, j, n] that stores median estimates for each specimen after outlier estimates of individual templates have been removed. i = number of templates; j = dimension; n = sample size.
```
> medians_no_outlier[1:2, , 3]
          [,1]     [,2]     [,3]
[1,] -7.218665 416.3260 158.4400
[2,] -1.850612 366.3141 180.6046
```
*  `medians_no_outlier[1:2, , 3]` fetches the new median estimates for Landmark 1 and 2 for the 3rd specimen

## Step 6. Compute RMSEs between the old and new median estimates
Finally, use rmse() to compute the root mean square error (rmse) between any two corresponding specimen' landmark matrices from the 3D arrays that stores the original median estimates and the new median estimates after extracting outliers. The rmse summarizes the positional differences between two landmark matrices. 

The example below uses `rmse()` computes the pairwise landmark distances/errors and rmse between the 26th specimen from the 3D arrays that stores old median estimates and new median estimates without outliers. The `rmse()` function takes two estimates
* `M1` is the first landmark matrix
* `M2` is the second landmark matrix

```
> RMSE_dists = rmse(M1 = MAL_medians[, , 26], M2 = medians_no_outlier[, , 26])
> 
> head(RMSE_dists)
$LM_distances
           1            2            3            4            5            6            7            8            9           10           11           12           13           14 
4.264961e-06 7.864200e-06 5.394797e-06 3.814697e-06 5.394797e-06 3.814697e-06 6.775709e-01 3.814697e-06 4.264961e-06 0.000000e+00 1.907349e-06 3.814697e-06 4.672031e-06 5.722046e-06 
          15           16           17           18           19           20           21           22           23           24           25           26           27           28 
3.814697e-06 4.264961e-06 1.907349e-06 3.814697e-06 1.298971e+00 8.529922e-06 5.953247e-01 5.394797e-06 1.907349e-06 3.814697e-06 5.478442e-06 5.394797e-06 6.607249e-06 8.529922e-06 
          29           30           31           32           33           34           35           36           37           38           39           40           41 
4.264961e-06 8.529922e-06 3.932100e-06 0.000000e+00 3.932100e-06 9.344062e-06 8.529922e-06 9.344062e-06 1.431983e+00 7.629395e-06 1.026671e+00 6.004116e-01 0.000000e+00 

$RMSE
[1] 0.3814594
```
* `RMSE_dists$LM_distances` store distances/errors between individual corresponding landmarks
* `RMSE_dists$RMSE` store the rmse between the two landmark matrices
