# Deploying `meetpup.space` on GitHub Pages

This folder (`/app/frontend/landing-site/`) contains the complete static website for Meet Pup:

```
landing-site/
├─ index.html         ← Landing page (Hero, features, showcase, CTA)
├─ support.html       ← Support / FAQ page (required by Apple)
├─ privacy.html       ← Privacy Policy (required by Apple)
├─ terms.html         ← Terms of Service
├─ styles.css         ← Shared stylesheet
├─ favicon.svg        ← Meet Pup paw favicon
├─ assets/            ← App screenshots (map, chat, family plan)
├─ CNAME              ← Tells GitHub Pages your custom domain
├─ .nojekyll          ← Disables Jekyll processing on GitHub
├─ robots.txt         ← Allow search engines
└─ sitemap.xml        ← SEO sitemap
```

Everything is ready. Just follow the 3 steps below to publish it live.

---

## 🚀 STEP 1 — Push these files to a GitHub repo

1. **Create a GitHub account** (skip if you already have one): https://github.com/signup
2. **Create a new repository:**
   - Go to https://github.com/new
   - Repository name: **`meetpup-site`** (or any name you prefer)
   - Choose **Public** (required for free GitHub Pages)
   - **DO NOT** initialize with README, .gitignore, or license (keep it empty)
   - Click **Create repository**
3. **Upload the files:**

   **Option A — Drag & drop (easiest, no command line):**
   - Zip the contents of `/app/frontend/landing-site/` (all files including hidden ones)
   - On the new empty repo page, click **"uploading an existing file"** link
   - Drag ALL the files (`index.html`, `support.html`, `styles.css`, `assets/`, `CNAME`, `.nojekyll`, etc.) into the browser
   - Add a commit message like `Initial landing site` and click **Commit changes**

   **Option B — Git command line (if you know Git):**
   ```bash
   cd landing-site/
   git init
   git add .
   git commit -m "Initial landing site"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/meetpup-site.git
   git push -u origin main
   ```

> ⚠️ **Important:** Make sure `.nojekyll` and `CNAME` (no file extension) are BOTH uploaded. In Finder/Explorer they may be hidden — enable "Show hidden files" before dragging.

---

## 🚀 STEP 2 — Enable GitHub Pages

1. In your `meetpup-site` repo, click **Settings** (top-right tab).
2. In the left sidebar, click **Pages**.
3. Under **Build and deployment**:
   - **Source:** Deploy from a branch
   - **Branch:** `main` · folder `/ (root)` · click **Save**
4. Wait ~30 seconds. Refresh the page. You'll see:
   > ✅ Your site is live at `https://YOUR_USERNAME.github.io/meetpup-site/`
5. Under **Custom domain**, type `meetpup.space` → **Save**.
6. Check **Enforce HTTPS** (may take up to 24 h for the certificate to provision).

---

## 🚀 STEP 3 — Point Namecheap DNS to GitHub Pages

1. Log into **Namecheap** → **Domain List** → click **Manage** next to `meetpup.space`.
2. Click the **Advanced DNS** tab.
3. **Delete any existing A/ALIAS/CNAME records for `@` and `www`** (parked page defaults).
4. **Add these 5 records exactly:**

   | Type | Host | Value | TTL |
   |------|------|-------|-----|
   | A Record | @ | `185.199.108.153` | Automatic |
   | A Record | @ | `185.199.109.153` | Automatic |
   | A Record | @ | `185.199.110.153` | Automatic |
   | A Record | @ | `185.199.111.153` | Automatic |
   | CNAME Record | www | `YOUR_USERNAME.github.io.` | Automatic |

   > Replace `YOUR_USERNAME` with your actual GitHub username. Keep the trailing dot on the CNAME value.

5. **Save all** changes (green check icon).
6. Wait **10–60 minutes** for DNS to propagate globally. You can watch it live at https://dnschecker.org/#A/meetpup.space.

Once DNS resolves, visit **https://meetpup.space** — the Meet Pup landing page will load. HTTPS will activate automatically within a few hours.

---

## 📧 Setting up `support@meetpup.space` email forwarding

You wanted emails to `support@meetpup.space` to forward to your personal inbox. Namecheap includes free email forwarding — no extra cost.

### Step 1 — Enable email forwarding in Namecheap

1. Log into **Namecheap** → **Domain List** → **Manage** next to `meetpup.space`.
2. Click the **Domain** tab (not Advanced DNS this time).
3. Scroll to the **Redirect Email** section.
4. Add a new alias:
   - **Alias:** `support`
   - **Forwards to:** `duffy.the.maltipoo@gmail.com` (or whichever inbox you actually check)
5. Click **Save Changes** (✓).

After you save, Namecheap will automatically inject the correct **MX records** into your DNS. Do not modify these MX records — they must stay for forwarding to work.

### Step 2 — Verify it works

Wait ~15–30 minutes, then send a test email from any inbox to `support@meetpup.space`. It should land in `duffy.the.maltipoo@gmail.com` within a minute.

> 💡 **Pro tip:** In Gmail, go to **Settings → Accounts → Send mail as → Add another email address**, add `support@meetpup.space`, verify it, and now you can *reply* from that address inside Gmail. Recipients only see `support@meetpup.space` — your personal address stays hidden.

### Step 3 — Coexists safely with Resend

Resend already uses **`noreply@meetpup.space`** for password reset emails. Namecheap's MX records for forwarding only affect INCOMING mail, and Resend's SPF/DKIM records only affect OUTGOING mail signing. Both work simultaneously with no conflict. Do NOT delete any existing Resend DNS records (SPF `TXT`, DKIM `CNAME`s).

---

## 🔄 Updating the site later

Any edit to a file in the GitHub repo → GitHub Pages auto-rebuilds in ~30 seconds. To edit directly in the browser:
1. Open the file (e.g. `index.html`) in GitHub.
2. Click the pencil icon (✏️) top-right.
3. Make your changes → **Commit changes** at the bottom.
4. Refresh `meetpup.space` after ~1 minute.

---

## ✅ What Apple reviewers will see

Once live, provide these URLs in App Store Connect:

| Field | URL |
|-------|-----|
| **Marketing URL** | https://meetpup.space |
| **Support URL** | https://meetpup.space/support.html |
| **Privacy Policy URL** | https://meetpup.space/privacy.html |

All three URLs are required for App Store approval. ✅

---

## 🛠️ Alternatives (if GitHub Pages doesn't work for you)

- **Cloudflare Pages** — similar workflow, faster CDN. Connect the same GitHub repo at https://pages.cloudflare.com.
- **Vercel** — also free for static sites. https://vercel.com/new
- **Netlify** — drag-and-drop deploy, no GitHub required. https://app.netlify.com/drop

Each supports custom domains + free HTTPS. The DNS instructions differ per provider — they'll walk you through it in their dashboard.
