# Module 00: Setup & two-machine workflow

**Goal:** Get your own site (repo, content, branch previews) working, with code on this machine and content on the other.

## What's already done on this machine

- The boilerplate is cloned into `C:\learn\aem-eds-lab` with `npm ci` already run.
- The AEM CLI is installed globally (`aem --version`).
- The `npm run dev` script runs `aem up --html-folder drafts`.
- `drafts/` holds mock content for every module, and `.hlxignore` keeps `drafts/` and `tutorial/` from being served.
- `.claude/launch.json` defines `aem-dev`, so you can start the server from the Claude app's preview pane.

Right now `origin` points to `adobe/aem-boilerplate`, so `localhost:3000/` proxies Adobe's demo site. That's fine for Modules 1–8. Repoint it once your own site exists.

## Task 1: Create your site (browser, either machine)

**Why:** You need your own `main--<repo>--<owner>.aem.page` so that pushes deploy and content can preview.

1. On GitHub, open `adobe/aem-boilerplate`, choose **Use this template**, and create a **public** repo, for example `summit-outfitters`.
2. Install the **AEM Code Sync** GitHub App on that repo only.
3. On the setup screen, set the **content source** to a Google Drive folder. Create that folder on the content machine first.
4. On the content machine, share the Drive folder with the service account shown in the setup instructions so AEM can read it.

> The current boilerplate has no `fstab.yaml`. Content source, index, sitemap, headers and similar settings live in the **configuration service** (https://tools.aem.live/). Don't add `fstab.yaml` or `helix-query.yaml`.

## Task 2: Repoint this workspace to your repo

**Why:** You keep the prepared drafts and tutorial, but work in your own repo. A template repo has fresh history, so clone it and copy the lab files across.

```bash
cd /c/learn
git clone https://github.com/<owner>/summit-outfitters.git
cp -r aem-eds-lab/drafts aem-eds-lab/tutorial aem-eds-lab/.claude aem-eds-lab/.hlxignore aem-eds-lab/package.json summit-outfitters/
cd summit-outfitters && npm ci
git add -A && git commit -m "chore: add learning lab scaffolding" && git push
```

From here on, work in `C:\learn\summit-outfitters`. Run `npm run dev` and check that the log says `reverse proxy to https://main--summit-outfitters--<owner>.aem.page`.

## Task 3: Author the baseline content (content machine)

Create these items in the Drive folder, then **Preview** each one with Sidekick:

| Document | Contents |
|---|---|
| `index` | An H1, a paragraph, and a `Hero` block table |
| `nav` | Three sections: brand link, a list of links, and tools |
| `footer` | One paragraph |

Install the **AEM Sidekick** extension on the content machine.

## Task 4: Prove the branch-preview loop

**Why:** Reviewers get preview links per branch, and every PR needs one.

```bash
git switch -c m00-hello
# change --link-color in styles/styles.css
git commit -am "chore: hello branch" && git push -u origin m00-hello
```

Open `https://m00-hello--summit-outfitters--<owner>.aem.page/`. It shows your branch code with the published content.

## Done when

- [ ] `npm run dev` serves `http://localhost:3000/drafts/m01-anatomy`
- [ ] `localhost:3000/` shows **your** Google Doc content
- [ ] A branch URL shows your color change while `main` doesn't
- [ ] `npm run lint` passes

## Troubleshooting

| Symptom | Likely cause |
|---|---|
| 404 on `aem.page` after creating the repo | Code Sync isn't installed on that repo, or the content source isn't configured |
| Your Doc edits don't appear | You didn't click **Preview** in Sidekick. Edits aren't live until you do. |
| Local server shows the old demo content | `origin` still points to `adobe/aem-boilerplate` |
| Corporate proxy blocks `aem up` | Set `HTTPS_PROXY` before running it, or use `--html-folder` drafts only |
