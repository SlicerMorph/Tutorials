_MorphoDepot Tutorial · Part 5 of 10 — Creating the Repository_

[⬅ Overview](./README.md)  ·  [⬅ Prev: Preparing Data: Color Table](./4-color-table.md)  ·  [Next: Project Management & Assignments ➡](./6-project-management.md)

---

## **5. Creating the Repository**

The **Create** tab turns your prepared volume and color table into a MorphoDepot repository. Creation always happens in two steps — you first **stage** the repository privately, then **publish** it — but *how* you publish depends on the repository **type** you pick first.

1. Open the **MorphoDepot** module and select the **Create** tab.

### Repository type: Personal (short-term) vs. Archival

Your first choice is the repository **type**, and it is **permanent** — it cannot be changed after the repository is staged (switching would mean moving between your personal account and the organization).

|  | Personal (short-term) | Archival |
| --- | --- | --- |
| Who can create | Anyone | MorphoDepot **organization members** only |
| Where it lives | Your personal GitHub account | The **MorphoDepot organization** |
| Scan storage | GitHub attachment, up to 2 GB | Jetstream2 cloud storage (10 GB by default) |
| Citable with a DOI | No | **Yes** — minted through Zenodo at each release |
| Review to publish | None — publishes directly | Editorial review before it goes public |
| Governance | Yours alone; delete anytime | Community-governed |
| Best for | Teaching, drafts, experiments | Lasting, citable datasets |

> [!NOTE]
> The **Archival** option is available only to MorphoDepot **organization members**. If you are not a member it is disabled, and you are guided to create a **Personal** repository instead (or to join the organization from the [MorphoDepot landing page](https://morphodepot.org)).

The two sections below are **standalone walkthroughs** — follow the one that matches your choice. Both share the same accession form and screenshot tools; those field-by-field details are collected once in the [reference](#53-reference-the-accession-form) at the end so they are not repeated.

### **5.1 Creating a Personal (Short-term) Repository**

A **personal** repository lives on your own GitHub account. It needs no organization membership, you manage (or delete) it entirely on your own, it publishes directly with **no review**, and it is **not citable** (no DOI). Terminology rules are relaxed. Use it for teaching, workshops, drafts, and experiments.

1. **Repository type** — select **Short-term / Personal**.
2. **Subject Data** — choose your **Source Volume** and a **Color Table** (a baseline segmentation is optional). See the [accession-form reference](#53-reference-the-accession-form).
3. **Accession Form** — fill in the specimen metadata, choose a **license**, and set a **repository name**. See the [reference](#53-reference-the-accession-form).
4. **Screenshots** *(optional)* — capture a few views to showcase the data. See the [screenshot reference](#54-reference-screenshots).
5. **Stage** — click **Create (stage privately)**, review the confirmation dialog (dimensions, spacing, size, specimen info), and click **Proceed**. The repository is created **privately on your account** and appears in the **Unpublished staged repositories** list, where you can reopen it to edit or **Discard** it.
6. **Publish** — select it in that list and click **Publish**. It becomes **public on your personal account immediately** — no review, no DOI. *(If you are not an organization member, enter a contact email before Publish is enabled.)*

> [!NOTE]
> **Relaxed terminology for personal repositories:** if your color table lacks complete terminology information, MorphoDepot can automatically assign default terminology entries so the repository can still be created (every entry is assigned to Tissue type with the SNOMED CT code `85756007`). This is intended for quick classroom exercises where formal ontology linking is not required.

### **5.2 Creating an Archival Repository**

An **archival** repository lives in the **MorphoDepot organization**, is stored on dedicated cloud storage, and is **community-governed**. It becomes **citable with a DOI** at its first release that contains a segmentation. Use it for research publications and datasets meant for long-term citation.

> [!IMPORTANT]
> Only MorphoDepot **organization members** can create archival datasets — join from the [MorphoDepot landing page](https://morphodepot.org). Publishing one is a **one-way door**: while it is private and staged you can discard it freely, but once it is **public you cannot quietly delete or hide it** — removal becomes an organization-level action. That durable commitment is what makes its DOI meaningful.

1. **Repository type** — select **Archival**. *(Disabled if you are not an organization member.)*
2. **Subject Data** — choose your **Source Volume** and a **real terminology Color Table** (a plain or default Slicer color table is rejected). If you include a baseline segmentation, it must be built on **this** source volume. See the [accession-form reference](#53-reference-the-accession-form).
3. **Accession Form** — fill in the specimen metadata, choose a **license**, set a **repository name**, and complete the **redistribution acknowledgement**. See the [reference](#53-reference-the-accession-form).
4. **Screenshots** *(required)* — include at least one screenshot that actually shows the data (and the segmentation, if present). See the [screenshot reference](#54-reference-screenshots).
5. **Stage** — click **Create (stage privately)**, review the confirmation dialog, and click **Proceed**. The repository is staged **privately inside the organization** and appears in the **Unpublished staged repositories** list.
6. **Publish → editorial review** — select it and click **Publish** to submit for review. The staged entry shows **pending review**; once an editor approves it, it reads **✓ approved — click Publish to finish**, and clicking **Publish** again completes the flip to **public inside the organization**. The dataset is now citable — a DOI is minted at each [release](./9-releases.md).

**Archival requirements (checked automatically).** Publishing an archival dataset passes a short **editorial review** — in the spirit of a journal's handling editor (the PLOS ONE model): the editor confirms the dataset is **sound and well-formed**, not that the segmentation is anatomically perfect or important. When you publish (or cut a release), the app re-checks your latest commit — a **hard** check that fails is bounced straight back with a list of fixes, while a **soft** finding is flagged for the editor. Design your dataset to meet these from the start:

1. **Name it for the specimen** — `Genus-species` plus what the dataset contains (e.g., `Ariopsis-felis-cranium`), not `my-repo`, `project1`, or `test123`.
2. **Segment with real terminology** *(hard)* — a real terminology color table with populated ontology codes (Category, Type, Region); every segment named from it; names unique.
3. **Describe a real specimen** — a real species (checked against GBIF) and, if accessioned, a resolvable iDigBio record.
4. **Set a correct voxel size in millimeters** — a physically plausible scale; convert microns first (10 µm = 0.010 mm).
5. **Build any baseline on the source volume** *(hard)* — its reference geometry must match the scan, and its segments follow the terminology rules above.
6. **Provide a license and a screenshot** *(hard)* — CC BY 4.0 or CC BY-NC 4.0, and at least one screenshot that actually shows the data.

In return, archival datasets give contributors automatic credit (verified ORCID name and affiliation for organization members) and citable Zenodo DOIs on every release.

> [!NOTE]
> The authoritative, up-to-date requirements are the **[Community Guidelines for MorphoDepot Archival Datasets](https://github.com/MorphoDepot/docs/blob/main/MorphoDepot-archival-guidelines.md)**.

### **5.3 Reference: the accession form**

Both repository types use the same accession form. Fill out the metadata — this generates the `README.md` for the repository.

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
* **Repository Name:** Create a short, unique name (no spaces, e.g., `Project_FishSkull_2024`).

### **5.4 Reference: screenshots**

You can capture and annotate screenshots to showcase your dataset. These images are embedded in the repository's README and shown in search results.

**Taking screenshots**

1. In the Create tab, locate the **Screenshots** section.
2. Arrange your 3D/2D views to show interesting features of your data.
3. Click **Take Screenshot** — the current viewport is captured and saved with a numbered filename (`screenshot-1.png`, `screenshot-2.png`, …); a counter shows how many you have taken.

**Reviewing and editing screenshots**

1. Click **Review Screenshots** to open the review dialog, which shows a thumbnail list (left), a large preview (right), and a caption editor below the preview.
2. **Select** a screenshot to view it full size; **add a caption** in the editor (e.g., "Dorsal view showing the parietal and frontal bones"); **delete** with "Delete Screenshot". Reordering is not currently supported — delete and retake if needed.
3. Click **Save** to confirm, or **Cancel** to discard all edits.

When the repository is created, screenshots are uploaded to a `/screenshots` folder, captions are saved to `/screenshots/captions.json`, and the README embeds each image with its caption.

> [!TIP]
> Screenshots are optional (except for archival datasets, which require at least one) but highly recommended — they give users a sense of the data visually without having to download the full repository.

---

[⬅ Overview](./README.md)  ·  [⬅ Prev: Preparing Data: Color Table](./4-color-table.md)  ·  [Next: Project Management & Assignments ➡](./6-project-management.md)
