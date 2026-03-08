# ROG G614FR Ubuntu Maintenance Skill

Public Codex skill for ASUS ROG G614FR systems, including the China-market
ROG Moba 9 line, focused on Ubuntu maintenance workflows that were validated on
a working reference machine.

This repository covers:

- long blank-screen boot delays before `/init`
- keyboard-connected-at-boot instability
- BIOS and GRUB tuning
- built-in speaker recovery when wired headphones still work
- login-time mute behavior

For the Chinese guide, see [README.zh-CN.md](README.zh-CN.md).

## Skill Directory

The skill itself lives in:

- [`rog-mob9-ubuntu-maintenance`](./rog-mob9-ubuntu-maintenance)

## Main Known-Good Baseline

- Ubuntu kernel: `6.8.0-85-generic`
- Working boot parameter:
  - `gpiolib_acpi.run_edge_events_on_boot=0`
- Working GRUB shape:
  - `hidden` menu
  - short timeout
  - no `splash`

## Notes

- This is a machine-family-specific skill, not a generic Ubuntu laptop skill.
- The audio fix is narrow and model-specific.
- BIOS `Fast Boot` should be treated as a test item, not a universal default.
