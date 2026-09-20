# Platform support

[Overview](../README.md) · [Installation](INSTALLATION.md)

| Edition | Linux | macOS | Windows |
| --- | --- | --- | --- |
| Browser 0.1.32 | Public download; tested with Chromium | Complete setup not verified | Complete setup not verified |
| Desktop 0.2.9 | Installable preview bundle for Debian 13 amd64; complete public bundle available | Planned next | Planned afterward |
| Browser 0.2.9 | Matching extension and runtime required; included in the complete public bundle | Complete setup not verified | Complete setup not verified |

Chromium extension technology is portable, but Augmentor uses native messaging to reach its local companion. The companion must run and be registered for the particular operating system and browser. DSH alone does not supply it. See [Chrome's native messaging documentation](https://developer.chrome.com/docs/extensions/develop/concepts/native-messaging).

Other Chromium browsers and Linux distributions are not universally certified. Firefox and Safari are not supported by these releases. Desktop graphical-control evidence is scoped to KDE Plasma Wayland and does not imply every application or capture workflow is verified.

The current backend is DeepSeek Harness. Pi support and Pi extensions are planned later, with fewer capabilities initially. No release dates are announced.

## Fedora preview

[Fedora 44 x86_64 Desktop RPM](FEDORA.md) is publicly downloadable for testing. Container installation, rendering and maintenance checks passed. Real desktop control, shortcuts, browser attachment and DSH model turns remain unverified. Atomic desktops and ARM are not covered.
