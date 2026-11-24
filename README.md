# Family tree

A small, single-page family tree builder. Add relatives, link them as parents, partners and children, and explore the family in five different views. It's one `index.html` file with no build step, hosted on GitHub Pages.

**Live site:** `https://<your-username>.github.io/family-tree/`

## How the data stays private

This repo is public because GitHub Pages on a free account only publishes from public repos. That's fine, because **this repo contains no family data**: it's only the app.

The tree itself lives in a separate **private** repo, [`family-tree-data`](https://github.com/<your-username>/family-tree-data), as a single `family.json` file. The app reads and writes that file through the GitHub API using a fine-grained access token that you paste into the app. That token works as your login:

- Without a token, anyone visiting the site sees an empty "start your tree" screen.
- The token is stored only in the browser you paste it into (in `localStorage`). It is never committed anywhere.
- **Settings and storage → Sign out on this device** removes the token and the cached copy of the tree from that browser.

The page also sets `noindex, nofollow`, so search engines are asked not to list it.

## Setting it up

### 1. Publish the app

1. Put `index.html` in the root of this repo.
2. Go to **Settings → Pages**.
3. Under "Build and deployment", choose **Deploy from a branch**, branch `main`, folder `/ (root)`, and save.
4. After a minute or so the site is live at the URL above.

### 2. Create the private data repo

1. Create a new **private** repo called `family-tree-data`.
2. Tick **Add a README file** so the repo has a `main` branch. The app can't create the first commit in an empty repo.

You don't need to create `family.json`. The app creates it the first time it saves.

### 3. Create an access token

1. On GitHub, go to **Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token**.
2. **Repository access:** choose *Only select repositories* and pick `family-tree-data`.
3. **Permissions → Repository permissions → Contents:** set to *Read and write*. Leave everything else at *No access*.
4. Set an expiry you're comfortable with. When it expires, generate a new one and paste it into the app again.
5. Copy the token and keep it in your password manager. You'll need it on each new device or browser.

### 4. Connect the app

1. Open the live site and choose **Connect GitHub storage**. Later, you can find it under **⋯ → Settings and storage**.
2. Enter your GitHub username, `family-tree-data`, branch `main`, file `family.json`, and the token.
3. Choose **Connect and load**.

If the repo already has a tree, the app loads it. If this browser already has people in it and the repo is empty, the app saves them to the repo.

## Using it

- **Add someone:** use **Add person**. It's quicker to open a relative's profile and use **Add parent**, **Add partner**, **Add child** or **Add sibling**. The new person is linked to that relative automatically.
- **Edit or delete:** select a person, then use **Edit details** or **Delete** in the side panel. Deleting someone removes their links to relatives, but not the relatives themselves.
- **Move around diagrams:** drag to pan, and scroll or pinch to zoom. **Fit** shows the whole diagram.
- **Home person:** mark someone as the **home person** to make them the starting point for the Ancestors view.

### Views

| View | What it shows |
| --- | --- |
| Family tree | Everyone, arranged by generation, with couples side by side |
| Ancestors | A pedigree chart for any chosen person, with "+ Add parent" slots where a parent is missing |
| Descendants | Everyone descended from a chosen person, with their partners |
| Timeline | Lifespans on one shared time axis, to see who overlapped with whom |
| People | A searchable, sortable directory with living and deceased filters |

Each view has its own address (`#tree`, `#ancestors`, `#descendants`, `#timeline`, `#people`), so you can bookmark one directly.

### Saving

- By default, each change is committed to `family-tree-data` a couple of seconds after you make it. The status in the header shows whether everything is saved.
- To save only when you choose to, turn off **Save to GitHub automatically**. Then use **Save now** when it appears.
- If the file was changed from another device since this page loaded it, the app asks whether to keep your version or load GitHub's.

### Backups

- **⋯ → Download backup (JSON)** saves a copy of the whole tree.
- **⋯ → Import from backup** replaces the current tree with a backup file.
- Because every save is a commit, the data repo's history is also a complete backup.

## Running it locally

Open `index.html` in a browser. Nothing needs installing. Without a GitHub connection, the tree is saved in that browser's local storage. **⋯ → Load sample family** fills in a fictional family, which is useful for trying out the views.

## Privacy notes

- Don't commit `family.json` or any exported backups to *this* repo. It's public.
- Anyone who uses your browser while you're signed in can see the tree, the same as any logged-in site. Sign out on shared computers.
- If a token is ever exposed, delete it on GitHub (**Settings → Developer settings → Personal access tokens**). Then create a new one.

## Tech

- Plain HTML, CSS and JavaScript in one file. No framework, no build step, no dependencies apart from Google Fonts.
- Talks to the GitHub REST API's [repository contents endpoints](https://docs.github.com/en/rest/repos/contents) directly from the browser.
- Photos are cropped to small squares and stored inside the JSON as data URLs, so the whole tree is one file.
