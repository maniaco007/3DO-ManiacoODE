# 3DO ManiacoODE

**[Português](README.md)**

**New firmware and a new menu for the 3DO SataTo3DO ODE: a Portuguese-language menu with a video intro, every game on the USB drive in alphabetical order, a save manager (console, Memory Unit and USB drive), FZ-EM256 Memory Unit support and firmware updates straight from the USB drive.**

A **[Maniaco Game Room](https://www.maniacogameroom.com.br/)** blog project.

![Menu intro](imagens/abertura.png)

## What 3DO ManiacoODE brings

| | |
|---|---|
| ![Options](imagens/opcoes.png) | ![Manage saves](imagens/opcoes-saves.png) |

### Menu (`boot.iso`)

- **Menu in Portuguese**, with accents (á, ã, ç, é…).
- **Video intro** when the console is turned on. A, B, C, X or Start skips it.
- **Every game on the USB drive in one alphabetical list**, no folder browsing. **L/R** skips a full screen of games.
- **Multi-disc** games are labeled: "Title (Disco 1)", "Title (Disco 2)".
- **Screen size** from 80 % to 100 %, for TVs that crop the edges. Saved in the 3DO memory.
- **Save manager** with three tabs — **Console**, **Memory Unit** and **USB drive** ("Pendrive"): copy, restore, delete and copy all at once. Back up your console saves to the USB drive.

### Firmware (`SataTo3DO.uf2`)

- **Alphabetical list** of every game, in any folder of the USB drive.
- **Updates from the USB drive:** after the first flash, you never need to open the console or connect the ODE to a PC again. Copy `SataTo3DO.uf2` to the USB drive and turn on the 3DO.
- **FZ-EM256 Memory Unit:** the console boots with the unit plugged in, and the menu and games work normally.
- **Saves on the USB drive:** file read and write from the menu.

## How to use

1. Download **`SataTo3DO.uf2`** and **`boot.iso`** from the **[Releases](../../releases)** page.
2. **First time:** flash `SataTo3DO.uf2` to the ODE's RP2040 from a PC (BOOTSEL mode). After that, updates go through the USB drive.
3. Copy **`boot.iso`** to the root of the USB drive (keep the previous one under another name if you want to go back).
4. Create a **`saves`** folder in the root of the USB drive, for save backups.
5. Put each game in its own folder, turn on the console and pick a game.

Full step-by-step guide with every option: **[Manual](MANUAL.en.md)**.

## Requirements

- A 3DO with the **SataTo3DO ODE** (RP2040 board). Tested on a **Panasonic FZ-10**.
- USB drive formatted as **FAT32** or **exFAT**.
- Games as **`.iso`** or **`.cue` + `.bin`**.

## Warning

**Never plug or unplug the Memory Unit while the console is on** — the 3DO bus does not support it and the console locks up. Plug it in with the console off. Games and BIOS files are not provided.

## License

**Freeware — free to use. © 2026 Maniaco Game Room. All rights reserved.** See the [license](LICENCA.md) and the [third-party components](TERCEIROS.md).

Found a problem? Open an [Issue](../../issues).
