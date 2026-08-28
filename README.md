[![Downloads](https://img.shields.io/github/downloads/proprene/qol-tweaks-nexus/latest/qol-tweaks-nexus.dll)](https://github.com/proprene/qol-tweaks-nexus/releases/latest)

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
