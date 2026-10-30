# PlugExtDet

**Check, understand, manage and install the plugins of Total Commander – safely, with a preview for every change.**

PlugExtDet (*Plugin Extension Detector*) reads the plugin sections of your `wincmd.ini`, asks every plugin which file types it actually handles, and shows where the two disagree. It also explains why Lister picks a plugin – or doesn't –, shows where content plugin fields are used, and previews what each content plugin delivers for a file. Every fix goes through a preview, a backup and a verified write.

A standalone 64‑bit exe (plus a small 32‑bit helper). No installation, no registry, no runtime to install. Freeware.

**New in 2.0:** install and uninstall plugins (also from ZIP, RAR and 7z), *Check Lister* – which Lister plugin shows a file and why, gaps in `[ListerModes]` found and closed, detect strings longer than TC reads (~2 KB) caught, 32‑bit plugins queried, yellow search highlight, key bar, a completely new help.

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
- **Gap in `[ListerModes]`** – TC stops reading view modes at the first empty entry; PlugExtDet finds it and closes the gap
- **Detect string too long** – TC reads only about 2 KB of a detect string; longer ones are reported and never written
- **Folder check** – plugins in a folder that doesn't match their type are shown in orange
- **Leftovers** – sections in plugin INIs (e.g. `pkplugin.ini`) without a matching plugin
- **Not installed** – plugin files in the plugins folder that are in no INI
- **Fields of missing plugins** – column views, tooltips or thumbnails that use a plugin which is not installed

### Used in (content plugins)
- Where TC uses a content plugin's fields: column views, tooltips, thumbnails, searches, rename templates
- Shown as symbols with a count in the extra column *Used in*, spelled out in the tooltip, the detail field and the plugin info – so you know what would be missing before you remove a plugin

### Install and uninstall
- **Install** from a folder, `pluginst.inf`, a plugin file or the downloaded **ZIP, RAR or 7z** (Ctrl+I, or a TC button with `/install "%P%N"`)
- Archives are unpacked with **TC's own DLLs** (LIBDEFLATE64, UNRAR64, TC7Z64) in a separate process; paths with `..`, links, encrypted entries and archive bombs are refused
- The dialog shows files (32/64 bit), target (editable), extensions and a **check**: who comes first, which extensions another plugin already has; updates keep your own detect string on request
- **Uninstall** removes the entries and, if the folder belongs to the plugin alone, moves it to the recycle bin – or just **disable** it
- Works without a running TC: the TC folder is remembered

### Preview
- **Preview tab** – pick a file and see what every content plugin delivers: field, value, readable type and where it is used
- **Sidebar** with all content plugins and how many fields have a value; the plugin that knows most about the file opens on the right, tick more as you like – remembered per extension for the session
- **tc (pseudo)** – TC's own basic fields (size, dates, attributes, version, comment …), read by PlugExtDet from Windows, with the real field reference `[=tc.…]`
- Matching plugins are asked in the background, each in its own short-lived process with a time limit; others on request; nothing is ever written
- Units on request, ready field reference `[=plugin.field]` to copy
- Call it from a TC button: `PlugExtDet.exe /preview "%P%N"` – a running PlugExtDet receives the file
- **Check Lister** (`/lister "%P%N"`) – all Lister plugins for one file in TC's order: what TC decides first (view mode, own viewer, blocks), *matches with F3*, *only with key 4*, *does not match* – each with its reason, the detect string part by part, and the **key 4 cycle**; right-click: *Always use … for .EXT* or *Move before the other candidates*

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
3. TC is closed via its window (no process killing), the file is written – PlugExtDet remembers which TC was running (32 or 64 bit)
4. The result is read back and verified
5. Every change is logged, and exactly the TC that PlugExtDet closed is started again – none if TC wasn't running
6. *File › Restore last backup* undoes it

### Keep track
- **Inventory** – records which plugins were present at first start, which came later, which were removed and when versions changed
- New plugins are highlighted for a configurable number of days
- Plugin info (Alt+Enter): version, architecture, build time, capabilities from the exports
- Extra columns (right-click the column headers): description, company, bits, build date, capabilities, size …
- *File › Clean up* moves old backups and logs to the recycle bin
- If the INI is changed outside PlugExtDet (by hand or by TC), it offers to read it again

### Comfort
- Colors: Standard (Windows), Light, Dark (eggshell text) – dark title bar on Windows 10/11, high contrast respected
- Help as CHM with *I want to …* tasks, a reference for every key and menu, background and terms A–Z (F1 opens the page of the current tab, Shift+F1 the symbols)
- **Find** (Ctrl+F) highlights every match in yellow, like Explorer
- **Key bar** at the bottom like in TC
- English and German built in; Russian, Ukrainian, Czech and Danish included as example files; any language can be added
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
4. Optional: TC buttons with `/preview "%P%N"`, `/lister "%P%N"` or `/install "%P%N"` for the file under the cursor

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
| Ctrl+I | Install plugin |
| Ctrl+F, F3 | Find, next match |
| Ctrl+S | Save changes |
| Ctrl+E | Settings |
| F5 | Reload |

## Files

| File | Purpose |
|---|---|
| `PlugExtDet.exe` | the program |
| `PlugExtDet32.exe` | helper that asks 32-bit-only plugins (started by PlugExtDet.exe only) |
| `Language\*_example.lng` | translations as examples (UTF-16) – remove `_example` from the name to use one |
| `PlugExtDet_eng.chm`, `PlugExtDet_deu.chm` | help |
| `PlugExtDet.ini` | settings, remembered and deactivated entries, plugins skipped in the preview; created on first start |
| `PlugExtDet.inventory.ini`, `PlugExtDet.log` | inventory and change log |
| `Backups\` | backups made before every save |

If the program folder is write-protected, the data files go to `%APPDATA%\PlugExtDet`.

## Translations

Every language is its own file in the folder `Language`. They are shipped as **examples** only – `rus_example.lng`, `ukr_example.lng`, `cze_example.lng`, `dan_example.lng` and `eng_example.lng` (all English, as a template). PlugExtDet never reads a file with "example" in its name.

**To use a language:** rename the file and remove `_example`, e.g. `ukr_example.lng` → `ukr.lng`. An update only brings new `*_example.lng` files, so your own file is never overwritten. After an update, compare it with the new example file and copy over the lines you are missing.

The language is the section name in the first line, e.g. `[ukr]` – the same code as TC's language file (`WCMD_UKR.LNG` → `ukr`); the file name does not matter. With *auto*, PlugExtDet follows TC's language.

**New language:**
1. Copy `eng_example.lng`, e.g. as `pol.lng`, and change the first line to `[pol]`
2. Translate the text right of `=`, keep placeholders like `{n}`, `\t`, `\n` and `&`
3. Set `lang_name=Polski (Polish)` for the language selection
4. Save as UTF-16 LE

Lines starting with `;` are not translated yet – translate them and remove the `;`. Contributions are welcome in the TC forum or via issues or pull requests.

## Good to know

- 32-bit-only plugins are queried through the small helper `PlugExtDet32.exe` (same source, built as 32-bit). Without it they are listed but not compared or previewed. Note: a 64-bit TC still cannot load them.
- TC's own fields (`tc.*`) are read by PlugExtDet itself as *tc (pseudo)* – almost exactly like TC; file system plugin fields (`<fs>`) only exist inside TC
- Some plugins start on purpose only inside TC (e.g. AudioInfo); they work normally there and are shown neutrally (grey dot), not as an error
- Windows shows CHM help from network drives only as a table of contents – copy the help locally
- Unsigned tools are occasionally flagged by machine-learning virus scanners; such reports are submitted to the vendor as false positives

## Thanks

PlugExtDet grew up in the Total Commander forum. A big thank you to everyone who asked, tested and suggested – especially **AntonyD** (the reason for the whole program; tooltips and search), **BEB** (preview, collapsed groups, sidebar, many tests, Ukrainian translation), **petermad** (detect-string and MULTIMEDIA tests, Danish translation), **macleod** (Czech translation) and **TychoBarfy** (the idea for "Used in").

And a big thank you to **Christian Ghisler** for Total Commander – without it none of this would be possible.

## Support

PlugExtDet is free. If you would like to support the work: [ko-fi.com/native2904](https://ko-fi.com/native2904) – thank you!

## License

Freeware – see `LICENSE.txt`.
