<h1 align="center">rpi5msx</h1>

<p align="center">
  <b>An MSX1 or MSX2 on the Raspberry Pi 5, in a window of its own.</b><br>
  Z80 with the MSX's wait states, TMS9918A and V9938 with all their modes and commands, AY-3-8910 and SCC sound, floppy drives, a Nextor hard disk, ROM cartridges.
</p>

<p align="center">
  <img alt="Raspberry Pi 5" src="https://img.shields.io/badge/Raspberry%20Pi-5-c51a4a">
  <img alt="Linux arm64" src="https://img.shields.io/badge/Linux-arm64-333">
  <img alt="SDL 3" src="https://img.shields.io/badge/SDL-3-1d4ed8">
  <img alt="CPU" src="https://img.shields.io/badge/CPU-Z80-555">
  <img alt="MIT licence" src="https://img.shields.io/badge/licence-MIT-green">
</p>

<p align="center">
  <b>Work in progress:</b> not everything has been tested, and games and demos may or may not
  work satisfactorily.
</p>

<p align="center">
  <img alt="MSX BASIC 2.1 with Disk BASIC on the MSX2: a program saved to a new disk and FILES" src="docs/images/msx2-files.png" width="49%">
  <img alt="MSX BASIC 1.0 on the MSX1 with Disk BASIC" src="docs/images/msx1-basic.png" width="49%">
  <br>
  <sub>The MSX2 saving a program to a new disk, and the MSX1, on the Pi.</sub>
</p>

rpi5msx runs [esp32msx](https://github.com/rlucencic/esp32msx)'s MSX on a Raspberry Pi 5 with
64-bit Linux, in a window of its own (X11 or Wayland, through SDL 3). The machine is the
same code as on the ESP32 boards: esp32msx's platform-independent core and
[esp32emu](https://github.com/rlucencic/esp32emu)'s CPU core, on
[rpi5emu](https://github.com/rlucencic/rpi5emu), the family's Linux runtime: a setup
screen in the window, sound, the Pi's keyboard and mouse, a web page on your network and a
console.

It's one of the rpi5 family, the Raspberry Pi versions of the esp32 emulators:
[rpi5zx](https://github.com/rlucencic/rpi5zx) (ZX Spectrum),
[rpi5cpc](https://github.com/rlucencic/rpi5cpc) (Amstrad CPC),
[rpi5cbm](https://github.com/rlucencic/rpi5cbm) (Commodore 64 and 128),
[rpi5coco](https://github.com/rlucencic/rpi5coco) (Tandy Color Computer 2 and 3),
[rpi5gs](https://github.com/rlucencic/rpi5gs) (Apple IIGS),
[rpi5xl](https://github.com/rlucencic/rpi5xl) (Atari 800XL and 130XE),
[rpi5ste](https://github.com/rlucencic/rpi5ste) (Atari STE),
[rpi5pc](https://github.com/rlucencic/rpi5pc) (IBM PC/XT and PC/AT),
[rpi5mac](https://github.com/rlucencic/rpi5mac) (Macintosh Plus, SE, Classic),
[rpi5mac2](https://github.com/rlucencic/rpi5mac2) (Macintosh LC and II),
[rpi5mac3](https://github.com/rlucencic/rpi5mac3) (Macintosh LC II and LC III),
[rpi5arc](https://github.com/rlucencic/rpi5arc) (Acorn Archimedes),
[rpi5ami](https://github.com/rlucencic/rpi5ami) (Amiga 500, 2000, 600, 1200).

## Features

**The machines**
- **MSX1** (64 KB, TMS9918A, or TMS9929A at 50 Hz) and **MSX2** (a memory
  mapper of 128 KB to 4 MB; V9938 with 128 KB of VRAM,
  RP5C01 clock), chosen on the setup screen with **50 or 60 Hz** video
- **Z80** at 3.58 MHz on the shared core, with the wait state the MSX engine
  adds to every M1 cycle
- **Slots and subslots** as an MSX lays them out: BIOS and BASIC, the
  cartridge, the memory mapper, the MSX2's SUB-ROM, the disk ROM, Nextor
- **TMS9918A / V9938**: text 40 and 80 columns (with blink), the graphic
  modes G1 to G7, multicolour, sprite modes 1 and 2 (eight a line, a colour
  per line), the 512-colour palette, vertical scroll, line interrupts and all
  the V9938's commands (lines, fills, block moves, logical operations)
- **Sound**: the AY-3-8910's three channels and noise, the key click, and
  Konami's SCC in SCC cartridges
- **Keyboard** by position (international layout), **joystick** on the
  numeric keypad or the cursor keys

**Drives**
- **Two floppy drives** behind a WD2793, as Philips built the disk interface:
  `.dsk` images (720 KB and 360 KB), read and written in place, new
  formatted disks on request. **Floppy Off** leaves the disk interface out,
  as on an MSX without drives: BASIC starts at once and, without a hard
  disk, has 28815 bytes free instead of 23430 (the disk ROM keeps its work
  area at the top of RAM), which some cassette-era programs need. A Nextor
  hard disk works either way
- **A hard disk** on a Sunrise IDE interface with **Nextor**: a raw image of
  any size, read and written in place; new ones come with a FAT16 partition
  (32 or 128 MB). With a hard disk in, Nextor starts first: the hard disk is
  A: and the floppies B: and C:, as on an MSX with a Nextor cartridge
- **ROM cartridges**: plain ROMs up to 64 KB and MegaROMs (ASCII 8, ASCII
  16, Konami, Konami SCC), the mapper found from the code

**Around it**
- **A window** of its own on the Pi's desktop (X11 or Wayland), resizable, full screen
  with Ctrl+Alt+F; the picture in the machine's shape, sharp pixels
- **Setup screen** in the window, first thing at start and any time with Ctrl+Alt+M:
  model, memory, video, floppy, the sound's volume, the drives and their images (insert, eject, new
  blank ones), Start / Back / Restart, Reset
- **The Pi's keyboard and mouse**: keys by position, as the boards' Bluetooth keyboards;
  a click in the window gives the mouse to the machine, Ctrl+Alt+G takes it back
- **Web page** at `http://<the Pi>:8080/`: drop images (or ZIP files) onto it, insert
  them, make blank ones, and watch the machine and its drives live
- **Sound** through SDL (PipeWire, PulseAudio or ALSA), the machine paced by the clock
- **Console** on the terminal and on `127.0.0.1:6400`: keys, typing, drives, screenshots

<p align="center">
  <img src="docs/images/setup.png" width="80%" alt="The setup screen in the window: the MSX's choices, the sound, Disks and Start">
  <br><sub>The setup screen in the window, before the MSX starts</sub>
</p>

## Getting started

### 1. What you need

- A Raspberry Pi 5 with 64-bit Linux (developed on Ubuntu 26.04; any Linux with SDL 3.2 or
  later should do)
- The tools and libraries:
  `sudo apt install build-essential cmake ninja-build pkg-config libsdl3-dev libsdl3-ttf-dev libpng-dev`
- [esp32emu](https://github.com/rlucencic/esp32emu), [rpi5emu](https://github.com/rlucencic/rpi5emu)
  and [esp32msx](https://github.com/rlucencic/esp32msx), cloned next to this project
- The ROMs (below)

### 2. Build

```sh
git clone https://github.com/rlucencic/esp32emu.git
git clone https://github.com/rlucencic/rpi5emu.git
git clone https://github.com/rlucencic/esp32msx.git
git clone https://github.com/rlucencic/rpi5msx.git && cd rpi5msx
cmake -S . -B build -G Ninja && cmake --build build
```

### 3. ROMs

In the data folder's `roms/` (`~/rpi5emu/roms`), named as esp32msx's:
`msx.rom` (MSX1), `msx2.rom` and `msx2ext.rom` (MSX2), `disk.rom` (Disk
BASIC), `nextor.rom` (Nextor for the hard disk). Not part of this
repository.

### 4. First start

1. `build/rpi5msx`: the setup screen opens in a window.
2. Choose the model and click **Start**.
3. Put images into `~/rpi5emu/msx/`, drop them on the window or the web page, and
   insert them on the setup screen's **Disks** page.

## Using it

| To | Do |
|---|---|
| Get back to the setup screen | Ctrl+Alt+M (the machine waits meanwhile) |
| Full screen | Ctrl+Alt+F |
| Give the mouse to the machine, take it back | A click in the window; Ctrl+Alt+G |
| Run as fast as it goes (warp) | Ctrl+Alt+W, again for real speed |
| Take a screenshot | Ctrl+Alt+S (into `~/rpi5emu/screenshots`), or the console's `s` |
| Insert an image | The setup screen's **Disks** page, drop it on the window or the web page, or name it on the command line: `build/rpi5msx game.rom` |
| Start straight into the machine | `build/rpi5msx -S`, with the setup screen's rows as `--choice`: `build/rpi5msx -S --choice Model=MSX1` |
| Show the window on another computer | Log in with `ssh -C -Y <the Pi>` and start it there: fine for still pictures; games want the Pi's own monitor |
| Load or save a program | `SAVE"A:NAME.BAS"`, `LOAD"A:NAME.BAS"`, `FILES` |

Images live in `~/rpi5emu/msx/`, settings in `~/.config/rpi5emu/settings.ini`; the
inserted images are remembered across starts. `build/rpi5msx --help` lists the options.

**Keyboard**: by position, the international MSX layout on a US keyboard:
Ctrl, Shift and Caps Lock as themselves, left Alt GRAPH, right Alt CODE,
F1-F5 as themselves and F6-F10 as SHIFT with F1-F5, Home CLS/HOME, Insert
INS, Delete DEL, End or Pause STOP, Page Up SELECT.

**Joystick**: the numeric keypad (8 4 6 2 and the diagonals, 0 or 5 trigger
A, `.` trigger B) in port 1; F11 switches it to port 2. F12 makes the cursor
keys the joystick too (Alt trigger A, AltGr trigger B).

## How it works

```mermaid
flowchart LR
  subgraph emu [Emulation thread]
    EMU["Emulation<br>Z80 · slots · VDP<br>PSG · SCC · FDC · IDE"]
  end
  subgraph main [Main thread]
    WIN["Window · setup screen<br>SDL 3"]
  end
  subgraph others [Other threads]
    WEB[Web page :8080]
    CON[Console :6400]
  end
  EMU -- picture --> WIN
  EMU -- sound --> SND[SDL audio]
  WIN -- keys · mouse --> EMU
  DATA[("~/rpi5emu<br>roms · msx")]
  EMU <--> DATA
  WEB <--> DATA
```

- **[esp32msx](https://github.com/rlucencic/esp32msx)'s `components/msx`**: the MSX,
  platform independent: the same code runs on the ESP32 boards.
- **`src/`**: the glue, esp32msx's `main/main.c` ported: memory, ROMs, settings, keyboard map,
  drives and status.
- **[rpi5emu](https://github.com/rlucencic/rpi5emu)**: the window, the setup screen, sound,
  input, the web page, the console, the data folder and the settings.
- **[esp32emu](https://github.com/rlucencic/esp32emu)**: the CPU core (Z80).

## Development

```sh
../rpi5emu/tools/pi.sh rpi5msx build        # from another computer: sync to the Pi, build there
../rpi5emu/tools/pi.sh rpi5msx run          # its window on this computer (ssh -C -Y)
../rpi5emu/tools/pi.sh rpi5msx headless     # no window: drive it through the console
../rpi5emu/tools/console.sh i 's /tmp/shot.png'
```

`CLAUDE.md` has the details: the layout, how it was ported, the checks; esp32msx's own
`CLAUDE.md` has the machine's.

## Limitations

- Checked on the Pi through its console and screenshots: the MSX2 at full speed, BASIC, a program saved to a new disk and FILES; the MSX1. Not yet by hand
  in the window, with the sound heard, or with much software.
- Over X forwarding (`ssh -Y`) every changed picture crosses the network: 8 to 15 a second.
- Tested so far with MSX1 and MSX2 BASIC, the screen modes, sprites and the
  V9938's commands, floppies, Nextor on a hard disk, and test cartridges for
  each mapper and the SCC; **games haven't been tried yet**.
- No cassette, printer, MSX-MUSIC (FM-PAC) or MSX-AUDIO; no MSX2+ or turbo R;
  `.dsk` images only (no `.dmk`); one hard disk; cartridges in one slot.
- Floppies are quicker than a real drive: a sector is there as soon as it's
  asked for.

## Credits

- [openMSX](https://openmsx.org): the reference for the Sunrise IDE and many
  details of the machines
- [fMSX](https://fms.komkon.org/fMSX/) by Marat Fayzullin: the way to tell
  MegaROM mappers apart
- [Nextor](https://github.com/Konamiman/Nextor) by Konamiman
- [SDL](https://libsdl.org) 3 for the window, the sound and the input

## Licence

[MIT](LICENSE).

MSX is a trademark of MSX Licensing Corporation. The ROM images are not part
of this repository.
