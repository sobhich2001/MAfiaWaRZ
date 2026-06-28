# Mafia Wars — Android APK (cloud build)

This builds an installable Android APK for the Mafia Wars LAN game using
GitHub Actions. You do **not** need Android Studio or any SDK on your computer.

The phone runs the **app (client)**. One **PC on the same Wi-Fi runs the server**
(`server.js`, not included in the app). Phones cannot run the Node server.

---

## Get the APK in 6 steps

1. **Create a free GitHub account** (if you don't have one) and make a new
   repository — call it `mafia-wars`. Make it **Public** (private also works,
   Actions minutes are free for public repos).

2. **Upload these files** into the repo, keeping the folder structure exactly:
   ```
   capacitor.config.json
   package.json
   .gitignore
   README.md
   www/index.html
   .github/workflows/build-apk.yml
   ```
   Easiest way: on the repo page click **Add file → Upload files**, then drag
   the whole folder in. Make sure the `.github/workflows/build-apk.yml` path
   is preserved (GitHub keeps folders when you drag a folder).

3. The upload counts as a push, so the build **starts automatically**. Click the
   **Actions** tab. You'll see a run called "Build Android APK" working
   (yellow dot → green check). First run takes ~3–6 minutes.
   - If it didn't auto-start, open **Actions → Build Android APK → Run workflow**.

4. When it finishes (green check), **click into the run**. Scroll to the bottom
   to the **Artifacts** section and download **`mafia-wars-debug-apk`**.
   It downloads as a `.zip`.

5. **Unzip it** — inside is `app-debug.apk`. Copy that file to your Android phone
   (USB, Google Drive, email to yourself, etc.).

6. On the phone, **tap the APK to install**. Android will warn about installing
   from an unknown source — allow it for your browser/Files app, then install.

---

## Playing over LAN

1. On a PC on the same Wi-Fi, install Node.js, put `server.js` in a folder, and run:
   ```
   npm install ws
   node server.js
   ```
2. Find that PC's local IP: `ipconfig` (Windows) or `ifconfig` / `ip addr`
   (Mac/Linux). It looks like `192.168.1.5`.
3. Open the app on each phone. In the **Host IP** field type that PC's IP
   (e.g. `192.168.1.5`) — **not** `localhost`. Enter a name and join.
4. First player to join is the Host and can start the game.

---

## Notes

- This is a **debug** APK — perfect for testing, not for the Play Store.
  A store release needs a signed build (a keystore), which is a separate step.
- The app talks to the server over `ws://` (plain, not encrypted). That's
  already enabled via `cleartext: true` in `capacitor.config.json`. Fine for a
  home network; don't use it over the open internet.
- To change the app name or ID, edit `capacitor.config.json` before pushing.
