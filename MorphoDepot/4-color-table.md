_MorphoDepot Tutorial · Part 4 of 10 — Preparing Data: Color Table_

[⬅ Overview](./README.md)  ·  [⬅ Prev: Preparing Data: 3D Volume](./3-prepare-volume.md)  ·  [Next: Creating the Repository ➡](./5-create-repo.md)

---

## **4. Preparing Data for MorphoDepot Repository: Color Table with Terminologies.**

You must define what anatomical structures will be segmented. This ensures all collaborators use the exact same terminology for labels. We suggest creating these color tables comprehensively (include as much structural detail as possible). You can see some examples of existing terminology color tables at https://github.com/SlicerMorph/terms-and-colors

### **4.1 Color Table Creation**

* **Custom Color Table:** Use the `Colors` module of Slicer to create the color table in the csv format. We recommend using **Uberon** anatomical terms as reference. *For more detail about importing and using Uberon See:* [SlicerMorph Color Table Creation Tutorial](https://github.com/SlicerMorph/Tutorials/blob/main/Segmentation/colors-and-terms/README.md)

* **Load from URL:** If you find a directly relevant color table in the referenced Terms-and-Colors repository, you can load it directly into Slicer using the Sample Data module's `Load From URL` feature
  * *Example:* Copy the Raw URL of a CSV file (e.g., from the SlicerMorph repo).
  * Use the **Sample Data** module -> **Load data from URL**.

### **4.2 What MorphoDepot requires of the color table**

* **Name it like a file.** MorphoDepot saves the color table node into the repository under its node name, so the name may contain only letters, digits, periods, underscores, and dashes, and must start and end with a letter or a digit. Rename it in the **Data** module (use the *All nodes* tab, then right-click -> Rename).
* **Terminology.**
  * For an **Archival** repository (in the MorphoDepot organization), a real terminology table is **mandatory**: a generic or built-in Slicer table — `Labels`, `GenericAnatomyColors`, `GenericColors`, a continuous colormap such as `Viridis` — is rejected outright, and so is a custom table with any entry missing its terminology. Fix the table, reload it, and try again.
  * For a **Personal** repository, MorphoDepot offers to fill any missing entries with a default terminology (Tissue, SNOMED CT `85756007`) so a classroom exercise is not blocked by ontology work. You can decline and complete the terminology by hand instead.
* **Cover what you expect to be segmented.** The color table is what contributors name their segments from, so a missing term is a missing structure. It is easier to add terms now than to reconcile ad-hoc segment names at release time (a color table *can* be replaced later — see [Part 5](./5-create-repo.md#52-what-staging-freezes-and-what-you-can-still-change) and [Part 9](./9-releases.md) — but the segments already named from the old one will not rename themselves).

---

[⬅ Overview](./README.md)  ·  [⬅ Prev: Preparing Data: 3D Volume](./3-prepare-volume.md)  ·  [Next: Creating the Repository ➡](./5-create-repo.md)
