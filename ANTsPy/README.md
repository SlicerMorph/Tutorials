# ANTsPy Registration Tutorial: Mouse Craniometric Analysis

## Introduction

This tutorial demonstrates a complete morphometric analysis workflow using the **ANTsPyRegistration** module in 3D Slicer. You will:
- Create one canonically oriented sample as a reference
- Build a population-averaged template from mouse microCT scans
- Register individual specimens to the template
- Perform statistical shape analysis using Jacobian determinants
- Compare morphological variation between groups

**Dataset:** Low-resolution mouse head microCT scans from 30 different inbred and hybrid mouse strains, with 45 craniometric landmarks per specimen. Landmarks are necessary to provide an initial alignment, as most automated registration methods fail if the positional difference between two volumes are too large. Most cases you do not need as many landmark to do an approximate alignment, 4-8 are usually enough.

**Biological Context:** Mouse strains exhibit cranial shape variation due to genetic differences. This tutorial shows how to quantify and analyze these differences.

## Tutorial parts

The tutorial is in eight parts. Part I prepares the data. Parts II–VI each cover one tab of the **ANTsPy Registration** module and build on each other, so do them in order. Part VII covers the Pair-wise tab, and Part VIII troubleshooting and advanced topics.

1. [**ANTsPy-I: Data and reference specimen**](Data_and_reference.md): Download the data, load it, and reorient a reference specimen with Crop Volume
2. [**ANTsPy-II: Group-wise tab, rigid alignment to the reference**](Groupwise_rigid.md): Rigidly align all specimens to the reference
3. [**ANTsPy-III: Average tab, an average reference volume**](Average.md): Average the aligned specimens and their landmarks
4. [**ANTsPy-IV: Template tab, building a population template**](Template.md): Build a population template from all specimens
5. [**ANTsPy-V: Group-wise tab, registering all specimens to the template**](Groupwise_template.md): Register every specimen to the template, with quality control
6. [**ANTsPy-VI: Analysis tab, template mask and Jacobian analysis**](Analysis.md): Make a skull mask, then compare groups with Jacobian analysis, before and after FDR correction
7. [**ANTsPy-VII: Pair-wise tab, registering one image to another**](Pairwise.md): Start a registration from an existing transform
8. [**ANTsPy-VIII: Troubleshooting and advanced topics**](Troubleshooting.md): Common problems and advanced topics

---

## Resources

- **SlicerANTsPy Documentation:** [GitHub](https://github.com/SlicerMorph/SlicerANTsPy)
- **ANTs Documentation:** [ANTs Wiki](https://github.com/ANTsX/ANTs/wiki)
- **3D Slicer Training:** [Slicer Documentation](https://slicer.readthedocs.io/)
- **SlicerMorph:** [slicermorph.github.io](https://slicermorph.github.io/)

## Citation

If you use this workflow in your research, please cite:

- **3D Slicer:** Fedorov A., et al. (2012). 3D Slicer as an image computing platform for the Quantitative Imaging Network. *Magnetic Resonance Imaging*, 30(9), 1323-1341.
- **ANTs:** Avants B.B., et al. (2011). A reproducible evaluation of ANTs similarity metric performance in brain image registration. *NeuroImage*, 54(3), 2033-2044.
- **SlicerMorph:** Rolfe S., et al. (2021). SlicerMorph: An open and extensible platform to retrieve, visualize and analyse 3D morphology. *Methods in Ecology and Evolution*, 12(10), 1816-1825.

---

**Tutorial Version:** 1.1  
**Last Updated:** July 31, 2026  
**Questions?** Open an issue on the SlicerANTsPy GitHub repository
