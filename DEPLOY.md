# Deployment Guide — Power Capital

Step-by-step to publish this site to GitHub Pages as a **project site**
(`kalaroberts.github.io/powercapital/`) and then map the custom domain
**powercapital.cloud**.

> Note: `CNAME` has been temporarily renamed to `CNAME.txt` so you can verify
> the site at the GitHub Pages URL first. You will rename it back in Phase 3,
> Step 6.

---

## Phase 2 — Publish to GitHub Pages

### 1. Copy the site to a permanent folder
```bash
mkdir -p ~/powercapital
cp -R "/Users/kalaj/.quickwork/profiles/federate-prod/sessions/fb7b9789-b26f-4536-8250-0a32f7065820/workspace/artifacts/powercapital/." ~/powercapital
cd ~/powercapital
```

### 2. Create the repo on GitHub
- Go to https://github.com/new
- **Repository name:** `powercapital`  (this becomes the URL path)
- Visibility: **Public** (required for free GitHub Pages)
- Do NOT add a README, .gitignore, or license (already present)
- Click **Create repository**

### 3. Push the files
```bash
git init
git add .
git commit -m "Initial commit — Power Capital site"
git branch -M main
git remote add origin https://github.com/kalaroberts/powercapital.git
git push -u origin main
```
(Swap `kalaroberts` if your GitHub username differs.)

### 4. Enable GitHub Pages
- Repo → **Settings → Pages**
- **Source:** Deploy from a branch
- **Branch:** `main`  **Folder:** `/ (root)` → **Save**
- Wait ~1–2 minutes.

### 5. Verify at the GitHub Pages URL
Visit: **https://kalaroberts.github.io/powercapital/**
- Click through all pages; check logo, favicon, fonts.
- Test the **contact form + hCaptcha** here (first submission activates
  Web3Forms — confirm the activation email, then the form delivers).

---

## Phase 3 — Connect powercapital.cloud

### 6. Re-enable the CNAME file
```bash
cd ~/powercapital
git mv CNAME.txt CNAME
git commit -m "Enable custom domain CNAME"
git push
```
(The CNAME file contains exactly: `powercapital.cloud`)

### 7. Add DNS records at your registrar
**Apex domain (`powercapital.cloud`) — four A records:**
```
A   @   185.199.108.153
A   @   185.199.109.153
A   @   185.199.110.153
A   @   185.199.111.153
```
**www subdomain — one CNAME record:**
```
CNAME   www   kalaroberts.github.io
```
(Use the GitHub **username**, not the repo path.)

### 8. Point GitHub at the domain
- Repo → **Settings → Pages → Custom domain**
- Enter `powercapital.cloud` → **Save**
- Wait for the DNS check to pass (minutes–24h).
- Check **Enforce HTTPS** once available (certificate issuance may add 1–2h).

### 9. Post-launch
- **Google Search Console:** add `powercapital.cloud`, verify (DNS TXT),
  submit `https://powercapital.cloud/sitemap.xml`.
- **Web3Forms dashboard:** enable **Require Captcha** now that the live
  hCaptcha form is confirmed working (same access key — no new key needed).

---

## Updating the site later
```bash
cd ~/powercapital
# make edits …
git add .
git commit -m "Describe the change"
git push
```
GitHub Pages redeploys automatically within a minute or two.
