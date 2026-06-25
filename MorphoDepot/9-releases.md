_MorphoDepot Tutorial · Part 9 of 9 — Releases & DOIs_

[⬅ Overview](./README.md)  ·  [⬅ Prev: Search & Discovery](./8-search.md)

---

## **9. Releases & DOIs**

> [!IMPORTANT]
> Releases are an **archival-only** feature. Only datasets created inside the **MorphoDepot organization** can be released, and a release is what mints a **citable DOI**. **Short-term (personal) repositories have no release cycle** — there is nothing to do here for them.

A **release** is a permanent, versioned snapshot of an archival dataset's segmentation. Each release is minted a citable **Zenodo DOI**, so the dataset — and everyone who contributed to it — can be cited. Releases are cut by the dataset's **curator** (its creator) at project milestones, consolidating the contributions accepted since the last release into a new canonical baseline segmentation. The underlying scan (`source_volume`) never changes between releases; only the segmentation layer — and, optionally, the color table — does.

Everything below happens in the **Release** tab of the MorphoDepot module, in three phases.

### 9.1 Announce a deadline (call for final contributions)

1. Open the **Release** tab and load the repository you curate.
2. In the **Pre-release Announcement** section, set a **deadline** and post the announcement. This notifies everyone with an open or in-progress issue that a release is coming, so they can finish their work and sync.
3. It also opens the window for contributors to confirm how they should be credited in the release.

### 9.2 Curate and build the new baseline

1. The tab lists the segmentation contributions merged since the last release — each `issue-N` with its anatomical structure, the contributor, and the merge date.
2. **Check the ones to include.** If any pull requests or issues are still open, you are warned — those cannot be part of this release, so resolve or defer them first.
3. Click **Load selected for merging**. MorphoDepot assembles the checked segmentations into a new `baseline` segmentation and opens it in **Segment Editor** with the source volume, just as contributors see it.
4. Clean up the baseline: rename segments to canonical terminology, fix boundaries, and resolve any duplicate-named segments (e.g. a structure segmented in two different issues).
5. *(Optional)* Load an updated **color table** — a superset of the previous one that adds any new terms — so future contributors have an entry for every structure.
6. Contributor credit for this cycle is gathered automatically; review and curate it before publishing.

> [!TIP]
> When you merge, rather than editing the loaded baseline in place, **clone** the repository's current baseline, **copy** the accepted issue segments into the clone, and select the clone as the new baseline. This keeps the original pristine as a reference and makes the change unmistakable.

### 9.3 Make the release (review → tag → DOI)

1. Select the **new baseline segmentation** and the **color table** (both required), optionally **take screenshots**, and click **Make Release**.
2. A **no-change guard** checks that the baseline actually differs from the last release, so an accidental empty release is caught before anything is published.
3. Because archival releases are citable, the release goes through the same **editorial review** as publication: it is submitted to the MorphoDepot reviewers, who **Approve**, **Inspect**, or **Request changes**. Until it is approved, `main` is untouched and the release does not yet exist.
4. **On approval,** MorphoDepot tags the release (`v1`, `v2`, `v3`, …), **mints the DOI through Zenodo**, and regenerates the repository's front-page `README.md` with a DOI badge and a ready-to-paste citation.

### 9.4 After a release

* Each release gets its own **version DOI**; a single **concept DOI** always resolves to the latest version. Both appear on the dataset's README with the citation block.
* Contributors with work in flight should **sync their fork** to the new baseline before submitting (`git fetch upstream && git merge upstream/main`). MorphoDepot posts reminders on the affected issues and pull requests.
* Rejected (unchecked) contributions stay in the repository as a historical record; the release notes you write should summarize what was added and who contributed.

> [!NOTE]
> Releases, DOIs, and contributor credit are part of the archival workflow. For the full requirements and policy, see the **[Community Guidelines for MorphoDepot Archival Datasets](https://github.com/MorphoDepot/docs/blob/main/MorphoDepot-archival-guidelines.md)**.

---

[⬅ Overview](./README.md)  ·  [⬅ Prev: Search & Discovery](./8-search.md)
