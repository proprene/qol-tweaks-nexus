# QoL-Tweaks (Nexus)

Quality-of-life tweaks for Guild Wars 2, built as a [Nexus](https://github.com/RaidcoreGG/Nexus) addon.

Runtime hooks via byte-pattern scan and detours. No game files are modified.
Runs on Windows and under Wine/CrossOver on macOS.

## Install

Drop `qol_tweaks.dll` into `<GW2>/addons/`. Nexus discovers it via its
`GetAddonDef` export.

The window starts **hidden**. Open it with `Alt+Shift+C`, the checkbox in
Nexus's addon options, or the Quick Access icon.

Settings live in a folder named after the DLL, created beside it on first
launch — `<GW2>/addons/qol_tweaks.dll` gives
`<GW2>/addons/qol_tweaks/qol_tweaks.ini`. The folder follows the DLL if it's
renamed.

> **Do not run this alongside `arcdps_qol_tweaks.dll`.** Both builds detour
> the same game functions. The addon detects the other build and disables its
> own hooks rather than risk it.

## After a Guild Wars 2 update

Nexus **disables this addon automatically** whenever the game updates, and it
stays disabled until you re-enable it in Nexus.

That is deliberate, not a bug. Every feature works by matching a byte pattern
against a specific `Gw2-64.exe` build. After a patch a pattern may match
nothing — the feature quietly stops — or match the wrong bytes and hook
something unintended, which is far worse than doing nothing. Staying off until
the signatures have been re-checked against the new build is the safe default.

If a release has been published for the new game build, update the addon first,
then re-enable it.

## Features

| Feature | Default |
|---|---|
| Cinematic skip (configurable delay) | on |
| Dialogue skip | off |
| Delete confirmation bypass | off |
| Drag-drop confirmation bypass | off |
| Salvage confirmation bypass | off |
| Bank opening lag fix | off |
| Legendary armory fix (hero panel) | always on |
| Quick Access shortcut | on |

## Build

```
cmake -B build -S . -G "Visual Studio 17 2022" -A x64
cmake --build build --config Release
```

Needs CMake 3.20+ and VS2022. No submodules — ImGui 1.80 is vendored in-tree.
CI builds every push on `windows-2022`.

`res/icon.png` is optional at configure time. When absent, the Quick Access
icon compiles out and the build stays green; when present, CMake embeds it and
defines `QOL_HAS_ICON`.

## Release procedure

Releases are published **by hand** — CI only produces an artifact.

1. Bump `ADDON_VERSION` in `src/globals.h` and `Version` in `GetAddonDef()`
   (`src/entry.cpp`). Both must agree.
2. Commit, tag, push.
3. Wait for CI, download the `qol_tweaks` artifact.
4. Attach `qol_tweaks.dll` to a release on the public
   [`proprene/qol-tweaks-nexus`](https://github.com/proprene/qol-tweaks-nexus)
   repo. That repo carries the Nexus build only — the ArcDPS build has its own
   release repo, `proprene/qol-tweaks`.

The in-addon updater reads `proprene/qol-tweaks-nexus/releases/latest` and looks
for an asset named exactly `qol_tweaks.dll`. A release without it makes the
updater report *"Update vX.Y.Z has no Nexus build attached yet."*

## Verification checklist

There is no automated test suite: the addon's entire behaviour is pattern
scanning and detouring a live `Gw2-64.exe`. CI proves it compiles; the rest is
manual.

- [x] Appears in the Nexus Addon Library, loads with no `WARNING` in the log
- [x] Window starts hidden; `Alt+Shift+C`, the addon-options checkbox, and the
      Quick Access icon each open it; `Escape` closes it
- [x] Status block reads `Status: OK`
- [x] Cinematic skip fires on a story instance cutscene
- [x] Dialogue skip advances an NPC conversation
- [x] Delete / drag-drop / salvage confirmations suppressed with toggles on
- [x] Bank fix toggles live; bank opens without the lag spike
- [x] Legendary armory still populates after opening hero equipment
- [x] Settings persist across a game restart
- [x] Updater reports up-to-date against the current release
- [ ] Updater against a release with no Nexus asset shows the
      "has no Nexus build attached yet" message, not a generic failure
- [x] Quick Access icon appears, click toggles the window, tooltip shows the
      bind, checkbox adds/removes it live
- [ ] Conflict guard fires with `arcdps_qol_tweaks.dll` also loaded
- [ ] Clean shutdown — no crash on game exit

## Related

The ArcDPS build lives in a separate repository. Signature updates after a GW2
patch must be applied to both; see `Signature-reference.md`.
