_MorphoDepot Tutorial · Part 5 of 9 — Creating the Repository_

[⬅ Overview](./README.md)  ·  [⬅ Prev: Preparing Data: Color Table](./4-color-table.md)  ·  [Next: Project Management & Assignments ➡](./6-project-management.md)

---

## **5. Creating the Repository**

This step uploads your volume and creates the collaborative space on GitHub.

1. Open the **MorphoDepot** module and select the **Create** tab.

Before filling the accession form, you must choose the intended lifespan of your repository. **This choice is permanent** — it cannot be changed after the repository is created.

### Repository type: Short-term vs. Archival

|  | Short-term | Archival |
| --- | --- | --- |
| Who can create | Anyone | MorphoDepot **organization members** only |
| Where it lives | Your personal GitHub account | The **MorphoDepot organization** |
| Scan storage | GitHub attachment, up to 2 GB | Jetstream2 cloud storage (10 GB by default) |
| Citable with a DOI | No | **Yes** — minted through Zenodo at each release |
| Governance | Yours alone; delete anytime | Community-governed (see below) |
| Best for | Teaching, drafts, experiments | Lasting, citable datasets |

* **Short-term** repositories are for teaching demonstrations, workshops, or temporary projects. Terminology validation is relaxed and they are meant to be ephemeral. If a short-term repository becomes useful and informative, consider creating an archival one from that point on.
* **Archival** repositories are for research publications and datasets meant for long-term citation. They live in the MorphoDepot organization, are stored on dedicated cloud storage, and become **citable with a DOI** at their first release that contains a segmentation. Creating archival datasets requires joining the organization at [join.morphodepot.org](https://join.morphodepot.org).

The steps below walk through creating the repository (the common, short-term case). **Archival** datasets must also meet a few [additional requirements](#archival-datasets-additional-requirements), described at the end of this page.

### **5.1 Subject Data**

* **Source Volume:** Select your cropped, cleaned, and properly named volume from the dropdown. (Mandatory field)  
* **Color Table:** Select your custom or loaded color table. (Mandatory field). If you do not choose a color table with terminology, you will not be able to proceed.   
* **Baseline Segmentation (Optional):** If you have an existing segmentation that serves as a starting point. 

### **5.2 Accession Form**

Fill out the metadata. This generates the README.md for the repository.

* **Subject Type:** Biological Specimen vs. Other.  
* **Accession Status:**  
  * *Accessioned:* Provide the institution code and catalog number (links to iDigBio if applicable).  
  * *Non-Accessioned:* Provide a unique identifier.  
* **Taxonomy:** Enter the **Species Name**. Verify spelling against Linnean hierarchies.  
* **Biological Details:** Sex and Developmental Stage (if known).  
* **Imaging Details:** Modality (CT, MRI, etc.) and contrast staining info.  
* **License:**  
  * *CC BY 4.0 (preferred):* Open, attribution required.  
  * *CC BY-NC 4.0:* Open, attribution required, non-commercial only.  
* **Repository Name:** Create a short, unique name (No spaces, e.g., Project\_FishSkull\_2024).

### **5.3: Adding Screenshots** 

You can capture and annotate screenshots to showcase your dataset. These images will be embedded in the repository's README and visible in search results.

**5.3.1 Taking Screenshots**

1. In the Create tab, locate the **Screenshots** section  
2. Arrange your 3D/2D views to show interesting features of your data  
3. Click **Take Screenshot**  
   * The current viewport is captured  
   * Screenshot is saved with a numbered filename (screenshot-1.png, screenshot-2.png, etc.)  
   * A counter shows how many screenshots you've taken

**5.3.2 Reviewing and Editing Screenshots**

1. Click **Review Screenshots** to open the review dialog  
2. The dialog shows:  
   * **Left panel**: Thumbnail list of all screenshots  
   * **Right panel**: Large preview of the selected screenshot  
   * **Caption editor**: Text area below the preview

**Editing Actions:**

* **Select a screenshot**: Click its thumbnail to view full size  
* **Add a caption**: Type descriptive text in the caption editor (supports multiple lines)  
  * Example: "Dorsal view showing the parietal and frontal bones"  
* **Delete a screenshot**: Select it and click "Delete Screenshot"  
* **Reorder**: Not currently supported; delete and retake if needed  
3. Click **Save** to confirm your changes  
4. Click **Cancel** to discard all edits

**5.3.3 What Happens to Screenshots**

When the repository is created:

* Screenshots are uploaded to a /screenshots folder in the repository  
* Captions are saved to /screenshots/captions.json  
* The README.md automatically embeds images with their captions  
* Search results will show a screenshot icon and thumbnails in tooltips

> [!TIP]
> Screenshots are optional but highly recommended to give the users a sense of the data visually without having to download the full repository.

### **5.4 Execution**

* Click **Create Repository**.
* A **confirmation dialog** will appear displaying critical information about your repository:
  * **Volume dimensions** (in voxels)
  * **Voxel spacing** (resolution in mm)
  * **Image size** (file size)
  * **Specimen information** from the accession form
* **Review carefully:** This is your final chance to catch errors before the repository is created. Common issues to verify:
  * Spacing values look correct for your imaging modality
  * Dimensions match your expected volume size
  * Species name is spelled correctly
* If corrections are needed, click **Cancel** and return to previous steps to fix them. Otherwise, click **Proceed**.
* *Wait Time:* Depending on internet speed and volume size (max 2GB), this may take several minutes.   
* **Success:** The button un-grays, and the repo appears in your GitHub account.

> [!NOTE]
> **Terminologies for Short-term Repositories:** For **short-term repositories**, if your color table lacks complete terminology information, MorphoDepot can automatically assign default terminology entries to allow repository creation (every entry in the color table is assigned to Tissue type with the SnoMed CT (SCT) code 85756007). This is intended for quick classroom exercises or other short-term repositories where formal ontology linking is not necessarily required.

### Archival datasets: additional requirements

The steps above create any repository. **Archival** datasets (created inside the MorphoDepot organization) carry extra requirements, because once published they are citable and community-governed. Publishing is a **one-way door**: a private, staged dataset can be discarded freely, but a published one cannot be quietly deleted or hidden by the curator alone.

Before an archival dataset is made public it passes a short **editorial review** — in the spirit of a journal's handling editor (the PLOS ONE model): the editor confirms the dataset is **sound and well-formed**, not that the segmentation is anatomically perfect or important. When you request publication (or cut a release), the app automatically re-checks your latest commit — a **hard** check that fails is bounced straight back with a list of fixes, while a **soft** finding is flagged for the editor. Design your dataset to meet these from the start:

1. **Name it for the specimen** — `Genus-species` plus what the dataset contains (e.g., `Ariopsis-felis-cranium`), not `my-repo`, `project1`, or `test123`.
2. **Segment with real terminology** *(hard)* — a real terminology color table with populated ontology codes (Category, Type, Region); every segment named from it; names unique.
3. **Describe a real specimen** — a real species (checked against GBIF) and, if accessioned, a resolvable iDigBio record.
4. **Set a correct voxel size in millimeters** — a physically plausible scale; convert microns first (10 µm = 0.010 mm).
5. **Build any baseline on the source volume** *(hard)* — its reference geometry must match the scan, and its segments follow the terminology rules above.
6. **Provide a license and a screenshot** *(hard)* — CC BY 4.0 or CC BY-NC 4.0, and at least one screenshot that actually shows the data.

In return, archival datasets give contributors automatic credit (verified ORCID name and affiliation for organization members) and citable Zenodo DOIs on every release.

> [!NOTE]
> The authoritative, up-to-date requirements are the **[Community Guidelines for MorphoDepot Archival Datasets](https://github.com/MorphoDepot/docs/blob/main/MorphoDepot-archival-guidelines.md)**.

---

[⬅ Overview](./README.md)  ·  [⬅ Prev: Preparing Data: Color Table](./4-color-table.md)  ·  [Next: Project Management & Assignments ➡](./6-project-management.md)
