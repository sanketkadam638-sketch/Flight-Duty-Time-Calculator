# Cabin Crew FDTL Calculator — Android App

A Capacitor-wrapped Android app for the offline Cabin Crew FDTL calculator
(`www/index.html`). It runs entirely on-device — no network calls, no
external fonts, works in airplane mode.

This repo is set up to build the Android APK **in the cloud via GitHub
Actions** — you do not need Node.js or Android Studio installed locally.

## How to build the APK from GitHub

1. Push this repository to GitHub (see steps below if you haven't already).
2. Go to the **Actions** tab of your repository.
3. Select **Build Android APK** from the left sidebar.
4. Click **Run workflow** (or just push a commit to `main` — it runs
   automatically on every push).
5. Wait for the run to finish (a few minutes), then open the completed run
   and download the **cabin-crew-fdtl-debug-apk** artifact from the
   "Artifacts" section at the bottom of the run page.
6. Unzip it to get `app-debug.apk`, transfer it to an Android phone (via
   email, Drive, USB, etc.), and install it — you may need to allow
   "Install unknown apps" for whichever app you used to open the file.

That debug APK is enough to test the full app on your own phone.

## Publishing to the Play Store

The Play Store requires a **signed release** build (an `.aab` file), which
needs a one-time signing key. This isn't set up in the workflow yet because
the signing key is something only you should generate and hold onto — it's
needed for every future update to this app, so losing it means you can never
update the app again under the same listing.

When you're ready to publish:
1. Create a [Google Play Developer account](https://play.google.com/console/signup) (one-time $25 fee).
2. Let me know, and I'll add a signing step to the GitHub Actions workflow —
   you'll generate a keystore once and store it as a GitHub secret, and every
   push will then produce a ready-to-upload signed `.aab`.
3. Upload the `.aab` in the Play Console, fill in the store listing
   (title, description, screenshots, privacy policy URL, content rating
   questionnaire, data safety form), and submit for review.

### Things you'll need for the store listing
- **App icon** (512×512 PNG) and **feature graphic** (1024×500 PNG) — not
  yet included in this project.
- **Screenshots** of the app running on a phone.
- **Privacy policy URL** — required even for offline apps with no data
  collection; a one-page "this app collects no data" statement hosted
  anywhere (e.g. a GitHub Pages page) satisfies this.
- **Content rating questionnaire** — this is a reference/utility tool with
  no objectionable content, so it should rate as "Everyone" straightforwardly.

## Updating the app content

Edit `www/index.html`, commit, and push. The next Actions run will pick up
the change automatically and rebuild the APK — no local sync step needed.

## Pushing this project to GitHub for the first time

```bash
git init
git add .
git commit -m "Initial commit: Cabin Crew FDTL calculator Android app"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPO-NAME.git
git push -u origin main
```
Replace the URL with the one GitHub shows you after creating a new,
empty repository at github.com.

## Notes

- The manifest includes the `INTERNET` permission — this is added by
  Capacitor by default for the WebView component itself, not because the
  app calls out to any server. All calculator logic runs on-device.
- App ID is `com.fdtlcalc.cabincrew`, app name "Cabin Crew FDTL" — both
  editable in `capacitor.config.ts` and
  `android/app/src/main/res/values/strings.xml` before your first release
  (the app ID cannot be changed after publishing).
- Airline and commute-time settings are saved in the app's local storage on
  the device itself, not synced anywhere.
