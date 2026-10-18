# Factory Prusa MINI display

The factory ST7789V display and rotary encoder are enabled by
`mini_display.cfg`. Klipper itself does not currently ship this display driver,
so the host-side changes are preserved in
`patches/klipper-prusa-mini-st7789v.patch`.

## Reapply after a Klipper update

From `/home/pi/klipper`, first verify that the working tree is clean, then run:

```sh
git apply --check /home/pi/printer_data/config/patches/klipper-prusa-mini-st7789v.patch
git apply /home/pi/printer_data/config/patches/klipper-prusa-mini-st7789v.patch
```

Restart the `klipper` service from Mainsail's machine controls. A normal
`RESTART` command is not sufficient after changing Python source files.

## Disable without removing the driver

Comment out `[include mini_display.cfg]` in `printer.cfg`, then restart Klipper.
This returns printer control without changing MCU firmware.

## Pin mapping

- SPI bus: `spi2a` (`PC2`, `PC3`, `PB10`)
- Reset: `PC8`
- Chip-select: `PC9`
- Command/data: `PD11`
- Encoder: `PE13`, `PE15`
- Encoder click: `PE12`

These values match Jeric's archived, previously used MINI configuration.

## Jeric dashboard and controls

The idle/printing dashboard shows temperatures, printer state, progress,
filename, elapsed time, loaded filament, and tracked Revo position. Pressing
the knob opens controls for selecting/starting a file, pause/resume, confirmed
safe stop, and tracked filament loading/unloading. Safe stop calls the existing
Revo-aware `CANCEL_PRINT` macro.
