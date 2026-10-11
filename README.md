# DisasterReady — VS Code + Capacitor

A local-first disaster preparedness app with English/Hindi safety guides, emergency-kit checklist, saved emergency plan, and an offline quiz.

## Requirements
- Windows 10/11
- Visual Studio Code
- Node.js LTS and npm
- Git (for GitHub)
- Android Studio and Android SDK (for building the APK)

## A. Run the website locally
1. Open this folder in VS Code.
2. Open **Terminal → New Terminal**.
3. Run:
   ```powershell
   npm install
   ```
4. For a quick browser preview, open `www/index.html` in your browser. Better: install the VS Code extension **Live Server**, right-click `www/index.html`, and choose **Open with Live Server**.
   The website itself uses no online APIs. Browser storage is local to that browser.

## B. Turn it into an Android APK
The first Android setup needs internet to download Capacitor packages and Android build tools. The finished app's safety guides and core features work offline.

1. Install Android Studio and complete its first-run SDK setup.
2. In the VS Code terminal, run:
   ```powershell
   npm install
   npx cap add android
   npx cap sync android
   npx cap open android
   ```
3. Android Studio opens. Wait for Gradle sync.
4. Choose **Build → Build Bundle(s) / APK(s) → Build APK(s)**.
5. The debug APK is normally at `android/app/build/outputs/apk/debug/app-debug.apk`.

### Keep the app offline
The bundled content works without a network. For an app that must have no network permission at all, after `npx cap add android`, open:
`android/app/src/main/AndroidManifest.xml`
and remove any line containing:
```xml
<uses-permission android:name="android.permission.INTERNET" />
```
Do not remove other permissions unless you understand why they are present. Re-run `npx cap sync android` after web asset changes; check the manifest again if you regenerate Android files.

## C. Update app after changing HTML/CSS/JS
1. Edit files under `www/`.
2. Run:
   ```powershell
   npx cap sync android
   ```
3. Rebuild the APK in Android Studio.

## D. Upload to GitHub
1. Create a new empty repository on GitHub named `DisasterReady` (do not add a README on GitHub if you already use this one).
2. In the VS Code terminal at this folder, run:
   ```powershell
   git init
   git add .
   git commit -m "Initial DisasterReady offline app"
   git branch -M main
   git remote add origin https://github.com/YOUR-USERNAME/DisasterReady.git
   git push -u origin main
   ```
3. Replace `YOUR-USERNAME` with your GitHub username. Sign in when Git prompts you.
4. `node_modules` and build outputs are excluded by `.gitignore`. The source code and `www` assets should be committed.

## Folder structure
```text
DisasterReady/
├── www/
│   ├── index.html
│   ├── style.css
│   └── app.js
├── package.json
├── capacitor.config.json
├── .gitignore
└── README.md
```

## Offline and safety limitations
- No server, API key, account, or live weather service is used by the app.
- LocalStorage is used for language preference, checklist progress, quiz best score, and emergency plan. Clearing app/browser data removes these records.
- The app cannot receive live disaster alerts while offline. It is a preparedness guide, not a replacement for local authorities, emergency dispatch, or professional medical advice.
- Test in Airplane Mode on the target phone before relying on it.


## Spatial UI and animated disaster scenes

The app now includes 20 offline safety guides in English and Hindi, including earthquakes, floods, cyclones, lightning, heatwaves, landslides, tsunamis, tornadoes, wildfires, volcanic eruptions, drought, blizzards, avalanches, dust storms, hailstorms, severe thunderstorms, extreme cold, sinkholes, coastal storm surge, and disease outbreaks. Each guide opens a layered animated scene designed with CSS, so the visual effects work offline and do not require downloaded video or images. Animations respect the device's reduced-motion accessibility setting.

Safety information is educational; local conditions vary. Follow official emergency instructions. The app does not provide live alerts.
