# Manual — 3DO ManiacoODE

**[Português](MANUAL.md)** · **[Back to start](README.en.md)**

The menu is in Portuguese; the on-screen names are shown in quotes with their meaning.

## Contents

1. [What you need](#1-what-you-need)
2. [First firmware install](#2-first-firmware-install)
3. [Preparing the USB drive](#3-preparing-the-usb-drive)
4. [Using the menu](#4-using-the-menu)
5. [Options](#5-options)
6. [Save manager](#6-save-manager)
7. [FZ-EM256 Memory Unit](#7-fz-em256-memory-unit)
8. [Updating the firmware from the USB drive](#8-updating-the-firmware-from-the-usb-drive)
9. [Troubleshooting](#9-troubleshooting)

## 1. What you need

- A 3DO with the **SataTo3DO ODE** (RP2040 board) installed. Tested on a **Panasonic FZ-10**.
- A USB drive formatted as **FAT32** or **exFAT**.
- The two files from the **[Releases](../../releases)** page:
  - **`SataTo3DO.uf2`** — the ODE firmware;
  - **`boot.iso`** — the menu.

## 2. First firmware install

Only the first time, flash the firmware from a PC:

1. With the console off, connect the ODE's RP2040 to the PC over USB in **BOOTSEL mode** (hold the board's BOOTSEL button while plugging it in).
2. The PC shows a drive named **RPI-RP2**.
3. Copy **`SataTo3DO.uf2`** to that drive. The board writes it and restarts on its own.

After that, updates go through the USB drive ([section 8](#8-updating-the-firmware-from-the-usb-drive)).

## 3. Preparing the USB drive

Lay out the USB drive like this:

```
boot.iso                 ← the menu
saves/                   ← folder for save backups (create it empty)
Alone in the Dark/
    Alone in the Dark.iso
Gex/
    Gex.iso
Policenauts/
    Policenauts (Disc 1).iso
    Policenauts (Disc 2).iso
```

- **`boot.iso`** goes in the root. If you had another menu, rename the old one (e.g. `boot_old.iso`) so you can go back.
- **Each game in its own folder.** A folder with a single game is shown with the **folder name**; with several files, each file name is shown.
- Supported formats: **`.iso`** (2048 or 2352 bytes per sector) and **`.cue` + `.bin`**.
- **Multi-disc** games: keep "Disc 1", "Disc 2"… in the file name and the menu shows "(Disco 1)", "(Disco 2)".
- The **`saves`** and **`covers`** folders are not shown in the game list.

## 4. Using the menu

When the console is turned on, the **video intro** plays (A, B, C, X or Start skips it) and the menu opens with **every game in alphabetical order**.

| Button | In the game list |
|---|---|
| **▲ ▼** | move up and down the list |
| **L / R** (or **◄ ►**) | skip a full screen of games |
| **A** (or **Start**) | start the game ("Jogar") |
| **X** | open the **Options** ("Opções") |

When you pick a game, the 3DO restarts and loads the disc. To go back to the menu, turn the console off and on.

## 5. Options

![Options](imagens/opcoes.png)

| Item | What it does |
|---|---|
| **"Tamanho da tela"** (screen size) | Shrinks the menu from 100 % down to 80 %, in 5 % steps, for TVs that crop the edges. Adjust with **◄ ►**; it is saved in the 3DO memory (NVRAM) when you leave with **B**. |
| **"Gerenciar saves"** (manage saves) | Opens the save manager (**A**). |
| **"Informações"** (information) | Menu version and number of games on the USB drive. |

**▲ ▼** pick the item; **B** goes back to the list.

## 6. Save manager

![Manage saves](imagens/opcoes-saves.png)

The save manager has three tabs, switched with **L / R**:

| Tab | What it shows |
|---|---|
| **Console** | the saves in the 3DO internal memory (NVRAM) and the free space |
| **Memory Unit** | the saves on the FZ-EM256 Memory Unit, if it is plugged in |
| **Pendrive** (USB drive) | the save backup in the `saves` folder of the USB drive |

Pick a save and press **A** to see the actions:

- **Copy to the USB drive / to the Memory Unit / to the console**;
- **Restore to the console / to the Memory Unit** (on the USB drive tab);
- **Delete** (or **remove from the backup**, on the USB drive tab);
- **Copy all to the USB drive** / **Restore all to the console**.

Delete, overwrite and restore all ask for confirmation. **B** goes back.

> The **Pendrive** tab only works if there is a **`saves`** folder in the root of the USB drive. If it is missing, the menu tells you.

## 7. FZ-EM256 Memory Unit

- **Plug and unplug the Memory Unit only with the console off.** With the console on, the 3DO locks up.
- With the unit plugged in, the console boots normally into the menu and games work.
- The **Memory Unit** tab looks for the unit when it is opened. If it is not plugged in, the menu shows "Memory Unit não encontrado" (not found).
- The menu **does not format** the Memory Unit: it only mounts it to read and write saves.

## 8. Updating the firmware from the USB drive

After the first install you never need to open the console again:

1. Copy the new **`SataTo3DO.uf2`** to the **root** of the USB drive.
2. Put the USB drive in the ODE and turn on the console.
3. The Pico LED (on the ODE board) stays on for a few seconds while the firmware is checked and written, and the ODE restarts on its own.
4. The file is renamed:
   - **`SataTo3DO.uf2.ok`** — updated (or it was already the same version);
   - **`SataTo3DO.uf2.erro`** — file rejected (the LED blinks slowly 5 times); the current firmware keeps working.

> **Do not turn off the console during the update.** If power fails right during the final write (about 2 seconds), flash again from a PC in BOOTSEL mode ([section 2](#2-first-firmware-install)).

## 9. Troubleshooting

| Problem | Fix |
|---|---|
| **The menu edges are cropped on the TV** | Options → **"Tamanho da tela"** → lower it to 95 % or 90 %. |
| **A game is missing from the list** | Check the extension (`.iso`, `.cue`). If the game has a `.cue`, open it in a text editor and check that the file name in `FILE "…"` matches the file in the folder. If the folder already has the `.iso`, the `.cue` can be deleted. |
| **The Pendrive tab asks you to create the folder** | Create a **`saves`** folder in the root of the USB drive. |
| **"Memory Unit não encontrado"** | Turn off the console, plug in the Memory Unit and turn it on again. |
| **The file became `SataTo3DO.uf2.erro`** | The file is corrupted or is not the ODE firmware. Download it again from the Releases page. |
| **I want the previous menu back** | Delete `boot.iso` and rename the old one back to `boot.iso`. |
