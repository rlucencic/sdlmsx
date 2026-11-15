# rpi5msx

An MSX1 or MSX2 for the Raspberry Pi 5, in a window of its own:
[esp32msx](https://github.com/rlucencic/esp32msx)'s MSX on the
[rpi5emu](https://github.com/rlucencic/rpi5emu) runtime.

- The Z80 with the MSX's wait states, the TMS9918A and V9938 with all their
  modes and commands, AY-3-8910 and SCC sound, floppy drives (DSK), a
  Nextor hard disk (HDD, IMG), ROM cartridges, the MSX2's mapper up to 4 MB.
- The setup screen (model, memory, video, floppy, sound, the drives), the
  web page for the disk images, the console, as the rest of the family.

## Building

See [rpi5emu](https://github.com/rlucencic/rpi5emu#building): esp32emu,
rpi5emu, esp32msx and rpi5msx cloned side by side, then

```sh
cd rpi5msx && cmake -S . -B build -G Ninja && cmake --build build
```

## ROMs

In the data folder's `roms/` (`~/rpi5emu/roms`), named as esp32msx's:
`msx.rom` (MSX1), `msx2.rom` and `msx2ext.rom` (MSX2), `disk.rom` (Disk
BASIC), `nextor.rom` (Nextor for the hard disk). Not part of this
repository.

## Running

```sh
build/rpi5msx                          # the setup screen first
build/rpi5msx -S --choice Model=MSX1   # straight into an MSX1
build/rpi5msx game.rom                 # with a cartridge in
```

Disks, hard disks and cartridges live in `~/rpi5emu/msx/`. The keyboard by
position (the international MSX keyboard): left Alt GRAPH, right Alt
CODE, F6-F10 as SHIFT with F1-F5, Home CLS/HOME, End or Pause STOP, Page Up
SELECT. The numeric keypad is the joystick in port 1 (F11 switches to 2),
F12 makes the cursor keys the joystick too (Alt A, AltGr B).

## Licence

MIT.
