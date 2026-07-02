_MorphoDepot Tutorial · Part 10 of 10 — Collections_

[⬅ Overview](./README.md)  ·  [⬅ Prev: Releases & DOIs](./9-releases.md)

---

## **10. Collections**

A **collection** groups existing MorphoDepot repositories under a common theme — a "repo of repos." Examples: *Snakes of Texas*, *Mammal Skulls of the PNW*, *Developing Mouse Embryos*. A collection does not copy any data; it simply **points at member repositories**, and RepoClerk renders it as a browsable gallery of those datasets.

Everything below happens in the **Collections** tab of the MorphoDepot module (the right-most tab).

> [!NOTE]
> **Browsing** collections is open to everyone. **Creating** a collection requires MorphoDepot **organization membership**, because the collection itself lives in the organization. If you are not a member, the *Create a Collection* section is disabled; you can join the organization from the [MorphoDepot landing page](https://morphodepot.org).

### 10.1 Browse existing collections

1. Open the **Collections** tab.
2. Click **Refresh** to load the existing collections (this also loads the list of repositories you can add to a new one).
3. Each entry shows the collection's **title**, how many **member repositories** it has, and its **curator**.
4. **Double-click** a collection to open its page on GitHub.

### 10.2 Create a collection (organization members)

1. In the **Create a Collection** section, enter a **Title** — the display name, e.g. `Snakes of Texas`. A short repository name is derived from it automatically. Optionally add a one-line **Description**.
2. Add member repositories — **at least two**:
   * Pick a repository from the dropdown (it lists the known MorphoDepot corpus, with species names), **or** paste a GitHub URL, then click **Add**.
   * Repeat to add more. Use **Remove selected** to drop any you added by mistake.
3. Click **Create Collection**.

What happens: MorphoDepot uses your own `gh` to create a new repository **in the MorphoDepot organization**, seeds it with a canonical `README.md` and a `CURATOR` file (naming you as curator), and tags it `md-collection`. RepoClerk then picks it up and renders it as a gallery of its member datasets.

> [!TIP]
> A collection's `README.md` is curated free-form. After creating it, you can edit it on GitHub to add context, section headings, or a narrative. Because membership is just a set of pointers, adding or removing datasets later is simply an edit to the collection repository — the member datasets are never modified.

---

[⬅ Overview](./README.md)  ·  [⬅ Prev: Releases & DOIs](./9-releases.md)
