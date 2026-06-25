_MorphoDepot Tutorial · Part 5 of 8 — Creating the Repository_

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
| Scan storage | GitHub attachment, up to 2 GB | Jetstream2 cloud storage (10 GB by default, up to 1 TB) with off-site backup |
| Citable with a DOI | No | **Yes** — minted through Zenodo at each release |
| Governance | Yours alone; delete anytime | Community-governed (see below) |
| Best for | Teaching, drafts, experiments | Lasting, citable datasets |

* **Short-term** repositories are for teaching demonstrations, workshops, or temporary projects. Terminology validation is relaxed and they are meant to be ephemeral. If a short-term repository becomes useful and informative, consider creating an archival one from that point on.
* **Archival** repositories are for research publications and datasets meant for long-term citation. They live in the MorphoDepot organization, are stored on dedicated cloud storage, and become **citable with a DOI** at their first release that contains a segmentation. Creating archival datasets requires joining the organization at [join.morphodepot.org](https://join.morphodepot.org).

> [!IMPORTANT]
> Creating an archival dataset is a **one-way door**. While it is still private and being prepared you can discard it freely, but once it is **published you cannot quietly delete or hide it** — removal becomes an organization-level action, not something the curator does alone. That durable commitment is what makes the DOI meaningful. If you want something you can delete at any time, use a short-term repository instead.

### Archival datasets: additional requirements & the publication review

Because archival datasets are held in trust for the community, each one passes a short **editorial review** before it is made public — in the spirit of a journal's handling editor (the PLOS ONE model). The editor confirms the dataset is **sound and well-formed**; they do *not* judge whether the segmentation is anatomically perfect or scientifically important. Accuracy stays the community's ongoing job and improves over time.

When you request publication (or cut a release), the app **automatically re-checks your latest commit**. A **hard** check that fails sends the request straight back to you with a list of fixes — no reviewer is involved. A **soft** finding does not block; it is flagged for the editor to look at. Design your dataset to meet these from the start to avoid round-trips:

1. **Name it for the specimen.** Use `Genus-species` plus what the dataset contains (e.g., `Ariopsis-felis-cranium`), not `my-repo`, `project1`, or `test123`. The name is permanent and public — it is how others find and cite the dataset.
2. **Segment with real terminology** *(hard check)*. The color table must be a real terminology table with populated ontology codes (Category, Type, and Region) — a plain or default 3D Slicer color table is rejected. Every segment must be named from that table, and segment names must be unique.
3. **Describe a real specimen.** The accession form must record a real species (checked against GBIF) and, if the specimen is accessioned, a resolvable iDigBio record.
4. **Set a correct voxel size in millimeters.** The scale must be physically plausible. Many scans are acquired in microns — convert before you set it (a 10 µm voxel = 0.010 mm).
5. **Build any baseline on the source volume** *(hard check)*. A dataset may publish as a bare scan with no segmentation, but any baseline you include must be built on the dataset's own source volume (its reference geometry must match the scan) and must follow the terminology rules above.
6. **Provide a license and a screenshot** *(hard check)*. Choose CC BY 4.0 or CC BY-NC 4.0, and include at least one screenshot that actually shows the data (and the segmentation, if the dataset has one).

In return, archival datasets give contributors **automatic credit** (organization members by their verified ORCID name and affiliation, others by GitHub handle) and **citable Zenodo DOIs** on every release — a version DOI per release plus a concept DOI that always resolves to the latest, added to the repository's front-page README automatically.

> [!NOTE]
> The requirements above are summarized for this tutorial. The authoritative, up-to-date version is the **[Community Guidelines for MorphoDepot Archival Datasets](https://github.com/MorphoDepot/docs/blob/main/MorphoDepot-archival-guidelines.md)**.

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

---

[⬅ Overview](./README.md)  ·  [⬅ Prev: Preparing Data: Color Table](./4-color-table.md)  ·  [Next: Project Management & Assignments ➡](./6-project-management.md)
