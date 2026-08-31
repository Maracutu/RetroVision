# 📁 RetroVision — Folder Structure

This is the on-disk layout of a RetroVision installation. Use it as a reference when building an `update.zip` (paths inside the zip must be **relative to the install root**, i.e. mirror the tree below).

```
RetroVision/
├─ RetroVision.vbs                 # entry point (hides console, starts the boot flow)
├─ RetroVision.bat                 # boot script: prep + intro video + launches EmulationStation
├─ RetroVision.exe                 # launcher
├─ RetroVision (console).exe       # launcher (debug console)
├─ retrovision-icon.ico
├─ RetroVision-User-Manual.pdf
│
├─ emulationstation/               # the EmulationStation front-end
│  ├─ emulationstation.exe         # the front-end binary
│  ├─ emulatorLauncher.exe         # per-emulator config + launch
│  ├─ batocera-store.cfg           # online content downloader (disabled)
│  ├─ resources/                   # global resources (splash, etc.)
│  ├─ plugins/  store/
│  └─ .emulationstation/           # main config + content
│     ├─ es_systems.cfg            # systems definitions (extensions, commands)
│     ├─ es_settings.cfg           # global + per-system settings
│     ├─ es_input.cfg  es_padtokey.cfg  es_features.cfg  ...
│     ├─ themes/                   # UI themes  →  RetroVision (active theme)
│     ├─ video/                    # boot intro videos (retrovision-*.mp4)
│     ├─ collections/  music/  scrapers/  scripts/  themesettings/  tmp/
│
├─ emulators/                      # ~141 standalone emulators, each in its own folder
│  ├─ rvupdate/                    # RetroVision self-updater
│  │  ├─ update.ps1                # checks GitHub Releases, downloads + applies update.zip
│  │  └─ update.bat                # launches the updater from the menu
│  ├─ retroarch/  pcsx2/  rpcs3/  dolphin-emu/  shadps4/  ...
│
├─ roms/                           # game STUBS, grouped by system (~249 systems)
│  └─ <system>/                    # e.g. ps5, ps4, xboxone, snes, n64, psx, ...
│     ├─ <Game>.<ext>              # ~1 KB stub containing the Google Drive link
│     ├─ gamelist.xml              # metadata + notes (Drive link shown here too)
│     ├─ images/                   # box art / logo (marquee) / thumbnails
│     ├─ videos/                   # preview videos
│     └─ manuals/                  # PDFs (where available)
│
├─ bios/                           # BIOS / firmware (per-system subfolders — see full list below)
├─ saves/                          # save files (per-system)
├─ system/                         # RetroBat internals
│  ├─ tools/                       # preparar.ps1 (runs every boot), ffplay, gamecontrollerdb
│  ├─ es_menu/                     # entries for the "Configuration / Emulators" tab
│  │  └─ Update RetroVision.menu   # the in-menu "Update RetroVision" entry
│  ├─ .rv_version                  # installed version marker (read by the updater)
│  ├─ scripts/  shaders/  decorations/  templates/  padtokey/  tattoos/  ...
│
├─ decorations/                    # bezels / overlays
├─ cheats/                         # cheat files
├─ screenshots/                    # in-game screenshots
├─ records/                        # gameplay recordings
├─ user/                           # user data
├─ library/  configs/  controles/  Drives/
```

## 🎛️ BIOS folders (`bios/`)

BIOS / firmware are organized in per-system subfolders. Place the required BIOS files inside the matching folder. **No BIOS files are included in this repository** — provide the ones you legally own.

```
bios/
├─ 32X/                 ├─ fmtowns/              ├─ mame2016/
├─ 3DO Bios/            ├─ fmtownsux/            ├─ melonDS DS/
├─ a2diskiing/          ├─ GB boot ROM/          ├─ Mupen64plus/
├─ Amiga Bios/          ├─ hatari/               ├─ neocd/
├─ bk/                  ├─ hatarib/              ├─ np2kai/
├─ bsnes-bios/          ├─ hbmame/               ├─ nxengine/
├─ bsyndrome/           ├─ HdPacks/              ├─ openlara/
├─ cannonball/          ├─ keropi/               ├─ openmsx/
├─ dc/                  ├─ kronos/               ├─ pcsx2/
├─ dinothawr/           ├─ laseractive/          ├─ PPSSPP/
├─ dolphin-emu/         ├─ mame/                 ├─ psxmame/
├─ dragon/              ├─ mame2000/             ├─ quasi88/
├─ easyrpg-player/      ├─ mame2003/             ├─ raine/
├─ eka2l1/              ├─ mame2003-plus/        ├─ same_cdi/
├─ fba/                 ├─ mame2010/             ├─ scummvm/
├─ fbalpha2012/         ├─ mame2014/             ├─ Sega CD BIOS/
├─ fbneo/               ├─ Sega Saturn Bios/     ├─ supermodel/
├─ fm7/                 ├─ swanstation/          ├─ ti99_4a/
├─ vice/                ├─ xmil/                 └─ xrick/
```

*(Some folders are region/database sub-parts: `EUR`, `JAP`, `USA`, `Databases`, `Machines`, `emulationstation`.)*

## 🔑 Key files for updates

| File | Purpose |
|---|---|
| `system/tools/preparar.ps1` | Runs on every boot: fixes emulator paths, sets branding, keeps online updaters off |
| `emulators/rvupdate/update.ps1` | The self-updater (pulls from GitHub Releases) |
| `emulationstation/.emulationstation/es_systems.cfg` | Systems + supported file extensions |
| `emulationstation/.emulationstation/themes/RetroVision/` | The active theme |
| `roms/<system>/gamelist.xml` | Per-system game metadata + notes |
| `system/.rv_version` | Current installed version |

## ☁️ The stub model

`roms/` does **not** hold full games — each entry is a lightweight **stub** (~1 KB) that carries the Google Drive link. Real games live on Google Drive and are downloaded on demand. Artwork, videos and metadata are bundled so the library looks complete before anything is downloaded.

> `.gitkeep` files mark otherwise-empty folders so the structure is preserved in git. `roms/` and `emulators/` contain many per-system / per-emulator subfolders that are not all enumerated here.
