# 40K Collection Tracker – standalone setup

This folder is a complete app: no Claude needed. You host it for free on GitHub Pages and connect your own Google account to it. Setup takes about 15 minutes, once.

Files: `index.html` (the app), `manifest.webmanifest`, `sw.js`, `icon-192.png`, `icon-512.png`.

---

## 1. Put the app online (GitHub Pages)

1. Sign in at github.com (create a free account if needed).
2. Click **New repository**. Name it `40k-tracker`, set it to **Public**, and click **Create repository**.
3. Click **uploading an existing file**, drag in all five files from this folder (not this guide), then click **Commit changes**.
4. Go to **Settings → Pages**. Under *Build and deployment* choose **Deploy from a branch**, branch **main**, folder **/ (root)**, then **Save**.
5. After a minute your app is live at `https://YOUR-GITHUB-NAME.github.io/40k-tracker/`.

The app works straight away. Your collection is saved on that device. Steps 2–3 add Google sync.

## 2. Create your Google sign-in (Google Cloud, free)

1. Open console.cloud.google.com and sign in with the Google account that should hold the sheet.
2. Top bar → project picker → **New project**, name it `40K Tracker`, then **Create**, and make sure it's selected.
3. **APIs & Services → Library**: search for and **Enable** both **Google Sheets API** and **Google Drive API**.
4. **Google Auth Platform** (or *OAuth consent screen*) → **Get started**:
   - App name `40K Collection Tracker`, support email = your address.
   - Audience: **External**.
   - Contact email = your address. Agree and **Create**.
5. **Audience → Test users → Add users**: add your own Gmail address (and a friend's, if they'll use your copy).
6. **Clients → Create client**:
   - Application type: **Web application**.
   - Under **Authorized JavaScript origins**, add `https://YOUR-GITHUB-NAME.github.io`: just the domain, with no `/40k-tracker` path and no trailing slash.
   - **Create**, then copy the **Client ID** (it ends in `.apps.googleusercontent.com`).

## 3. Connect the app

1. Open your app URL. Scroll to **Google Sheet** and paste the Client ID. Tap **Save**, then **Connect Google**.
2. Google will warn that it *hasn't verified this app*. That's expected, because it's your own private app: tap **Continue**.
3. The app creates a sheet called **40K Collection Tracker** in your Drive and syncs to it.

Optional: so you don't have to paste the ID on every device, open `index.html`, find `const CLIENT_ID_BUILTIN = '';`, put your ID between the quotes, and upload the file to GitHub again.

## 4. Add it to your phone

- **iPhone (Safari):** open the URL → Share → **Add to Home Screen**.
- **Android (Chrome):** open the URL → menu → **Install app**.

It then opens full-screen like a normal app and works offline (sync resumes when you're online).

---

## How syncing works

- Every change on a device is sent to the sheet a few seconds later (switch this off with *Sync automatically after changes*).
- When you open the app, or tap **Sync now**, it checks whether the sheet changed elsewhere (another device, or you edited it directly) and loads those changes.
- If the sheet *and* this device both changed since the last sync, the app asks which one should win.
- In the sheet you can change **Qty**, **Status** (Wishlist / Unbuilt / Built / Painted) and **Models**, or add a new row with just **Faction** and **Unit** names. Rows that don't match a unit in the current Munitorum Field Manual are listed and left out.
- Google sign-ins from browser apps last about an hour. After that, the app asks you to tap **Sync now** again (one tap, no password).

## Good to know

- The app can only see sheets it created itself (Google's narrowest permission, `drive.file`), not the rest of your Drive.
- The sheet the Claude version of the tracker uses is a separate file. To move that collection over, copy its rows into the new app's sheet, then tap **Load from sheet** in the app.
- While the Google project is in *Testing*, only the test users you added can sign in (up to 100). That's fine for personal use; you don't need to publish or verify it.
- Points update automatically: once a day the app checks the 40kdc-data mirror of the Munitorum Field Manual.
