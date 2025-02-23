## MorphoDepot

MorphoDepot is the extension of the ["fork and contribute" workflows in software development](https://www.atlassian.com/blog/2013/05/stash-git-forking-development-workflow) to digital morphology tasks such as segmentation. It allows to grow a collection of segmented scans of biological specimens in a distributed way, without having to create a centrally managed repository. MorphoDepot repositories are created publicly under the data donor's github account and is under the control of the that account. The only requirement to create or contribute to such repositories is to have a github account and that you have the rights to distribute the data.

MorphoDepot is also the name of the 3D Slicer extension that allows you to use Slicer as a platform to create or interact with MorphoDepot repositories on github. We use GitHub's CLI (automatically installed with the MorphoDepot extensions) for task automation. 

### What can I use MorphoDepot for? 
If you are an instructor who is interested in corporating digital morphology techniques as a part of a course, you can use the MorphoDepot to create a repository with data, from which you can assign tasks to your students and optionally review them. MorphoDepot is fully integrated with [MorphoCloud On Demand for 3D morphology](https://instances.morpho.cloud), so beyond an obtaining a github account, your students do not have to install any software. All necessary tools (3D Slicer, SlicerMorph, MorphoDepot, etc) are already available in MorphoCloud On Demand instances. 

If you a partial segmentation of a specimen, and would like to share it with the rest of the community, you can do it through the MorphoDepot infrastructure. Others can continue what you have started and even contribute back to your repository (after your review and acceptance).

### MorphoDepot Extension
3D Slicer's MorphoDepot Extension consists of four modules:

* __MorphoDepot:__ Primary module which installs the necessary libraries for the functionality and allows you retrieve tasks that are assigned specifically to the user logged into the github. Through this module you can contribute new segmentations or revise existing ones, once a task is assigned to you.  
* __MorphoDepotReview:__ This module allows you to review the submitted segmentation tasks by others to the repository(ies) that you own. You can load their segmentation(s), and comment on them (i.e., request revisions) or accept and update the existing segmentation. 
* __MorphoDepotAccession:__ This module allows you to create a public MorphoDepot repository
* __MorphoDepotBrowse:__ When implemented, this module will allow you to search for MorphoDepot repositories using keywords such as species tag, image contents, modality type etc.

