# Raju Reyaz PWA — Deployment Guide

## What's Fixed

**Font Visibility Issues:**
- Changed `display=swap` → `display=optional` for Google Fonts (prevents flash of invisible text, graceful fallback)
- Added system font stack fallbacks: `-apple-system, BlinkMacSystemFont, 'Segoe UI'`
- Added `text-shadow` to white text over images (improves readability if image doesn't load)
- Fallback colors for `.splash-card-v2` (dark espresso) if backgrounds fail
- `#EFF0ED` instead of `#fff` for better contrast and edge rendering

**PWA Setup:**
- `manifest.json` with fullscreen display mode (`display: fullscreen`)
- Service worker (`sw.js`) for offline caching and asset preloading
- Apple mobile web app meta tags (`apple-mobile-web-app-capable`, `-status-bar-style`)
- SVG icon embedded in manifest (no image file needed)
- PWA installable on Android and iOS, opens in fullscreen
- Network-first caching strategy (loads from network first, falls back to cache)

---

## Deploy to Vercel

### Step 1: Initialize Git Repo (if not already)
```bash
git init
git add .
git commit -m "Initial commit: Raju Reyaz PWA"
```

### Step 2: Push to GitHub
```bash
git remote add origin https://github.com/YOUR_USERNAME/raju-reyaz-pwa.git
git branch -M main
git push -u origin main
```

### Step 3: Connect Vercel to GitHub
1. Go to [vercel.com](https://vercel.com)
2. Sign in / create account
3. Click **Add New... → Project**
4. Select **Import Git Repository**
5. Find `raju-reyaz-pwa` repo (authorize GitHub access if needed)
6. Click **Import**

### Step 4: Configure Environment
- **Framework Preset**: None / Other (it's static HTML)
- **Build Command**: (leave empty)
- **Install Command**: (leave empty)
- **Output Directory**: `.`
- Click **Deploy**

Vercel will deploy automatically. Your app is live at:
```
https://raju-reyaz-pwa.vercel.app
```

---

## Auto-Deploy via GitHub Actions

The `.github/workflows/deploy.yml` file automates deployment on every push to `main`.

### To Enable:

1. **Get Vercel tokens:**
   - Go to [vercel.com/account/tokens](https://vercel.com/account/tokens)
   - Create a token, copy it
   - Go to **GitHub Repo Settings → Secrets and variables → Actions**
   - Add secret: `VERCEL_TOKEN` = your token

2. **Get Vercel IDs:**
   - Run locally: `vercel link` (follow prompts)
   - This creates `.vercel/project.json`
   - Extract `projectId` and `orgId`

3. **Add to GitHub Secrets:**
   - `VERCEL_PROJECT_ID` = your project ID
   - `VERCEL_ORG_ID` = your org ID

4. **Push & Watch:**
   ```bash
   git push origin main
   ```
   GitHub Actions automatically deploys to Vercel.

---

## Verify PWA on Mobile

### Android:
1. Open Chrome → visit your deployed link
2. Menu → **Install app** (or add to home screen)
3. App opens fullscreen

### iOS:
1. Open Safari → visit your deployed link
2. Share menu → **Add to Home Screen**
3. Open from home screen — runs fullscreen

---

## Rollback / Redeploy

If you need to redeploy without pushing:
1. Go to [vercel.com/dashboard](https://vercel.com/dashboard)
2. Select project
3. Deployments tab → click any previous deployment
4. Click **...** → **Redeploy**

---

## Performance Notes

- **Fonts**: Google Fonts cached via service worker. First load ~100ms slower (font load), subsequent loads instant.
- **Images**: Unsplash images cached. On slow networks, fallback colors show (espresso dark backgrounds).
- **Offline**: Core booking flow works offline (HTML/CSS/JS in cache). Images won't load, but interface is functional.

---

## Customization

**Change theme colors:**
Edit `:root` in `<style>` section of `index.html`:
```css
--gold: #B8965A;      /* Primary accent */
--ivory: #F8F4EE;     /* Background */
--espresso: #1A1208;  /* Dark text/buttons */
```

**Change display mode:**
In `manifest.json`, change:
```json
"display": "fullscreen"  → "standalone" (floating navigation bar on top)
```

---

## Support

Questions? Check:
- [Vercel Docs](https://vercel.com/docs)
- [PWA Docs](https://web.dev/progressive-web-apps/)
- [Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
