# Raakaa MTF Sentiment (Android)

A Cordova wrapper around the Raakaa Capital multi-timeframe sentiment
speedometer page (`www/index.html`), built into an Android `.apk` by
GitHub Actions — no local Android Studio needed.

## Folder contents

```
raakaa-mtf-sentiment-apk/
├─ www/index.html                     # the app itself
├─ config.xml                         # Cordova app config (name, id, version...)
├─ package.json                       # Cordova + cordova-android deps
├─ .gitignore
└─ .github/workflows/build-apk.yml    # builds the .apk on every push
```

## 1. Put this on GitHub

```bash
cd raakaa-mtf-sentiment-apk
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/<your-username>/<your-repo>.git
git push -u origin main
```

## 2. Get the .apk

Pushing to `main` triggers **Build Android APK**
(`.github/workflows/build-apk.yml`) automatically on GitHub's own
Ubuntu runner, which installs the JDK, Android SDK, and Cordova, then
builds the app.

- Repo → **Actions** tab → open the latest **Build Android APK** run.
- Under **Artifacts**, download `raakaa-mtf-sentiment-apk` — it contains the `.apk`.
- This is a **debug build**, so it's unsigned but installs fine on a
  phone with "install from unknown sources" enabled — no keystore setup required.

### Optional: attach the .apk to a GitHub Release
```bash
git tag v1.0.0
git push origin v1.0.0
```

## 3. Install it on a phone

Copy the `.apk` to your Android device (email, cloud drive, USB) and
open it. If blocked, enable **Settings → Security → Install unknown
apps** for whichever app you used to open the file.

## 4. Build locally instead (optional)

Requires Node.js, JDK 17, and the Android SDK installed.

```bash
npm install -g cordova
npm install
cordova platform add android
cordova build android          # debug APK in platforms/android/app/build/outputs/apk/debug/
```

## Publishing a signed release APK (optional, later)

The workflow currently produces a **debug** APK, good for testing. To
publish to the Play Store or distribute a signed release build later:
1. Generate a keystore (`keytool -genkey -v -keystore release.keystore ...`).
2. Add it and its passwords as GitHub **Secrets**.
3. Add a signing step to `build-apk.yml` and switch the build command
   to `cordova build android --release`.

## Editing the app

`www/index.html` is the same file used elsewhere — edit it directly,
commit, and push; the next Action run rebuilds the `.apk` automatically.
