# rpi5msx: the MSX1 and MSX2 on the Raspberry Pi 5

esp32msx's MSX (`../esp32msx/components/msx`, platform independent) on the
rpi5emu runtime (`../rpi5emu`, its CLAUDE.md: the workflow, the Pi, the
lessons), the Z80 (`CONFIG_Z80_MSX`, `CONFIG_Z80_READ_SPECIAL`) esp32emu's.
The machine's own behaviour, lessons and tests are esp32msx's CLAUDE.md;
this project is the glue.

## Layout

| Path | What |
|---|---|
| `src/main.c` | The MSX as an `emu_machine_t`: esp32msx's `main/main.c` through `../rpi5emu/tools/port_glue.py` and `port_finish.py`, then: the mapper up to 4 MB as on the Tab5, a new size allocated at the restart (the boards restart into it), the ROM messages |
| `src/settings.c` | esp32msx's, over `emu/nvs.h` (section `msx`) |
| `src/rom_store.c` | `~/rpi5emu/roms/` through `emu_rom_read()` (`port_rom_store.py`): msx.rom ... nextor.rom |

Keep `src/main.c` in step with esp32msx's `main/main.c`.

## Build, run, check

```sh
../rpi5emu/tools/pi.sh rpi5msx build
../rpi5emu/tools/pi.sh rpi5msx headless     # or run: the window here
../rpi5emu/tools/console.sh i 'T FILES\n'
../rpi5emu/tools/pi.sh rpi5msx shot out.png
```

Console: the runtime's (`h`) and esp32msx's: `k <n>` tap a key, `K <n>
0|1` (48 SHIFT, 49 CTRL, 60 STOP), `T <text>` (`\n` RETURN), `j <port>
<bits>` (1 up 2 down 4 left 8 right 16 A 32 B), `R <model> [hz] [KB]`.

Checked on the Pi (2026-11-15, headless): the MSX2 (512 KB, Disk BASIC,
Nextor) at 100%, BASIC typed into; a blank disk made, a program saved and
FILES (TEST), the file in the image after quitting; the MSX1's BASIC 1.0.
Not yet: the window by hand, sound heard, games, the hard disk, 60 Hz.
