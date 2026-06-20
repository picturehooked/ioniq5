# Ioniq 5 Colour Comparison — Deployment Guide

## Live site
- **URL:** TBC (post Vercel deploy)
- **Hosting:** Vercel (auto-deploy on push)
- **GitHub repo:** https://github.com/picturehooked/ioniq5.git
- **Branch:** main

---

## First-time setup (already done if repo exists)

```bash
cd "/Users/charlesfellowes/Documents/Claude/Projects/i5 Colours"
git init
git branch -M main
git remote add origin https://github.com/picturehooked/ioniq5.git
git config user.email "shop@planitearth.co.uk"
git config user.name "Charles Fellowes"
git add -A
git commit -m "Initial deploy — Ioniq 5 colour comparison"
git push origin main
```

Then in Vercel: Add New Project → import `picturehooked/ioniq5` → Framework preset: **Other** → Root directory: `/` → Deploy.

---

## Standard update workflow

```bash
cd "/Users/charlesfellowes/Documents/Claude/Projects/i5 Colours"
rm -f .git/index.lock
git add -A
git commit -m "Description of what changed"
git push origin main
```

The `rm -f .git/index.lock` line is a precaution — the sandbox sometimes leaves a stale lock file. Safe to include every time; it does nothing if the file isn't there.

Vercel detects the push and deploys automatically within 1–2 minutes.

---

## File structure

```
i5 Colours/
├── index.html           ← main app
├── hyundailogo.png
├── DEPLOYMENT.md        ← this file
├── Ecotronic/           ← ecotronic grey pearl images
├── Cyber Grey/          ← cyber grey metallic images
├── red/                 ← red images
└── *.jpeg               ← interior images (dash, seats — colour-agnostic)
```

---

## Adding images

Drop new `.jpeg` files into the correct colour subfolder, then update the `images` map in `index.html` to reference them. Push as normal.

---

## Pending build items

_None outstanding._

---

## Vercel configuration

No `vercel.json` required — static HTML site with no routing complexity. Framework preset: **Other**.

---

## GitHub authentication

GitHub requires a Personal Access Token (not a password) for push operations from Terminal.

- Generate at: github.com → Settings → Developer Settings → Personal Access Tokens → Tokens (classic)
- Scope needed: `repo`
- Use the token as the password when Terminal prompts you
- Tokens expire — generate a new one if authentication fails

---

## If a build fails

1. Vercel → project → Deployments → click the failed deployment
2. Read the error in the build log
3. Fix, push again — Vercel redeploys automatically

Or force a redeploy: Vercel → project → Deployments → latest → Redeploy
