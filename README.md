# PlugExtDet

**Check and correct the plugin entries of Total Commander – safely, with a preview for every change.**

PlugExtDet (*Plugin Extension Detector*) reads the plugin sections of your `wincmd.ini`, asks every plugin which file types it actually handles, and shows where the two disagree. Missing extensions after an update, stale entries, plugins in the wrong folder, detect strings TC can't parse – everything ends up in one list, and every fix goes through a preview, a backup and a verified write.

A standalone 64‑bit exe. No installation, no registry, no runtime to install. MIT licensed.

```
 Lister │ Content │ Packer │ File system │ Inventory │ Findings
────────┴─────────┴────────┴─────────────┴───────────┴──────────
 #  Plugin        Check   Path                      │ Extension  Status
 0  ImageViewer    ±      %COMMANDER_PATH%\wlx\…    │ ☑ PDF      active
 1  MarkdownView   =      %COMMANDER_PATH%\wlx\…    │ ☑ DJVU     active, not from plugin
 2  OldViewer      ×      %COMMANDER_PATH%\wlx\…    │ ☐ EPUB     new in plugin
```

---

## Why

Over the years a TC configuration collects small inconsistencies nobody notices:

- a plugin update adds new formats, but your INI still has the old detect string,
- extensions were added by hand and later forgotten,
- a `.wlx` landed in the `wcx` folder,
- a detect string like `ext=JPG` (no quotes) makes TC report *Plugin detect string error*,
- the same plugin is registered twice, or a removed plugin left its section behind.

PlugExtDet makes these visible – and fixable – in a few seconds.

## Features

### Compare
- **All four plugin types** – Lister, Content, Packer and File system, each in its own tab
- **Asks the plugins themselves** – detect strings via `ListGetDetectString` / `ContentGetDetectString`, packer capabilities via `GetPackerCaps`, file system plugins via their exports
- **Safe querying** – every plugin is loaded in a separate, short-lived process; a crashing or hanging plugin never takes PlugExtDet or TC down
- **Understands TC's syntax** – `MULTIMEDIA`, `FORCE`, wildcards, `FIND()`, negations with `!` and `!=`
- **RedirectSection aware** – works with configurations split into several INI files

### Find
- **Findings tab** – errors, differences and notes in one list; Enter jumps to the entry
- **Mark as checked** (Ins) – intended oddities stay out of the way until something changes
- **Folder check** – plugins in a folder that doesn't match their type are shown in orange
- **Leftovers** – sections in plugin INIs (e.g. `pkplugin.ini`) without a matching plugin

### Fix
- **Checkboxes** – switch single extensions on or off
- **Plugin-friendly editing** – `MULTIMEDIA & (…)` and `FORCE & MULTIMEDIA & (…)` are edited inside the brackets; exclusion lists `MULTIMEDIA & !(…)` can be maintained the same way
- **Take the plugin's detect string** with one click, or edit any entry as text (F4) and delete it (Del) – the TC way
- **Remembers** extensions you added yourself and later unchecked, so they can be switched back on

### Save – carefully
1. Preview of every change, old and new side by side
2. Backup of each file before it is touched
3. TC is closed via its window (no process killing), the file is written
4. The result is read back and verified
5. Every change is logged, TC is started again
6. *File › Restore last backup* undoes it

### Keep track
- **Inventory** – records which plugins were present at first start, which came later, which were removed and when versions changed
- New plugins are highlighted for a configurable number of days
- Plugin info (Alt+Enter): version, architecture, build time, capabilities from the exports

### Comfort
- Colors: Standard (Windows), Light, Dark – dark title bar on Windows 10/11, high contrast respected
- Help as CHM (F1 opens the chapter of the current tab), legend with every symbol (Shift+F1)
- English and German built in; Russian, Ukrainian and Czech included; any language can be added
- Language follows TC automatically and can be switched without restart
- Resizable panes, sortable columns, own commands in the context menu

## Requirements

- Windows 7 or later, 64-bit
- Total Commander (portable installations are supported)

## Getting started

1. Unpack the zip into any folder, keep the files together
2. Start `PlugExtDet.exe` – it finds TC's `wincmd.ini` by itself (or *File › Open wincmd.ini…*)
3. Look at the **Findings** tab first – it's the quickest overview

Nothing is written until you press **Ctrl+S** and confirm the preview.

## Keys

| Key | Action |
|---|---|
| F1 / Shift+F1 | Help / Legend |
| F4 | Edit entry |
| Del | Delete entry (or uncheck extension) |
| Ins | Mark finding as checked |
| Enter / Backspace | Jump to entry / back to findings |
| Alt+Enter | Plugin info |
| Ctrl+S | Save changes |
| Ctrl+E | Settings |
| F5 | Reload |

## Files

| File | Purpose |
|---|---|
| `PlugExtDet.exe` | the program |
| `PlugExtDet.lng` | translations (UTF-16) |
| `PlugExtDet_eng.chm`, `PlugExtDet_deu.chm` | help |
| `PlugExtDet.ini` | settings, created on first start |
| `PlugExtDet.inventory.ini`, `PlugExtDet.log` | inventory and change log |
| `Backups\` | backups made before every save |

If the program folder is write-protected, the data files go to `%APPDATA%\PlugExtDet`.

## Translations

Every language is a section in `PlugExtDet.lng`, named like TC's language file (`WCMD_POL.LNG` → `[pol]`):

1. Copy the `[eng]` section and rename it
2. Translate the text right of `=`, keep placeholders like `{n}`, `\t`, `\n` and `&`
3. Add `lang_name=Polski (Polish)` for the language selection
4. Save as UTF-16 LE

After an update, new texts appear at the end of each section in English with a `;` in front – translate them and remove the `;`. Contributions are welcome via issues or pull requests.

## Building

MinGW-w64 (GCC), no other dependencies:

```bat
build.bat
```

or by hand:

```bat
windres resource.rc -O coff -o resource.o
g++ -std=c++17 -O2 -s -static -municode -mwindows -DUNICODE -D_UNICODE ^
    main.cpp resource.o -o PlugExtDet.exe ^
    -lcomctl32 -luxtheme -lcomdlg32 -lversion -lshlwapi -ldwmapi
```

Help: `python help\build_help.py <version>` (needs `chmcmd` from Free Pascal).
Language file: `python lang\build_lng.py`.

## Good to know

- 32-bit-only plugins can't be queried by the 64-bit exe; they are listed but not compared
- Some plugins refuse to load outside TC (Windows error 1114); they are marked, not treated as broken
- Windows shows CHM help from network drives only as a table of contents – copy the help locally
- Unsigned self-built tools are occasionally flagged by machine-learning virus scanners; the source is here to check

## License

MIT – see `LICENSE`.
