## ProjectSemiLM
**Summary:** This module provides a utility to transfer a template of semilandmarks from a base 3D model to new 3D models using Thin Plate Splines (TPS) warp. A set of corresponding anatomical landmarks in the template and each target model is required.

### USAGE
#### ProjectSemiLM PARAMETERS
* __Base mesh__ Selects the reference model used to place the initial semi-landmark points (using any method) from the scene.

* __Base landmarks__ Selects the fixed, anatomical landmarks placed on the reference model from the scene.

* __Base semi-landmarks__ Selects the semi-landmark points placed on the reference model from the scene.

* __Mesh directory__ Specifies the directory of models that will have semi-landmarks placed by this module.

* __Landmark directory__ Specifies the directory of fixed, anatomical landmarks corresponding to the models that will have semi-landmarks placed by this module. The landmarks should be stored as `.fcsv` files and have the same filename prefix as the corresponding model.

* __Output directory__ Specifies the directory where the semi-landmark files for each of the new models will be stored as `.fcsv` files.

* __Projection__ After the TPS transform transferred semi-landmarks may not always be on the surface of the model. This parameter allows projecting them to the nearest point on the model along the normal direction. 

#### LIMITATIONS
* This method requires fixed, anatomical landmarks to be placed on the template and each target mesh. If these are not available, the `ALPACA` module can be used as an alternative.

* The performance of this method will depend on the number and distribution of the anatomical landmarks, the amount of morphological variation in the dataset, and the selection of an appropriate reference model. 

* To minimize the bias associatde with using a template, it is recommended to use an average model as the base reference model, if one is available. If you already have fixed landmarks, you can use export the Mean Model from the GPA to be used as template. 

* If you have incomplete models as targets, but they still have the full set of anatomical landmarks, set the projection factor 0 to disable projection. That way you might still be able to place semi-landmarks in regions where the model is deficient or missing.  
