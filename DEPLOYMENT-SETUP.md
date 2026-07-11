# The VOV Standard Deployment Setup

This setup separates `vovstandard.com` from VOV Verified ownership.

## Critical HTTPS requirement (read first)

`vovstandard.com` and `www.vovstandard.com` must both terminate on the VOV Standard Pages site.

Use these DNS records:

- `@` A -> `185.199.108.153`
- `@` A -> `185.199.109.153`
- `@` A -> `185.199.110.153`
- `@` A -> `185.199.111.153`
- `@` AAAA -> `2606:50c0:8000::153`
- `@` AAAA -> `2606:50c0:8001::153`
- `@` AAAA -> `2606:50c0:8002::153`
- `@` AAAA -> `2606:50c0:8003::153`
- `www` CNAME -> `vovstandard.com`

Do not point `www` to `offgridtv.github.io` for this setup. That can leave `www.vovstandard.com` on a default `*.github.io` certificate and cause HTTPS principal mismatch errors.

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

## 6) Verify HTTPS for both hostnames

Run these checks after each deploy or DNS change:

```bash
curl -I https://vovstandard.com/
curl -I https://www.vovstandard.com/
```

Expected:

- `https://vovstandard.com/` returns `200 OK`
- `https://www.vovstandard.com/` returns `301` to `https://vovstandard.com/` without TLS/certificate errors

If `www` fails TLS validation, fix DNS first (`www` CNAME -> `vovstandard.com`) and then re-check GitHub Pages "Enforce HTTPS".

## 7) Verify artifact content in vovstandard-deploy

Confirm files in root of deploy repository:

- `index.html`
- `style.css`
- `assets/EVAI Logo.svg`
- `assets/VOV landing page image 7-26.png`
- `CNAME` with value `vovstandard.com`
- `.nojekyll`
