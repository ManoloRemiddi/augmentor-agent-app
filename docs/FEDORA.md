# Fedora 44 preview installation

[Website](https://augmentoragent.com/) · [Project](../README.md)

This experimental x86_64 package installs Augmentor Agent Desktop and its runtime. DSH and a configured local or cloud model are required separately. Container installation and rendering passed; real Fedora desktop control, shortcuts, browser attachment and DSH model turns remain unverified. This is not yet a fully qualified Fedora release. Atomic desktops, ARM and other Fedora versions are not covered.

Download the [RPM](https://github.com/ManoloRemiddi/augmentor-agent-app/releases/download/v0.2.9-fedora-preview/augmentor-agent-0.2.9-1.fc44.x86_64.rpm) and [SHA256SUMS](https://github.com/ManoloRemiddi/augmentor-agent-app/releases/download/v0.2.9-fedora-preview/SHA256SUMS) into the same folder. Open a terminal there:

```sh
sha256sum -c SHA256SUMS
sudo dnf install ./augmentor-agent-0.2.9-1.fc44.x86_64.rpm
```

Continue only if the checksum reports OK. DNF resolves dependencies. The preview RPM is unsigned.

Open **Augmentor Agent** from the application menu. In Settings connect your running DSH profile and configured model. The original Linux bundle targets DSH 0.1.5-rc.1; inspect compatibility before changing an existing version. Preserve chats, credentials, settings and customized presets. Test a harmless model response before relying on it.

This Fedora download is for Desktop. It includes companion infrastructure but no matching Browser extension ZIP; do not pair it with the older public Browser 0.1.32 installer.

Before updating or removing, finish tasks, close the app, disconnect any Browser integration and run `augmentor-maintenance prepare`. Then use DNF. User data is retained on removal.

```sh
sudo dnf remove augmentor-agent
```

[Report results](https://github.com/ManoloRemiddi/augmentor-agent-app/issues) with Fedora version, desktop environment and the failing step; omit credentials and private conversations.
