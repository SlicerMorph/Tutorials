## Dealing with DICOMs

[DICOM is a complex standard](https://www.dicomstandard.org/current) for storing and exchanging medical imaging modalities. While it is ubiquitous for clinical imaging research, it is not a common format for high-resolution imaging of organismal structure via microCT. 

Slicer has an extensive support for DICOM format through its [`DICOM' module.](https://slicer.readthedocs.io/en/latest/user_guide/modules/dicom.html). We refer you to this extensive documentation, if you are planning to work with DICOM datasets in Slicer. However, there are few key points we would like highlight: 

1. DICOM format may contain sensitive, private information even if the images themselves are of non-human species (e.g., data coming from vet clinics). So if you are posting questions on the forum and have screenshots, make sure either your data is anonymized, or those fields are covered.  
2. DICOM module will create a local database on your computer, which contains all the imported DICOM files and the associated metadata. If you are doing human subject research, make sure you have necessary privacy protections in place (particularly if this is a shared computer). 
3. Not all DICOMs are created equal. Slicer will not load any DICOM that's not compliant, and it will require patching them (correcting them). Non-compliance to the standard seems to be a common issue for DICOMs generated by research microCT vendors. If you encounter such datasets, before proceeding with using other software, which may ignore such issues, please try to understand what Slicer is complaining about. This can be obtained by reading the output from the DICOM module. 

[See this forum thread about a large collection of baboon skulls from MorphoSource that seems to have incorrect DICOM metadata that is resulting in measurement errors and other problems](https://discourse.slicer.org/t/load-specific-dicom-data-with-python-in-slicer-5-6-1/35314)
