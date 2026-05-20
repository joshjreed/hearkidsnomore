# Putting hearkidsnomore.com on GitHub Pages

A walk-through from "files on my laptop" to "site live at https://www.hearkidsnomore.com".

Estimated time: ~30 min of work, plus up to a few hours of waiting for DNS + the HTTPS cert.

---

## 1. Get the files into a GitHub repo

- [ ] Create a GitHub account at https://github.com/signup if you don't already have one.
- [ ] Install Git for Windows if you don't have it: https://git-scm.com/download/win
- [ ] On GitHub, click **New repository**. Name it `hearkidsnomore` (or anything). Public. Don't add a README, .gitignore, or license — we already have files locally.
- [ ] In PowerShell, from `C:\Users\reedj\projects\otsu`, run:

  ```powershell
  git init
  git add .
  git commit -m "initial site"
  git branch -M main
  git remote add origin https://github.com/joshjreed/hearkidsnomore.git
  git push -u origin main
  ```

  The first push will prompt you to log in via browser.

---

## 2. Turn on GitHub Pages

- [ ] In the repo on github.com, go to **Settings → Pages**.
- [ ] Under **Build and deployment → Source**, pick **Deploy from a branch**.
- [ ] Branch: `main`, folder: `/ (root)`. Click **Save**.
- [ ] Wait ~1 minute. Refresh. You should see a green box: "Your site is live at https://<username>.github.io/hearkidsnomore/".
- [ ] Click that link and confirm the page loads.

---

## 3. Point the domain at GitHub (DNS in Hover)

Log in to https://hover.com, click your domain, then **DNS**.

Delete any existing `A`, `AAAA`, or `CNAME` records on `@` and `www` first (Hover sometimes adds parking-page defaults).

- [ ] Add four `A` records on `@` (the apex / root domain) pointing to GitHub's Pages IPs:

  | Host | Type | Value           |
  |------|------|-----------------|
  | @    | A    | 185.199.108.153 |
  | @    | A    | 185.199.109.153 |
  | @    | A    | 185.199.110.153 |
  | @    | A    | 185.199.111.153 |

- [ ] Add one `CNAME` record on `www` pointing at your GitHub Pages host:

  | Host | Type  | Value                              |
  |------|-------|------------------------------------|
  | www  | CNAME | `joshjreed.github.io.`             |

  (Trailing dot is fine; Hover may strip it.)

- [ ] Save changes in Hover.

---

## 4. Tell GitHub about the custom domain

The `CNAME` file in this repo already contains `www.hearkidsnomore.com`, so GitHub will detect it on the next push. But also:

- [ ] Back in **Settings → Pages** on github.com, in the **Custom domain** field, type `www.hearkidsnomore.com` and click **Save**. GitHub will verify the DNS — this can take a few minutes.
- [ ] Wait. The page will show "DNS check in progress" and then "DNS check successful". If it stays stuck for >1 hour, recheck the DNS records in Hover.

---

## 5. Enable HTTPS

- [ ] Once the DNS check passes, the **Enforce HTTPS** checkbox becomes clickable. Check it.
- [ ] First-time cert provisioning can take up to ~24 hours but is usually under an hour.
- [ ] Visit `https://www.hearkidsnomore.com` — you should see the site with a lock icon.
- [ ] Visit `http://hearkidsnomore.com` (no www, no https) — it should redirect to the www + https version automatically.

---

## 6. Add real photos later

When you have pictures:

- [ ] Drop them in an `images/` folder in this project (e.g. `images/pool.jpg`).
- [ ] Open `index.html` and replace each `<div class="ph">photo N</div>` with:

  ```html
  <img src="images/pool.jpg" alt="Kids at the pool">
  ```

- [ ] Commit and push:

  ```powershell
  git add .
  git commit -m "add real photos"
  git push
  ```

- [ ] GitHub Pages rebuilds in ~30 seconds. Hard-refresh the site (Ctrl+Shift+R) to bypass cache.

---

## Troubleshooting

- **Site loads at `<username>.github.io/hearkidsnomore` but not the custom domain:** DNS hasn't propagated yet. Wait an hour, then check https://dnschecker.org/#A/hearkidsnomore.com — you should see the four GitHub IPs.
- **HTTPS box is grayed out:** GitHub hasn't finished verifying DNS. Wait, then refresh the Pages settings page.
- **"There isn't a GitHub Pages site here":** The `CNAME` file in the repo got deleted or has the wrong domain. Make sure it contains exactly `www.hearkidsnomore.com` on one line.
- **Page looks unstyled:** `style.css` didn't push, or the link tag points at the wrong path. Check that the file exists in the repo on github.com.
