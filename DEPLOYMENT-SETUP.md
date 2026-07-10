# The VOV Standard Deployment Setup

This setup separates `vovstandard.com` from VOV Verified ownership.

## 1) Create deploy repository

Create repository: `vovstandard-deploy`

- Empty repository
- Default branch: `main`
- Public visibility (recommended for GitHub Pages)

## 2) Configure GitHub Pages in vovstandard-deploy

In repository settings:

- Pages source: `Deploy from a branch`
- Branch: `main`
- Folder: `/ (root)`

## 3) Configure custom domain in vovstandard-deploy

In repository settings -> Pages:

- Custom domain: `vovstandard.com`
- Save

Do not change DNS in this step.

## 4) Configure VOV-ecosystem automation

In `VOV-ecosystem` repository settings:

- Add secret `DEPLOY_REPO_TOKEN`
  - Fine-grained token with `Contents: Read and write` for `vovstandard-deploy`
- Optional variable `DEPLOY_REPO_OWNER`
  - Set if deploy repository owner differs from `OffGridTV`

## 5) Deploy

Run the `Deploy VOV Standard` workflow for The VOV Standard site.

Result:

- Pushes static artifact to `vovstandard-deploy/main`
- Includes `CNAME` containing `vovstandard.com`
- Includes `.nojekyll`

## 6) Verify artifact content in vovstandard-deploy

Confirm files in root of deploy repository:

- `index.html`
- `style.css`
- `assets/EVAI Logo.svg`
- `assets/VOV landing page image 7-26.png`
- `CNAME` with value `vovstandard.com`
- `.nojekyll`
