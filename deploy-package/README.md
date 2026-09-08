# ২য় বন্ধু মহামিলন ঢাকা ২০২৬ — Host Package

This package contains a host-ready `index.html` for the event registration app (based on "Eid Bondhu Adda") adapted for the event title "২য় বন্ধু মহামিলন ঢাকা ২০২৬" and instructions to deploy the Google Apps Script backend.

Contents
- index.html — single-file static web app (form, canvas ticket renderer, QR scanner, dashboard). Replace the SCRIPT_URL constant with your Apps Script Web App URL before hosting.
- README.md — this file.

Quick checklist before hosting
1. Deploy the provided Google Apps Script (AppsScript.gs) as a Web App and copy the Web App URL.
2. In the Apps Script, set `SECRET_TOKEN` to a secure random string.
3. In `index.html`, set `SCRIPT_URL` to your deployed Web App URL and ensure `CLIENT_SECRET` matches `SECRET_TOKEN`.
4. Enable the Slides API (Advanced service) and Drive API in the Apps Script project.

Detailed deployment steps

1) Create and prepare the Google Sheet
- Create a Google Sheet and note its spreadsheet ID (from the URL). If you used the one provided earlier, the script already points to it.
- Create a sheet named `registrations` with headers (first row):
  - name, phone, paymentInfo, photoUrl, checkedIn, mealServed, snackServed, souvenirGiven, createdAt, pngUrl

2) Create the Google Slides template
- Create a Google Slides presentation and design your ticket on a single slide.
- Add placeholder text fields using tokens exactly like `{{NAME}}`, `{{PHONE}}`, `{{PAYMENT}}` where you want the dynamic values substituted.
- Copy the presentation ID from the URL (the long ID in `/presentation/d/<ID>/edit`).

3) Apps Script setup
- Open https://script.google.com and create a new project.
- Paste the Apps Script code (AppsScript.gs) you were given earlier. Replace `TEMPLATE_PRESENTATION_ID` and `SHEET_ID` with your slide and sheet IDs, and set `SECRET_TOKEN` to a random value.
- In the Apps Script editor, enable Advanced Google Services -> Slides API.
- In the Google Cloud Console (linked to the Apps Script project), enable the Slides API and Drive API for the project.

4) Deploy the Apps Script as Web App
- Click "Deploy" -> "New deployment" -> Choose "Web app".
- Execute as: `Me` (so the script uses your account permissions to write to the sheet and create Drive files).
- Who has access: choose `Anyone` or `Anyone with link` (depending on whether you want public access).
- Deploy and copy the web app URL.

5) Wire the client
- Open the `index.html` in this package.
- Set `SCRIPT_URL` to the deployed web app URL.
- Set `CLIENT_SECRET` to the same value as `SECRET_TOKEN` used in Apps Script.

6) Host the static site
- Host `index.html` on any static hosting provider (GitHub Pages, Netlify, Vercel static, or a simple web server).
- Example local test: `python -m http.server 8000` then open `http://localhost:8000/deploy-package/index.html`.

7) Test
- Open the hosted page, fill the form and submit — you should see a generated PNG (downloaded automatically) and the registration row written to the Google Sheet. The PNG is uploaded to Drive and the URL saved in the sheet (pngUrl).

Security & notes
- The `CLIENT_SECRET` in client-side JS is inspectable. For stronger protection, host a small server-side proxy that keeps the secret out of JS.
- Returning large base64 PNGs in JSON is acceptable for small volumes. For large-scale usage, rely on Drive URLs only and fetch asynchronously.
- Make sure to trash or remove temporary copies of slides if you change the script behavior.

Download ZIP
- The package is committed to the branch `deploy/event-registration-package`.
- Download ZIP: `https://github.com/SSC93Pirojpur/ssc93Pirojpur-Eid-Bondhu-Adda/archive/refs/heads/deploy/event-registration-package.zip`

If you want, I can also:
- Replace CLIENT_SECRET with a custom value you provide and re-package.
- Create a small CI workflow to auto-deploy the static site.
