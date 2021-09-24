# Generalized Procrustes Analysis (GPA) Module III

## Exporting data for Analysis in R 
You can get your morphometrics data from SlicerMorph into R in two ways:

1. You can use the SlicerMorphs GPA aligned coordinates directly into your allometric regression model or any other shape analysis.
2. You can use the original landmark coordinates as saved in FCSV or JSON format. 

Provided that you used identical settings across different GPA sofware (e.g., gpagen in geomorph), both options should (and will) give you identical results (within machine precision). Below is an exercise that demonstrates it using one of the sample datasets from SlicerMorph. 

### How to import FCSV and JSON files into R?

FCSV is a very simple file format. You can write your import function simply using the base R **read.csv()** function. For JSON, you will need a JSON parser library. THere are a number of them for R (e.g., jsonlite, rjson etc). SlicerMorphR package provides convenience functions to read/write fcsv. Please install via devtools by:


```
devtools::install_github('SlicerMorph/SlicerMorphR')
```

then the command,
```
SlicerMorphR::read.markups.fcsv('path to file')
```

will report the coordinates saved in the fcsv file as a 2D matrix. 

### Reading the GPA module LOG File
Additionally, as part of the SlicerMorphR, we provide another convenience function to parse the log output (**analysis.log** file) from GPA module output. Parser saves everything necessary, including the output files as well as raw coordinates, in a large R object. Please see the example 

```
> library(SlicerMorphR)
# point out to the analysis.log file created by the SlicerMorph's GPA function. 
> log=parser(file.choose(), forceLPS = TRUE)
  incomplete final line found on 'C:\Users\amaga\Desktop\1\2021-09-24_10_04_34\analysis.log'
> log
$input.path
[1] "C:/Users/amaga/Desktop/1"

$output.path
[1] "C:/Users/amaga/Desktop/1/2021-09-24_10_04_34"

$files
[1] "1.fcsv" "2.fcsv" "3.fcsv"

$format
[1] ".fcsv"

$no.LM
[1] 509

$skipped
[1] FALSE

$semi
[1] TRUE

$semiLMs
  [1] " 1"  "2"   "3"   "4"   "5"   "6"   "7"   "8"   "9"   "10"  "11"  "12"  "13"  "14"  "15"  "16"  "17"  "18"  "19"  "20"  "21"  "22"  "23"  "24"  "25"  "26"  "27"  "28"  "29"  "30"  "31"  "32"  "33"  "34"  "35"  "36"  "37"  "38" 
 [39] "39"  "40"  "41"  "42"  "43"  "44"  "45"  "46"  "47"  "48"  "49"  "50"  "51"  "52"  "53"  "54"  "55"  "56"  "57"  "58"  "59"  "60"  "61"  "62"  "63"  "64"  "65"  "66"  "67"  "68"  "69"  "70"  "71"  "72"  "73"  "74"  "75"  "76" 
 [77] "77"  "78"  "79"  "80"  "81"  "82"  "83"  "84"  "85"  "86"  "87"  "88"  "89"  "90"  "91"  "92"  "93"  "94"  "95"  "96"  "97"  "98"  "99"  "100" "101" "102" "103" "104" "105" "106" "107" "108" "109" "110" "111" "112" "113" "114"
[115] "115" "116" "117" "118" "119" "120" "121" "122" "123" "124" "125" "126" "127" "128" "129" "130" "131" "132" "133" "134" "135" "136" "137" "138" "139" "140" "141" "142" "143" "144" "145" "146" "147" "148" "149" "150" "151" "152"
[153] "153" "154" "155" "156" "157" "158" "159" "160" "161" "162" "163" "164" "165" "166" "167" "168" "169" "170" "171" "172" "173" "174" "175" "176" "177" "178" "179" "180" "181" "182" "183" "184" "185" "186" "187" "188" "189" "190"
[191] "191" "192" "193" "194" "195" "196" "197" "198" "199" "200" "201" "202" "203" "204" "205" "206" "207" "208" "209" "210" "211" "212" "213" "214" "215" "216" "217" "218" "219" "220" "221" "222" "223" "224" "225" "226" "227" "228"
[229] "229" "230" "231" "232" "233" "234" "235" "236" "237" "238" "239" "240" "241" "242" "243" "244" "245" "246" "247" "248" "249" "250" "251" "252" "253" "254" "255" "256" "257" "258" "259" "260" "261" "262" "263" "264" "265" "266"
[267] "267" "268" "269" "270" "271" "272" "273" "274" "275" "276" "277" "278" "279" "280" "281" "282" "283" "284" "285" "286" "287" "288" "289" "290" "291" "292" "293" "294" "295" "296" "297" "298" "299" "300" "301" "302" "303" "304"
[305] "305" "306" "307" "308" "309" "310" "311" "312" "313" "314" "315" "316" "317" "318" "319" "320" "321" "322" "323" "324" "325" "326" "327" "328" "329" "330" "331" "332" "333" "334" "335" "336" "337" "338" "339" "340" "341" "342"
[343] "343" "344" "345" "346" "347" "348" "349" "350" "351" "352" "353" "354" "355" "356" "357" "358" "359" "360" "361" "362" "363" "364" "365" "366" "367" "368" "369" "370" "371" "372" "373" "374" "375" "376" "377" "378" "379" "380"
[381] "381" "382" "383" "384" "385" "386" "387" "388" "389" "390" "391" "392" "393" "394" "395" "396" "397" "398" "399" "400" "401" "402" "403" "404" "405" "406" "407" "408" "409" "410" "411" "412" "413" "414" "415" "416" "417" "418"
[419] "419" "420" "421" "422" "423" "424" "425" "426" "427" "428" "429" "430" "431" "432" "433" "434" "435" "436" "437" "438" "439" "440" "441" "442" "443" "444" "445" "446" "447" "448" "449" "450" "451" "452" "453" "454" "455" "456"
[457] "457" "458" "459" "460" "461" "462" "463" "464" "465" "466" "467" "468"

$scale
[1] TRUE

$MeanShape
[1] "MeanShape.csv"

$eigenvalues
[1] "eigenvalues.csv"

$eigenvectors
[1] "eigenvectors.csv"

$OutputData
[1] "OutputData.csv"

$pcScores
[1] "pcScores.csv"

$ID
[1] "1" "2" "3"

$LM
, , 1

             x         y          z
1   -119.41100 -146.0000 -103.12200
2    -91.28290 -146.5180 -106.17800
3   -104.41600 -147.3190 -101.43900
4   -132.96001 -148.9320 -105.66100
5   -102.12800 -149.5630  -88.17560
6    -77.41010 -149.4530 -108.55200
7   -145.47900 -153.5410 -109.04600
8   -101.93500 -154.1380 -113.94700
9   -122.89200 -155.0570 -112.92400
10   -65.81770 -155.9640 -112.56000
11   -87.08820 -157.8150 -115.46900
12  -100.37100 -160.0540  -77.19170
13  -113.62000 -158.4480 -103.31000
14   -95.21590 -159.3920 -103.03000
15  -137.91299 -161.3010 -117.83200
16  -112.33900 -162.3110 -118.69100
17  -156.11400 -162.7710 -111.97900
18   -74.72500 -163.3230 -121.26100
19  -106.02300 -164.8930  -92.30380
20   -55.08290 -163.6990 -116.02500
21   -96.85870 -166.4300 -123.10200
22  -124.21300 -167.4240 -124.16500
23  -128.85600 -167.2480 -109.38000
24  -149.68600 -169.9070 -121.99100
25  -103.00600 -170.8400  -68.88910
26   -85.96550 -169.6620 -106.72200
27   -84.34640 -171.1590 -129.33600
28  -117.47300 -171.5760  -99.07040
29   -62.44160 -171.3610 -125.33500
30  -109.37700 -171.9680 -131.22400
31   -94.74760 -171.6800  -96.17270
32  -163.04100 -174.0280 -119.97600
33   -99.47630 -173.4850  -83.00870
34  -135.74699 -174.0220 -129.72800
35   -48.38560 -174.8440 -122.96800
36   -74.05800 -175.5810 -113.15600
37  -140.78101 -176.2640 -113.74500
38   -92.75697 -385.3340 -150.51355
39  -124.20300 -176.8450 -136.89600
40   -95.77080 -176.5430 -136.61400
41   -87.00420 -364.9549 -161.73238
42   -72.11260 -178.2390 -133.10100
43  -115.20700 -179.1930  -87.23600
44  -129.17500 -178.8170  -98.14000
45   -83.06470 -179.9900  -96.96940
46   -86.64186 -341.7536 -169.86841
47  -148.43800 -181.4740 -131.42799
48   -81.93000 -181.8410 -141.87601
```


See the github page for SlicerMoprhR for more examples of usage. > library(SlicerMorphR)

These three functions provide everything you need to get SlicerMorph data into R; regardless if it is raw coordinates, or output from GPA module. 

## Example R analysis
Now, we will go through a R markdown document that will use the parser() function above to import data into R and fit an allometric regression model to both raw coordinates as well and GPA aligned procrustes coordinates. To do the next steps in tutorial, you have to complete these two steps:

1. Use the GPA module to run GPA/PCA analysis on the Gorilla skull landmark dataset available in the `Sample Data` module. You can refer to the instructions in [GPA I: Basics of GPA Fitting](../GPA_1/README.md) for details on using this module. Make a note of the output folder you specified. You will need to enter this into the R.
2. [Download this R markdown document and open in Rstudio](https://raw.githubusercontent.com/muratmaga/SlicerMorph_Rexamples/main/Geomorph_regression.Rmd) 

Continue following the instructions in the R Markdown document. 

