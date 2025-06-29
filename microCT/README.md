# What is microCT imaging? 

MicroCT imaging, short for micro-computed tomography, is a technique that uses X-rays to create detailed 3D images of objects. It works by taking many 2D X-ray images, called projection images, of an object from different angles and then combining them using a process called reconstruction to form a 3D virtual histology of the object. This method is non-destructive, meaning the object being scanned remains intact. 
## Comparison to Medical Imaging
MicroCT imaging is similar to medical CT (computed tomography) scans used in hospitals, but there are some significant differences. Both use X-rays and reconstruction to create 3D images. 
The term "microCT" does not originate from its frequent use in scanning small objects, but rather from the difference in the beam diameter, which is also referred to as the "spot size", and resolution range compared to medical CT (computed tomography). In microCT, the X-ray beam and detector are optimized to achieve extremely high resolutions, often in the micrometer (µm) range, which is why "micro" is part of the name. This capability enables microCT to reveal incredibly fine details in scanned objects.

In contrast, medical CT uses a larger spot size and broader X-ray beam to image larger objects, such as human organs or entire body sections. Medical CT is designed for speed and efficiency when scanning large volumes, prioritizing to reduce the harm to the patient due to the effects of ionizing radition on cellular structure over ultra-fine resolution. The typical resolution in medical CT is in the millimeter range—sufficient for diagnosing conditions like tumors or fractures but unable to capture the microscopic features that microCT excels at revealing.

The smaller beam diameter or spot size in microCT systems provides the ability to focus on tiny details, making it ideal for research applications like analyzing bone microstructure, studying the internal anatomy of small organisms, or examining the porosity of materials. However, the trade-off is that microCT has a much smaller field of view, longer scan times, and may not be suitable for imaging larger specimens efficiently. Medical CT scans are used for diagnosing diseases in humans, while microCT is often used in research, engineering, and studying biological specimens.

## Projection (Shadow) Images
Shadow, or projection, images in microCT imaging are the 2D X-ray images that are captured during the scanning process. They are formed when an X-ray beam passes through the object being scanned and creates a pattern of varying intensities on the detector. These patterns resemble shadows because they display the silhouette and internal structure of the object, depending on how much the X-rays are absorbed—or attenuated—by different parts of the material.

 <figure>
  <img src="./projection.png" alt="projection images">
  <figcaption><b>Fig.1 X-ray projection images vs Cross-sectional images.</b> Three images on the left are examples of projection image taken by the scanner at various angles during the scanning. These projection images are then mathematically reconstructed into cross-sectional images shown on the right (hence the name tomography). It is these cross-sectional images that we can go through in program like 3D Slicer and visualize as a virtual 3D object.   </figcaption>
</figure> 


### How Shadows/Projection Images Are Created
The X-ray source emits a beam of X-rays, which travel in straight lines toward the object. As the X-rays interact with the object, their intensity is reduced in certain areas depending on the object's thickness, density, and composition. This reduction in X-ray intensity is known as attenuation, which occurs because X-rays are absorbed or scattered by the material.

Dense materials, like bone or metal, attenuate more X-rays because they absorb or scatter a larger portion of the beam. This results in darker areas on the shadow or projection image. For example, in mammalian skulls enamel of the tooth is typically the densest material , followed by the inner ear. In images above, those regions are the darkest, indicating that a lot of the X-ray is being absored.

Less dense materials, like soft tissue or air, attenuate fewer X-rays, leading to lighter areas on the image. Again in the images above, particularly in the middle one, the lightest shade of gray (on the right) indicates air. Similarly the regions of the skull with large spaces such as endocranial cavity, nasal passages etc, show in lighter shades of gray compared to the bony part of the skull

After passing through the object, the remaining X-rays strike the detector, which records the variation in X-ray intensity across the surface. This results in a 2D projection image that reflects both the external outline and internal structure of the object, based on how much X-rays were attenuated in different regions.

### Where Projection Images Are Captured
The shadows are captured on an X-ray detector, which is positioned opposite the X-ray source, with the object placed between them. As the object rotates during the scan, multiple shadow or projection images are captured from various angles. This rotational capture process allows the system to collect sufficient data for reconstructing a 3D model of the object.
 <figure>
  <img src="https://www.microphotonics.com/wp-content/uploads/2015/05/micro-CT-x-ray-source-schematic.png" alt="microCT schematic">
  <figcaption><b>Fig.2 Schematic of a microCT system (source Microphotonics).</b></figcaption>
</figure> 


### Why Attenuation is Important
Attenuation is a key factor in creating shadow images because it reveals the differences in material properties within the object. The degree of attenuation depends on factors like the material's atomic number and density. High attenuation areas (such as bone) appear darker on the image, while low attenuation areas (such as soft tissue) appear lighter. These variations are what make it possible to distinguish different structures within the object.

By understanding attenuation and capturing these 2D projection images, microCT systems provide the raw data necessary to reconstruct detailed 3D models of an object’s internal and external features. These shadow images are the foundation of the imaging process, bridging the physical characteristics of the object with the final reconstructed visualization.

## Reconstructing slices 
Reconstruction is the process of combining all the 2D projection images into a detailed 3D representation of the object. Complex mathematical algorithms are used to achieve this. One widely used algorithm is [the Feldkamp-Davis-Kress (FDK) algorithm](https://opg.optica.org/josaa/abstract.cfm?uri=josaa%E2%80%901%E2%80%906%E2%80%90612), which is specifically designed for cone-beam CT systems like microCT. The FDK algorithm is efficient and well-suited for reconstructing high-resolution images from a large number of projections. It is particularly effective for small-scale objects, making it a popular choice in microCT imaging. Throught the reconstruction process, scientists and researchers to visualize the internal structure of an object layer by layer, without physically cutting it open. 

### Hounsfield Units and Calibration
Hounsfield Units (HU) are used in medical CT imaging to measure tissue density, with water at 0 HU, air at -1000 HU, and dense bone above 1000 HU. This standardized scale helps doctors differentiate between various tissues. In microCT imaging, however, values in the images are not necessarily calibrated to a standard like HUs. Instead, the images display relative differences in X-ray absorption, meaning the values are not standardized across scans or systems. 

This lack of calibration has consequences. It makes comparisons between microCT scans from different machines or conditions challenging and prevents precise density measurements like those achieved with HUs. However, it is possible to calibrate microCT scans using phantoms—objects with known material properties. For example, density phantoms made of materials like hydroxyapatite (a mineral in bones) or aluminum can be scanned alongside the specimen. These phantoms establish reference values, enabling a calibration curve that provides standardized density measurements. Water or air phantoms can also serve as baselines. Calibration broadens the scope of microCT imaging, allowing for quantitative studies and more reliable comparisons across systems. This is especially useful in fields like bone research,where measuring bone mineral density is critical, or materials science, where precise density values inform material properties. Despite typically relying on relative values, microCT imaging can achieve greater versatility and accuracy with proper calibration.

 <figure>
  <img src="./medCTvmicroCT.png" alt="Medical CT compared to microCT">
  <figcaption><b>Fig.3 Medical CT of a human head compared to microCT of a dry mouse skull.</b> In both dataset redline provides the <b>Line Profile</b> of the intensities along its path. The plot of the line profile shows that in medical CT values start around -1000 (which is air), then jump about 1500 (dense cranial bone), then drops down to 0 (which is brain tissue. Most of the soft tissue is water, hence HU value of 0). In contrast, microCT values are not calibrated and range from 0 to 22,000 in this datasets. While the values are different, profiles are similar (with the exception of lack of brain tissue in the mouse skull). The resolution of medical CT is 1.0x1.0x5mm whereas the resolution of microCT dataset is 0.023x0.023x0.023 (or 23 micrometers). Compared to medical CT, microCT provides extremely detailed view of the cranial bones, such as the delicate turbinates in mouse.   </figcaption>
</figure> 


## Imaging Soft Tissue with MicroCT
Imaging soft tissue with microCT presents unique challenges because soft tissues have low X-ray attenuation, meaning they do not absorb X-rays as effectively as denser materials like bone or metal. This results in poor contrast between different soft tissue types in the final images, making it difficult to distinguish their structures. For example, muscles, fat, and connective tissues may appear very similar in unenhanced microCT scans, limiting the technique's usefulness for studying soft tissue anatomy.

To overcome this limitation, researchers use diffusible stains, which are contrast agents that enhance the visibility of soft tissues in microCT scans. These stains work by binding to specific components of the tissue, increasing their X-ray attenuation and thereby improving contrast. One commonly used stain is iodine-based solutions, such as Lugol's iodine (I2KI), which binds to glycogen and other tissue components. Another example is phosphotungstic acid (PTA), which interacts with proteins and provides excellent contrast for imaging muscle fibers and other soft tissues.

<img src="https://github.com/SlicerMorph/Tutorials/blob/main/microCT/liquid%20phantoms.png" width="800">

For instance, diffusible iodine-based contrast-enhanced computed tomography (diceCT) has been successfully used to visualize the internal anatomy of small animals, embryos, and even human soft tissues. This technique allows researchers to study delicate structures like blood vessels, nerves, and ligaments in three dimensions without damaging the specimen. In one study, PTA staining was used to reveal the intricate arrangement of muscle fibers in a mouse limb, providing insights into its functional morphology. Similarly, iodine staining has been applied to study the internal anatomy of insects, enabling detailed visualization of their organs and tissues.


<figure>
  <img src="./pone.0142974.g001.png" alt="microCT scan of stained mouse heads" width="800">
  <figcaption> <b>Fig.4 MicroCT images of mouse heads stained using iodine.</b> (From Figure 1 of Anderson R, Maga AM (2015) A Novel Procedure for Rapid Imaging of Adult Mouse Brains with MicroCT Using Iodine-Based Contrast. PLoS ONE 10(11): e0142974. https://doi.org/10.1371/journal.pone.0142974)</A> </figcaption>
</figure> 


## MicroCT Reconstructions Data Formats
Saving reconstructed microCT images as individual 2D images in formats like PNG/JPG/TIFF/BMP can lead to several challenges, especially when compared to more specialized formats like DICOM or NRRD.

Problems with Saving as Individual 2D Images (e.g., PNG)
1.	**Loss of Metadata:** 2D bitmap files are primarily designed for visual representation and do not inherently store metadata about the scan, such as voxel size, spatial orientation, or acquisition parameters. This makes it difficult to interpret the images in a meaningful 3D context.
2.	**Fragmentation of Data:** When each slice of a microCT scan is saved as a separate 2D  files, the dataset becomes fragmented. Managing hundreds or thousands of individual files can be cumbersome and prone to errors, such as missing or misordered slices.
3.	**Lack of 3D Context:** Bitmap files are inherently 2D, so they do not preserve the spatial relationships between slices. Reconstructing the 3D volume from these files requires additional effort and software, which may introduce errors or inconsistencies.
4.	**Limited Interoperability:** 2D bitmaps files are not standardized for medical or scientific imaging, making it harder to integrate them into workflows that rely on specialized software for analysis or visualization.
   
### Advantages of DICOM Format
[DICOM (Digital Imaging and Communications in Medicine)](https://www.dicomstandard.org/current) is a widely used format in medical imaging. It addresses many of the issues associated with 2D bitmap formats:

*	**Metadata Storage:** DICOM files include detailed metadata about the scan, such as voxel dimensions, patient information, and imaging parameters, ensuring that the data is self-descriptive.
*	**Integration:** DICOM is a standardized format, making it compatible with a wide range of medical imaging software and systems.
*	**3D Context:** DICOM files can store entire 3D datasets in a single file or a structured series, preserving the spatial relationships between slices.

However, DICOM is not always ideal for microCT imaging, as it was primarily designed for clinical applications and may include unnecessary overhead for research-focused workflows.

### Why we prefer NRRD for MicroCT
[NRRD (Nearly Raw Raster Data)](https://teem.sourceforge.net/nrrd/format.html) is a format specifically designed for scientific imaging, making it well-suited for microCT data:
1.	**Compact and Flexible:** NRRD files store the entire 3D dataset in a single file or a pair of files (data and header), reducing fragmentation and simplifying data management.
2.	**Rich Metadata:** The NRRD header is highly customizable, allowing researchers to include detailed information about voxel size, spatial orientation, and other scan parameters.
3.	**Open and Interoperable:** NRRD is an open format supported by many scientific imaging tools, such as 3D Slicer and ImageJ, as well as programming environments like Python and R, making it easy to integrate into research workflows.

In summary, while saving reconstructed microCT images as individual PNG/TIFF/BMP files may be convenient for simple visualization, it is not suitable for preserving the integrity and usability of the data. Secondarily exporting these 2D bitmap formats into a standard compliant DICOM format can be also challenging. That's because DICOM is a fairly complicated standard and without a good working knowledge of what DICOM fields to use to describe the imaging data, it is possible to create **incorrect** DICOM files. As such NRRD provide a better solution for research applications due to its flexibility, metadata support, and compatibility with scientific tools. Correctly, importing 2D image stacks (or sequences) and saving them into NRRD file with correct voxel dimensions, image orientation and other metadata is the first step in the 3D morphology workflows, and particularly for quantitative morphology. 

