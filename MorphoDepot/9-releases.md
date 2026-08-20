_MorphoDepot Tutorial · Part 9 of 10 — Releases & DOIs_

[⬅ Overview](./README.md)  ·  [⬅ Prev: Search & Discovery](./8-search.md)  ·  [Next: Collections ➡](./10-collections.md)

---

## **9. Releases & DOIs**

> [!IMPORTANT]
> If you are not a member of the MorphoDepot organization, the **Release** tab is disabled for you.
>
> Releases are an **archival-only** feature. The Release tab lists exactly the repositories **in the MorphoDepot organization that you curate** — a personal repository is never listed and has no release cycle, so there is nothing to do here for one.

A **release** is a permanent, versioned snapshot of an archival dataset's segmentation. Each release is minted a citable **Zenodo DOI**, so the dataset — and everyone who contributed to it — can be cited. Releases are cut by the dataset's **curator** (its creator) at project milestones, consolidating the contributions accepted since the last release into a new canonical baseline segmentation. The underlying scan (`source_volume`) never changes between releases; only the segmentation layer — and, optionally, the color table — does.

> [!NOTE]
> Every repository ships with a `v1` tag from the moment it is created — that is the accession snapshot, not a curated release. Your first curated release will therefore be **v2**.

Everything below happens in the **Release** tab of the MorphoDepot module, in three phases.

### 9.1 Announce a deadline (call for final contributions)

1. Open the **Release** tab, click **Refresh Github**, and double-click the repository you curate. Each row shows how many issues and pull requests are open, and hovering it lists them.
2. Open the **Pre-release Announcement** section, set a **deadline**, edit the message if you like (`{deadline}` is replaced with the date), and click **Notify contributors**.
3. MorphoDepot posts the message as a comment on **every open issue and pull request** — so anyone watching gets a notification — and creates a **pinned announcement issue** labelled `release-pending`, so anyone who merely visits the repository sees it too.

If an announcement already exists, the section header says so and shows the deadline, and the button becomes **Replace announcement**: setting a new deadline edits the existing announcement in place rather than creating a second one. The announcement is retired automatically (unpinned, unlabelled, closed) once the release is submitted.

### 9.2 Build the new baseline

Double-clicking the repository in the Release tab cloned its `main` branch and loaded **everything committed there** into the scene: the source volume, the color table, the current `baseline` segmentation, and one `issue-N` segmentation for every contribution merged since the last release. Assembling those into the next baseline is a manual, deliberate curation step — MorphoDepot does not merge them for you.

1. Work in **Segment Editor**, or the **Segmentations** module, to build the new baseline: bring the accepted `issue-N` segments together, rename them to canonical terminology, fix boundaries, and resolve duplicates (the same structure segmented in two different issues).
2. *(Optional)* Load an updated **color table** — a superset of the previous one that adds any new terms — so future contributors have an entry for every structure.
3. Check the **Release comments** box: MorphoDepot has already pre-filled it with the issues closed since the last release. Add your own summary above that change log.

> [!TIP]
> Rather than editing the loaded `baseline` in place, **clone** it in the Data module, **copy** the accepted issue segments into the clone, and select the clone as the new baseline. This keeps the original pristine as a reference and makes the change unmistakable.

### 9.3 Make the release (review → tag → DOI)

1. Select the **New baseline segmentation** and the **Color table** — both are required, and the baseline is deliberately *not* pre-selected, so that a release cannot accidentally re-publish the old baseline. Optionally **Take Screenshot** to add new images.
2. Click **Make release**. MorphoDepot checks the obvious mistakes first and asks you to confirm before continuing past any of them:
   * the new baseline is **identical** to the one already committed (so the release would contain no new work);
   * the baseline has **no segments**;
   * the color table you picked is **not a terminology table**, or is byte-identical to the committed one although you picked a different node;
   * **no pre-release announcement** was made, or its deadline has not passed yet.
3. **Contributor credit.** A grid opens listing everyone who contributed a merged `issue-N` pull request since the last release. Your own name, ORCID, and affiliation are filled in automatically as lead author; fill in names for outside contributors, tick **Author?** for anyone you want cited as a co-author, and add offline contributors by hand. Contributors without a name are credited by GitHub handle. This becomes `CONTRIBUTORS.json` and the Contributors section of the README.
4. A final summary lists exactly what the release will change — baseline, color table, README (the previous one is kept as `README-vN.md`), new screenshots, and the per-issue segmentation files that will be dropped from the working tree but kept in history. Confirm it.
5. **Review.** Because archival releases are citable, the release goes through the same **editorial review** as publication. The release commit is pushed to a `release-candidate-vN` branch and a review request is emailed to the MorphoDepot reviewers — **`main` is untouched and the release does not yet exist**.
6. You are then offered to **close the remaining open issues and pull requests**, since their work is already folded into the release commit.
7. **On approval**, MorphoDepot archives the current `main` as `pre-release-vN`, fast-forwards `main` to the candidate, creates the `vN` tag, **mints the DOI through Zenodo**, and regenerates the repository's front-page `README.md` with a DOI badge and a ready-to-paste citation. If changes are requested instead, you get an email with the details and nothing is published.

### 9.4 After a release

* Each release gets its own **version DOI**; a single **concept DOI** always resolves to the latest version. Both appear on the dataset's README with the citation block.
* Contributors do not need to run any git commands to catch up: the next time they load an assigned issue, MorphoDepot syncs their fork and starts the new branch from the released `main`. Work that was still open when the release was cut is closed with a comment asking them to open a new issue against the updated baseline.
* Nothing is lost. Contributions you did not fold into the new baseline remain in the repository's history, as do the previous baseline, the previous README, and the `pre-release-vN` branch.

> [!NOTE]
> Releases, DOIs, and contributor credit are part of the archival workflow. For the full requirements and policy, see the **[Community Guidelines for MorphoDepot Archival Datasets](https://github.com/MorphoDepot/docs/blob/main/MorphoDepot-archival-guidelines.md)**.

---

[⬅ Overview](./README.md)  ·  [⬅ Prev: Search & Discovery](./8-search.md)  ·  [Next: Collections ➡](./10-collections.md)
