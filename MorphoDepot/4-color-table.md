_MorphoDepot Tutorial · Part 4 of 9 — Preparing Data: Color Table_

[⬅ Overview](./README.md)  ·  [⬅ Prev: Preparing Data: 3D Volume](./3-prepare-volume.md)  ·  [Next: Creating the Repository ➡](./5-create-repo.md)

---

## **4. Preparing Data for MorphoDepot Repository: Color Table with Terminologies.**

You must define what anatomical structures will be segmented. This ensures all collaborators use the exact same terminology for labels. We suggest creating these color table comprehensively (include as much structural detail as possible), You can see some examples of existing terminology color tables at https://github.com/SlicerMorph/terms-and-colors

### **4.1 Color Table Creation**

* **Custom Color Table:** Use the `Colors` module of Slicer to create the color table in the csv format. We recommend using **Uberon** anatomical terms as reference. *For more detail about importing and using Uberon See:* [SlicerMorph Color Table Creation Tutorial](https://github.com/SlicerMorph/Tutorials/blob/main/Segmentation/colors-and-terms/README.md)  

* **Load from URL:** If you find a directly relevant color table in the referenced Terms-and-Colors repository, you can load it directly into Slicer using the Sample Data modules `Load From URL` feature  
  * *Example:* Copy the Raw URL of a CSV file (e.g., from the SlicerMorph repo).  
  * Use the **Sample Data** module \-\> **Load data from URL**.

---

[⬅ Overview](./README.md)  ·  [⬅ Prev: Preparing Data: 3D Volume](./3-prepare-volume.md)  ·  [Next: Creating the Repository ➡](./5-create-repo.md)
