_MorphoDepot Tutorial · Part 5 of 10 — Creating the Repository_

[⬅ Overview](./README.md)  ·  [⬅ Prev: Preparing Data: Color Table](./4-color-table.md)  ·  [Next: Project Management & Assignments ➡](./6-project-management.md)

---

## **5. Creating the Repository**

The **Create** tab turns your prepared volume and color table into a MorphoDepot repository. It always happens in **two steps**:

1. **Stage** — MorphoDepot uploads your scan and creates a **private** repository from it. Nothing is public, nothing is discoverable, and you can come back to it as often as you like.
2. **Publish** — you make it public. *How* that works depends on the repository **type** you pick first.

Open the **MorphoDepot** module and select the **Create** tab.

### Repository type: Personal vs. Archival

Your first choice, at the top of the form, is where the repository lives. It is **permanent**: the two types live in different places, so switching later means discarding and starting over.

|  | **Personal** | **Archival** |
| --- | --- | --- |
| Who can create | Anyone | MorphoDepot **organization members** only |
| Where it lives | Your own GitHub account | The **MorphoDepot organization** |
| Scan storage | GitHub attachment, up to **2 GB** | Jetstream2 cloud storage, up to **10 GB** |
| Citable with a DOI | No | **Yes** — minted through Zenodo at each release |
| Publishing | Directly, no review | Editorial review before it goes public |
| Governance | Yours alone; delete anytime | Community-governed; publishing is a one-way door |
| Releases | Not applicable | Versioned, citable [releases](./9-releases.md) |

**When to choose Personal.** Any time you want the repository to stay entirely under your own control. A course exercise or a workshop dataset that you will delete at the end of term is the most common case, but it is not the only one — you may simply not be an organization member yet, you may be trying the workflow out, the data may not be yours to deposit permanently, or you may want to keep the ability to rename, unpublish, or delete the repository at will. Personal repositories are ordinary MorphoDepot repositories: they are searchable, other people can be assigned issues on them, and the whole segment/review/merge cycle works exactly the same.

**When to choose Archival.** When the dataset is meant to last and to be cited — a published study, a reference dataset, anything you want other people to build on over years. Archival datasets are the ones that get DOIs and shared stewardship, and that is also what they cost: once public, you cannot quietly delete or hide one.

> [!NOTE]
> The **Archival** option is disabled if MorphoDepot can confirm you are not an organization member; hovering it explains why. You can create a **Personal** repository right away, or join the organization from the [MorphoDepot landing page](https://morphodepot.org).

### **5.1 Before you start**

Have all of this ready in the scene *before* you open the Create tab:

* A **source volume** — cleaned, posed, cropped, with correct spacing in mm, and named with letters, digits, `.`, `-`, `_` only ([Part 3](./3-prepare-volume.md)).
* A **color table** — same naming rule; for an archival repository it must be a real terminology table ([Part 4](./4-color-table.md)).
* *(Optional)* A **baseline segmentation** built on that same volume, if you want contributors to start from existing work.

### **5.2 What staging freezes, and what you can still change**

This is the part that catches people out, so it is worth stating plainly. When you click **Create (stage privately)**, MorphoDepot saves the volume, uploads it, creates the private repository, pushes the metadata, and then **deletes its local working copy** — the multi-gigabyte scan does not linger on your disk.

**Frozen at staging — changing any of these means discarding and starting over:**

* the **source volume** (the scan);
* the **voxel dimensions and spacing** read from it — they are copied into the repository's metadata and never re-read;
* the **repository name** (the field becomes read-only when you reopen the repository);
* the **repository type** (the selector is hidden while you are editing a staged repository);
* the **owner** — your account, or the organization.

**Editable for as long as the repository is staged — reopen, change, click Save Changes, repeat:**

* every answer on the **accession form** — species, sex, developmental stage, modality, contrast, image contents, anatomical areas, specimen record URL;
* the **license** (the `LICENSE.txt` file is rewritten to match);
* the **color table** — by selecting a *different* color table node;
* the **baseline segmentation** — by selecting a *different* segmentation node;
* the **screenshots** — add, delete, or re-caption them.

> [!IMPORTANT]
> The color table and the baseline segmentation are **replaced**, not edited in place. If you reopen a staged repository and edit the loaded baseline segmentation in the scene, MorphoDepot refuses to save it and asks you to prepare the finished segmentation separately and load it as a new node. Same for the color table: fix it, load it, and select it.

Each **Save Changes** rewrites the staged repository as a single clean commit, so the published history stays tidy — as if you had created it correctly the first time.

### **5.3 Creating a Personal repository**

1. **Repository type** — select **Personal**.
2. **Subject Data** — choose your **Source volume** and a **Color table**; a **Baseline segmentation** is optional. See the [accession-form reference](#56-reference-the-accession-form).
3. **Screenshots** *(optional, recommended)* — capture a few views. See the [screenshot reference](#57-reference-screenshots).
4. **Auto-assign** *(optional, on by default)* — leave "Set the GitHub workflow to auto-assign new issues to their creators" ticked so that students' issues are assigned to them automatically. It works the same for either repository type, and needs the `workflow` scope on your GitHub login. See [Part 6](./6-project-management.md#63-assigning-issues-owner-action).
5. **Accession Form** — fill in the specimen metadata and choose a **license**. MorphoDepot suggests a repository name from what you enter (e.g. `mus-musculus-microct-whole`) and tells you whether that name is free on your account; edit it if you like.
6. **Stage** — click **Create (stage privately)** and review the confirmation dialog: destination, repository name, volume, color table, specimen details, and the computed physical size, voxel dimensions and spacing. Click **OK**.
7. MorphoDepot uploads the scan, creates the private repository, and resets the scene and the form. You are told where to find it again — the **Staged repositories — not yet published** list at the top of the Create tab.
8. **Publish** — double-click it in that list to reopen it. The **Make Repository Public** section appears at the bottom of the tab. Enter a **contact email** — MorphoDepot pre-fills it from your GitHub account when it can read it, and you can change it — then click **Publish**. It is used to reach you about MorphoDepot, is submitted only at publish (never for a repository you discard), and is not stored in the repository. Organization members are not asked: their address is already on file from ORCID onboarding.
9. The repository becomes **public on your account immediately** — no review — its GitHub page opens in your browser, and it becomes discoverable in [Search](./8-search.md) once the index picks it up.

> [!NOTE]
> **Relaxed terminology for personal repositories:** if your color table lacks complete terminology information, MorphoDepot offers to fill in default terminology entries so the repository can still be created (every entry is assigned to Tissue type with the SNOMED CT code `85756007`). This is intended for quick classroom exercises where formal ontology linking is not required. Archival repositories do not get this shortcut.

### **5.4 Creating an Archival repository**

An **archival** repository lives in the **MorphoDepot organization**, is stored on dedicated cloud storage, and is **community-governed**. It becomes **citable with a DOI** at its first release that contains a segmentation.

> [!IMPORTANT]
> Only MorphoDepot **organization members** can create archival datasets — join from the [MorphoDepot landing page](https://morphodepot.org). Publishing one is a **one-way door**: while it is private and staged you can discard it freely, but once it is **public you cannot quietly delete or hide it** — removal becomes an organization-level action. That durable commitment is what makes its DOI meaningful.

1. **Repository type** — select **Archival**. *(Disabled if you are not an organization member.)*
2. **Subject Data** — choose your **Source volume** and a **real terminology color table**. A generic or built-in Slicer color table (Labels, GenericAnatomyColors, a continuous colormap, …) is **rejected**, and so is a table with any entry missing its terminology. If you include a baseline segmentation, it must be built on **this** source volume.
3. **Accession Form** — fill in the specimen metadata, choose a **license**, set a **repository name**, and tick the **redistribution acknowledgement** in Section 6 (archival only). If the species name does not resolve cleanly in GBIF you get an advisory warning — it never blocks staging, but a reviewer may follow up.
4. **Screenshots** *(required)* — include at least one screenshot that actually shows the data (and the segmentation, if present).
5. **Auto-assign** *(optional, on by default)* — exactly as for a personal repository: leave "Set the GitHub workflow to auto-assign new issues to their creators" ticked so contributors' issues are assigned to them automatically. The option is independent of the repository type.
6. **Stage** — click **Create (stage privately)** and confirm. The dialog names the organization explicitly, so there is no doubt about where the repository is going. It is staged **privately inside the organization** and appears in the **Staged repositories** list.
7. **Submit for review** — reopen it and click **Publish**:
   * If your dataset ships a **baseline segmentation**, you are asked *"Who made the baseline segmentation?"* so that people other than yourself can be credited. You are added as lead author automatically; click **Done** if there is no one else to add. Cancelling here cancels the publish.
   * MorphoDepot then runs automated quality controls. If a **hard check fails** you get the specific list of things to fix and the repository stays staged: fix them, **Save Changes**, and click **Publish** again.
   * If the checks pass, a review request is emailed to the MorphoDepot reviewers and the entry in the staged list shows **⏳ pending review**.
8. **Finish the publish** — when a reviewer approves, you get an email and the entry reads **✓ approved — right-click ▸ Publish**. **Right-click it and choose Publish (make public)**. That final flip is yours to make; do it **within 14 days** of approval.
9. The dataset is now public inside the organization, and a DOI is minted. Further DOIs follow at each [release](./9-releases.md).

> [!WARNING]
> Do **not** reopen an approved repository to edit it. MorphoDepot deliberately refuses — editing would change the reviewed content and invalidate both the review and the DOI. If something really must change after approval, edit it and submit for review again.

**Archival requirements (checked automatically).** Publishing an archival dataset passes a short **editorial review** — in the spirit of a journal's handling editor (the PLOS ONE model): the editor confirms the dataset is **sound and well-formed**, not that the segmentation is anatomically perfect or important. When you publish (or cut a release), the app re-checks your latest commit — a **hard** check that fails is bounced straight back with a list of fixes, while a **soft** finding is flagged for the editor. Design your dataset to meet these from the start:

1. **Name it for the specimen** — `Genus-species` plus what the dataset contains (e.g., `Ariopsis-felis-cranium`), not `my-repo`, `project1`, or `test123`.
2. **Segment with real terminology** *(hard)* — a real terminology color table with populated ontology codes (Category, Type, Region); every segment named from it; names unique.
3. **Describe a real specimen** — a real species (checked against GBIF) and, if accessioned, a resolvable public collection record.
4. **Set a correct voxel size in millimeters** — a physically plausible scale; convert microns first (10 µm = 0.010 mm).
5. **Build any baseline on the source volume** *(hard)* — its reference geometry must match the scan, and its segments follow the terminology rules above.
6. **Provide a license and a screenshot** *(hard)* — CC BY 4.0 or CC BY-NC 4.0, and at least one screenshot that actually shows the data.

In return, archival datasets give contributors automatic credit (verified ORCID name and affiliation for organization members) and citable Zenodo DOIs on every release.

> [!NOTE]
> The authoritative, up-to-date requirements are the **[Community Guidelines for MorphoDepot Archival Datasets](https://github.com/MorphoDepot/docs/blob/main/MorphoDepot-archival-guidelines.md)**.

### **5.5 Managing staged repositories**

The **Staged repositories — not yet published** list at the top of the Create tab is your safety net. It is read live from GitHub, not from anything stored on your computer, so it works after a crash, after closing Slicer, and from a different machine.

* **Double-click** an entry to reopen it: MorphoDepot clones it, downloads the scan, pre-fills the accession form from what was submitted, and reloads the color table, baseline segmentation, and screenshots. The section header changes to *"Editing staged repository: …"*.
* **Save Changes** applies your edits without publishing. Do it as many times as you need.
* **Right-click** an entry for *Open the private repo in browser*, *Load the repo to edit*, or — for an approved archival dataset — *Publish (make public)*.
* **Refresh** re-queries GitHub.
* **Discard** abandons a staged repository. For a personal repository MorphoDepot opens its GitHub **Settings** page so you can delete it yourself from the Danger Zone (MorphoDepot never asks for permission to delete your repositories); for an organization repository it is marked as discarded, its stored scan is released, and it disappears from the list.

> [!NOTE]
> **Housekeeping.** A staged repository is not free — its scan is already uploaded. If yours is still unpublished after a week you get a reminder email, and unfinished staged repositories may be removed by a MorphoDepot administrator after four weeks. Publish it or discard it.

> [!TIP]
> If the exact same scan (byte-for-byte) is already in another MorphoDepot repository, the staged status line says so and you are asked to confirm before publishing. That is usually a sign of an accidental duplicate.

### **5.6 Reference: the accession form**

Both repository types use the same accession form. Fill out the metadata — this generates the `README.md` for the repository and the `MorphoDepotAccession.json` file behind [Search](./8-search.md).

* **Subject Type:** Biological specimen vs. Other. Choosing *Other* replaces the specimen sections with a free-text description.
* **Acquisition type:** Non-accessioned, or from an accessioned specimen (a natural history collection).
* **Accessioned specimen:** paste the **record URL** from a public database — GBIF, iDigBio, or Arctos are all understood, and MorphoDepot extracts the Darwin Core identifier (e.g. `UWBM:Mamm:82522`) for the README. A missing or unrecognized URL never blocks staging.
* **Species information:** enter **Genus species** (two words). Use **Search taxon in GBIF** to look one up and insert the exact spelling, plus **sex** and **developmental stage**.
* **Image data description:** modality, contrast staining, and whole vs. partial specimen (a partial specimen also asks which anatomical areas are present).
* **Licensing:**
  * *CC BY 4.0 (preferred):* Open, attribution required.
  * *CC BY-NC 4.0:* Open, attribution required, non-commercial only.
  * Archival repositories additionally require the redistribution acknowledgement.
* **Github:** the **repository name** — suggested from your metadata, editable, and checked for availability as you type. Keep it short, and name it for the specimen.

### **5.7 Reference: screenshots**

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
