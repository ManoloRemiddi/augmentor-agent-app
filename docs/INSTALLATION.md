# Install Augmentor Agent

[Overview](../README.md) · [Website installation prompts](https://augmentoragent.com/#installation) · [Platform support](PLATFORMS.md)

Choose one route. Do not mix the public Browser 0.1.32 companion with the 0.2.9 extension.

## 1. Desktop + Browser — Linux preview 0.2.9

**You need the prepared preview bundle. It is not publicly downloadable yet.** If you do not already have it, use the public Browser release below or wait for publication. No download URL should be inferred from the version number.

Requirements: Debian 13 amd64, DSH 0.1.5-rc.1 with a configured model, and Chromium for the Browser edition. Desktop computer-control evidence is scoped to KDE Plasma Wayland; not every Linux desktop is verified.

1. Read the bundle's installation and release notes. Verify its files with `sha256sum -c SHA256SUMS`; obtain the bundle from a trusted release source.
2. Preserve existing chats, settings, model choices and browser profiles. For an upgrade, finish active tasks, disconnect/disable the Browser extension and run `augmentor-maintenance prepare`. Continue only when preparation succeeds, and keep the backup.
3. From the bundle directory, install the two packages:

   ```sh
   sudo apt install ./augmentor-runtime_0.2.9_amd64.deb ./augmentor-desktop_0.2.9_amd64.deb
   ```

4. Open **Augmentor Agent** from the application menu. Connect the supported running DSH profile in Settings. Keep launch tokens private. Review integration changes and preserve customized presets; restart DSH only when idle, then check and save the connection.
5. Extract `augmentor-browser-0.2.9.zip` into a permanent directory. In Chromium, open `chrome://extensions`, enable Developer mode and use **Load unpacked**. Register the companion using the browser installation instructions supplied with that bundle.
6. Verify a model response, the Desktop shortcut and the Browser connection with a harmless read-only task. Do not claim installation succeeded if a connection or verification fails.

DSH, model files, provider credentials and optional third-party plugins are not included in the Augmentor bundle. The preview has documented limits; it is not a stable release certified for all Linux systems.

## 2. Desktop only — Linux preview 0.2.9

Use the same bundle and steps 1–4 above, then verify a Desktop model response and your show/hide shortcut. **Skip extension installation and native-host registration for Chromium.** Chromium is not required for Desktop-only use. Both Debian packages are still required.

## 3. Browser only — public Linux release 0.1.32

This older release is publicly downloadable and tested with Linux Chromium and DSH 0.1.5-rc.1.

1. Check Node.js 22.19+ or 24+ and your DSH installation. Preserve its existing home/profile. Do not blindly downgrade a newer DSH version.
2. [Download the complete Browser ZIP](https://github.com/ManoloRemiddi/augmentor-dsh-extension-plugin/releases/download/v0.1.32/augmentor-0.1.32-dist.zip) and extract it into a permanent directory. Keep its extension, plugin and native-host files together.
3. Open `chrome://extensions`, enable Developer mode and load the extracted `extension/` folder unpacked. Copy its extension ID.
4. From the extracted release directory, register the native companion and plugin:

   ```sh
   sh install-native-host.sh EXTENSION_ID "$HOME/.config/chromium"
   dsh plugin --profile web add "$PWD/plugin"
   ```

   Replace `EXTENSION_ID` with the actual ID. Use your actual browser user-data root and DSH profile if different. Update an existing linked installation in place instead of adding duplicates.
5. Restart the existing DSH process only when tasks are finished, restart Chromium, and open the sidebar. Verify the release version, model list and a response using your configured model.

For prerequisites, authentication and troubleshooting, follow the [original release guide](https://github.com/ManoloRemiddi/augmentor-dsh-extension-plugin/tree/v0.1.32) and [website manual](https://augmentoragent.com/docs.html#install).

## Agent-assisted installation

The [website](https://augmentoragent.com/#installation) provides separate copyable prompts for all three routes above. Preview prompts require a bundle you already possess; they do not make an unpublished download available.
