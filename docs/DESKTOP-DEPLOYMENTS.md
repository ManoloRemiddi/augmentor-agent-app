<!-- Copyright © 2026 Manolo Remiddi · SPDX-License-Identifier: MIT -->

# Consistent installed desktop releases

“Latest” means the latest tested artifact explicitly activated for this user.
Saving source files, checking out a branch or building an experiment does not
change the installed app. Startup must not guess by modification time, version
number or whichever preview directory happens to exist.

## One selection

`~/.local/share/augmentor/desktop.json` (under `XDG_DATA_HOME` when set) is the
selection for login, the systemd desktop supervisor, the menu, shortcuts, the
secondary window, recovery and the mobile desktop. The old voice-preview and
login commands are aliases of the canonical launcher. Mobile reads the same
root, Python and Node paths; `--desktop-root` is an explicit development override.
A missing selected build is reported rather than silently choosing an older one.

`scripts/install-desktop-startup.py` bootstraps/refreshes these entrypoints and
installs `augmentor-update`. Once a managed release is selected, the installer
refuses to replace it with another root or interpreter: use the update command.
Installer and updater share a kernel lock so simultaneous promotions cannot race.

## Required development and update workflow

1. Implement and test the change in source. Build a complete runnable candidate
   separate from the selected release. For an incremental preview patch, first
   copy the current artifact to a separate candidate and apply the reviewed
   files there; never copy files into the selected release. Record the original
   artifact identity and the patch commits for such a mixed candidate.
2. Run the subsystem tests and the necessary integration checks. Keep the product
   manifest, installed DSH integration, native dependencies and speech contract
   compatible. In particular, a 0.2.9 source checkout cannot replace the installed
   0.2.8 UI against a 0.2.8 product integration just by changing the launcher.
3. Stage the candidate using the installed command:

   ```sh
   augmentor-update stage /absolute/path/to/tested-candidate --source-ref TESTED_COMMIT_OR_MIXED_ARTIFACT_DESCRIPTION
   ```

   Optional `--python` and `--node` explicitly select new interpreters; otherwise
   staging preserves the selected paths, including the Python venv symlink.
   The command prints a unique release directory.
4. Select the returned release directory:

   ```sh
   augmentor-update activate /absolute/path/printed/by/stage
   augmentor-update status
   ```

5. Report the source reference, artifact hash, selected/running roots and evidence.
   At the next login, all managed surfaces load the selected release. A window
   already open retains its running code until closed/reopened or logout; status
   reports `updatePending` for each such window. Recovery continues to work on
   that window while selection is pending. Do not interrupt active work just to
   make the running and selected roots match sooner.

The stage operation copies the native surface, services, adapters, built runtime,
configuration defaults, licenses, scripts and dependencies into a unique release
folder. It does not copy an entire checkout or user conversations/configuration.
It imports the actual Qt window/controller and checks the Node executable. Its
inventory hashes every regular artifact file and records internal symlinks;
external symlinks are refused because they could silently change a release.
Python bytecode caches are excluded. Python and an external Node runtime remain
explicit external dependencies and must be upgraded deliberately with validation.

Promotion verifies the complete staged inventory, repeats import checks and,
for this registered DSH installation, verifies the candidate's authenticated
product identity and model catalog against the running integration. It makes no
model request, changes no saved chat and restarts no backend. The runtime must be
reachable for this preflight; an offline or incompatible candidate cannot replace
the current selection. The checks complement feature tests; imports/catalog
access do not prove every feature or microphone/speaker behavior.

The only selection commit is an atomic, fsynced replacement of `desktop.json`.
A failed stage, failed preflight or interruption before that replacement leaves
the old selection intact. Both artifact directories survive promotion. Treat
selected release files as immutable; hash verification rejects changed artifacts
on subsequent promotion. Runtime startup does not continually hash dependencies.

## Inspect and undo without AI

```sh
augmentor-update status
augmentor-update rollback
augmentor-recover
```

Status reports the selected version, source reference and artifact identity,
plus the actual running roots and connection/audio-control state of desktop,
mobile and secondary windows. An absent window is reported as absent.

Rollback validates and selects `desktop.previous.json`; as with promotion, open
windows retain their code until their next start. The previous artifact is not
deleted. Before a coordinated DSH/speech upgrade, plan its own compatible rollback;
this command changes only the native desktop selection, not backend state, data,
plugins, model settings or external speech dependencies. There is no automatic
fallback that silently launches an obsolete version after a runtime error.

Historical scripts such as `deploy-dual-memory-preview.py` are not the desktop
update path. They refuse activation against a managed release. Package/browser
publishing and source commits remain separate operations; updating any of those
alone must not be described as deploying this installed desktop.

## September 20 validation

The implementation is published with this guide on the current development
branch/PR #3. The installed baseline is a separately staged copy of the user's
reboot-confirmed 0.2.8 audio-enabled preview with the `2dca65f` startup/recovery
patches. Its descriptor records this mixed provenance and the exact artifact
hash; it is not represented as an unmodified 0.2.9 build.

The full native suite passed **383 tests**; the **15 focused deployment/startup
tests** also passed after the final artifact-copy correction. Qualification covers interrupted promotion, failed imports/connection checks,
changed artifacts, external symlinks, internal links, preserved interpreter paths,
selection/rollback, mobile selection and manual recovery with a pending update.
Live qualification imports the staged artifact, checks the actual DSH integration
and renders its native window in a separate offscreen preview without changing
the user's open conversations. A real 0.2.9 source candidate was refused against
the active 0.2.8 DSH integration, preserving the working selection. Live rollback
of the selection also passed without restarting any window.

The selected baseline artifact SHA-256 is
`52f4ef2a146cf727627db0df66f70b80f5c59d9957f704479e4f9d45a913df53`.
All three existing windows remained connected with audio controls while their
selection was pending; they adopt the copied release at their next start.
This does not claim another physical reboot
of the newly staged release path; the user reported the preceding repair's reboot.
