# MorphoSource Import Module User Guide

![Snapshot of MorphoSourceImport UI](MorphoSourceImport.png "MorphoSourceImport")
## Introduction
The MorphoSource Import Module for 3D Slicer enables users to search, query, and download 3D mesh and CT image series from the MorphoSource database. These can then be imported into 3D Slicer for analysis and visualization.

## Prerequisites
- Latest version of 3D Slicer installed.
- Internet access for querying the MorphoSource database.
- A MorphoSource account and an API key.

## Launching the Module
1. Open 3D Slicer.
2. Download the SlicerMorph extension via the Extensions Manager.
3. Once 3D Slicer restarts, go to the Modules Menu
4. Select the MorphoSourceImport module.

## Performing a Query
In the MorphoSource Query Parameters section:
- **Query Keyword**: Enter a keyword (e.g., "Skull").
- **Taxon**: Specify taxonomic classification (optional, e.g., "Primates").
- **Media Tag**: Enter a media tag to refine search (optional).
- **Data Types**: Choose desired data types (Mesh, CT Image Series).
- **Open Access Datasets Only**: Check to retrieve only open access datasets.
- Set the download folder by clicking "Select Download Folder".
- Click "Submit Query".

## Reviewing Query Results
Results will be displayed in a table with columns:
- **Image**: Thumbnail preview, rightlick to open Morphosource object webpage for more details.
- **Media ID**: Unique media identifier.
- **Title**: Dataset title.
- **Media Type**: Media type (Mesh or CT Image Series).
- **Object ID**: Unique object identifier.
- **File Size**: Size (MB) of Morphosource Media Item.
- Navigate using "Previous Page" and "Next Page".

- Once your search queries have loaded, you can click the 
"Download Query Results" button to obtain a .csv document with 
all your search queries.


## Downloading Data
1. Select desired datasets by checking the boxes.
2. Click "Download Media Items".
3. Click "Set API Key" and enter your API key.
4. Enter your Usage Statement.
5. Check at least one relevant Usage Category.
6. Click "Download Checked Items".

## Using the Data
After import, use 3D Slicer's tools for visualization and analysis.

## Conclusion
The MorphoSource Import Module is a valuable tool for accessing and utilizing 3D anatomical data from MorphoSource in 3D Slicer.