This folder contains tutorials for running MALPACA and an optional K-means method for selecting templates.

# MALPACA
[MALPACA.md](https://github.com/chz31/Tutorials/edit/main/MALPACA/MALPACA.md) contains the tutorial for running MALPACA, which is a method for automated landmarking using multiple templates that can accomate large morphological disparity within a sample. The MALPACA pipeline is essentially performing multiple independent ALPACA runs, each of which based on a single template. It then calculates the median from the results of all the ALPACA runs as the final output of the landmark estimates. For details of ALPACA, please see the [ALPACA tutorial](https://github.com/chz31/Tutorials/blob/main/ALPACA/README.md)


# K-means template selection
[K-means_templates_selection.md](https://github.com/chz31/Tutorials/blob/main/MALPACA/K-means_templates_selection.md) contains the tutorial for running a K-means based method to select multiple templates from a sample for executing the MALPACA pipeline. This is an optional steps that can be executed before a MALPACA run when users are uncertain about what templates should be used.
