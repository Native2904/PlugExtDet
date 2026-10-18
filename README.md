# PlugExtDet

**Check and correct the plugin entries of Total Commander – safely, with a preview for every change.**

PlugExtDet (*Plugin Extension Detector*) reads the plugin sections of your `wincmd.ini`, asks every plugin which file types it actually handles, and shows where the two disagree. It also explains why Lister picks a plugin – or doesn't –, shows where content plugin fields are used, and previews what each content plugin delivers for a file. Every fix goes through a preview, a backup and a verified write.

A standalone 64‑bit exe. No installation, no registry, no runtime to install. Freeware.

```
 Lister │ Content │ Packer │ File system │ Inventory │ Findings │ Preview
────────┴─────────┴────────┴─────────────┴───────────┴──────────┴─────────
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
- the same plugin is registered twice, or a removed plugin left its section behind,
- a plugin near the top matches every file, so the specialists behind it never get their turn,
- a column view still shows fields of a plugin you removed long ago (`<?>`).

PlugExtDet makes these visible – and fixable – in a few seconds.

## Features

### Compare
- **All four plugin types** – Lister, Content, Packer and File system, each in its own tab
- **Asks the plugins themselves** – detect strings via `ListGetDetectString` / `ContentGetDetectString`, packer capabilities via `GetPackerCaps`, file system plugins via their exports
- **Safe querying** – every plugin is loaded in a separate, short-lived process; a crashing or hanging plugin never takes PlugExtDet or TC down
- **Understands TC's syntax** – `MULTIMEDIA`, `FORCE`, wildcards, `FIND()`, negations with `!` and `!=`
- **RedirectSection aware** – works with configurations split into several INI files

### Understand how Lister chooses
- **TC's own order** – `NoMediaFileTypes`, `NoPluginFileTypes`, view mode by file type (`[ListerModes]`), plugin order and TC's own viewers are taken into account
- **Status per extension** – active, *blocked* by TC, or *passed over* (rule in TC, or a type TC shows itself, which needs `MULTIMEDIA`)
- **Catch-all plugins** – plugins that match (almost) every file are shown in blue; extra column *Reach*

### Find
- **Findings tab** – errors, differences and notes in one list, written in plain language: what is going on, what you notice in TC, and what you can do; Enter jumps to the entry
- **Mark as checked** (Ins) – intended oddities stay out of the way until something changes
- **Folder check** – plugins in a folder that doesn't match their type are shown in orange
- **Leftovers** – sections in plugin INIs (e.g. `pkplugin.ini`) without a matching plugin
- **Not installed** – plugin files in the plugins folder that are in no INI
- **Fields of missing plugins** – column views, tooltips or thumbnails that use a plugin which is not installed

### Used in (content plugins)
- Where TC uses a content plugin's fields: column views, tooltips, thumbnails, searches, rename templates
- Shown as symbols with a count in the extra column *Used in*, spelled out in the tooltip, the detail field and the plugin info – so you know what would be missing before you remove a plugin

### Preview
- **Preview tab** – pick a file and see what every content plugin delivers: field, value, readable type and where it is used
- Only plugins whose detect string matches the file are asked (optionally all), each in its own short-lived process with a time limit; nothing is ever written
- Units on request, ready field reference `[=plugin.field]` to copy
- Call it from a TC button: `PlugExtDet.exe /preview "%P%N"` – a running PlugExtDet receives the file

### Fix
- **Checkboxes** – switch single extensions on or off
- **Plugin-friendly editing** – extensions are cut out of or added to their bracket, the rest of the string stays exactly as written; works with several brackets like `FORCE & (…) | MULTIMEDIA & (…)`; exclusion lists `MULTIMEDIA & !(…)` can be maintained the same way
- **Take the plugin's detect string** with one click, or edit any entry as text (F4) and delete it (Del) – the TC way
- **Edit with help** – a syntax line checks the detect string while you type, Tab completes keywords and extensions, and PlugExtDet asks before you apply a faulty string
- **Change the order** of Lister and Content plugins – select several, move them together, gather scattered ones into a block (Ctrl+↑/↓, Ctrl+Home/End)
- **Deactivate** a plugin without touching its files – the entry is kept and can be activated again at its old place
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
- Extra columns (right-click the column headers): description, company, bits, build date, capabilities, size …
- *File › Clean up* moves old backups and logs to the recycle bin
- If the INI is changed outside PlugExtDet (by hand or by TC), it offers to read it again

### Comfort
- Colors: Standard (Windows), Light, Dark – dark title bar on Windows 10/11, high contrast respected
- Help as CHM (F1 opens the chapter of the current tab), legend with every symbol (Shift+F1)
- English and German built in; Russian, Ukrainian, Czech and Danish included; any language can be added
- Language follows TC automatically and can be switched without restart
- Resizable panes, sortable columns, own commands in the context menu
- Long paths are shortened in the middle; the tooltip shows the full path

## Requirements

- Windows 7 or later, 64-bit
- Total Commander (portable installations are supported)

## Getting started

1. Unpack the zip into any folder, keep the files together
2. Start `PlugExtDet.exe` – it finds TC's `wincmd.ini` by itself (or *File › Open wincmd.ini…*)
3. Look at the **Findings** tab first – it's the quickest overview
4. Optional: add a TC button with `PlugExtDet.exe /preview "%P%N"` to preview the selected file

Nothing is written until you press **Ctrl+S** and confirm the preview.

## Keys

| Key | Action |
|---|---|
| F1 / Shift+F1 | Help / Legend |
| F4 | Edit entry |
| Del | Delete entry (or uncheck extension) |
| Ins | Mark finding as checked |
| Ctrl+↑ / Ctrl+↓ | Move selected plugins up / down (Lister, Content) |
| Ctrl+Home / Ctrl+End | Move selected plugins to top / bottom |
| Enter / Backspace | Jump to entry / back to findings |
| Alt+Enter | Plugin info |
| Tab (in F4) | Complete keyword or extension |
| Ctrl+S | Save changes |
| Ctrl+E | Settings |
| F5 | Reload |

## Files

| File | Purpose |
|---|---|
| `PlugExtDet.exe` | the program |
| `PlugExtDet.lng` | translations (UTF-16) |
| `PlugExtDet_eng.chm`, `PlugExtDet_deu.chm` | help |
| `PlugExtDet.ini` | settings, remembered and deactivated entries, plugins skipped in the preview; created on first start |
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

## Good to know

- 32-bit-only plugins can't be queried by the 64-bit exe; they are listed but not compared or previewed
- TC's own fields (`tc.*`) and file system plugin fields (`<fs>`) are not plugins and can't be previewed
- Some plugins refuse to load outside TC (Windows error 1114); they are marked, not treated as broken
- Windows shows CHM help from network drives only as a table of contents – copy the help locally
- Unsigned tools are occasionally flagged by machine-learning virus scanners; such reports are submitted to the vendor as false positives

## License

Freeware – see `LICENSE.txt`.
