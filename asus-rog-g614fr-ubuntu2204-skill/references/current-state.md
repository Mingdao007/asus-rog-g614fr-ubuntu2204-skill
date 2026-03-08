# Current State

## Machine Identity

- Model: `ASUS ROG Strix G16 G614FR`
- External keyboard used during boot diagnosis:
  - `ROCCAT Vulcan II Max`
  - USB ID `1e7d:2ee2`

## Known-Good Ubuntu Baseline

- Ubuntu kernel: `6.8.0-85-generic`
- Ubuntu package version: `6.8.0-85.85~22.04.1`
- NVIDIA driver: `580.95.05`
- Known-good boot cmdline:
  - `quiet gpiolib_acpi.run_edge_events_on_boot=0`
- Known-good GRUB state:
  - `GRUB_TIMEOUT_STYLE=hidden`
  - `GRUB_TIMEOUT=1`
  - `GRUB_CMDLINE_LINUX_DEFAULT="quiet gpiolib_acpi.run_edge_events_on_boot=0"`

## Boot Findings

- The repaired `36-37s` blank-screen delay happened before `Run /init as init
  process`.
- The confirmed slow initcall was:
  - `acpi_gpio_handle_deferred_request_irqs`
- The kernel parameter that removed the stall was:
  - `gpiolib_acpi.run_edge_events_on_boot=0`

## Healthy Linux-Side Boot

- `/init` reached in under about `1s`
- `userspace` around `3s`

## Audio Findings

- Symptom pattern:
  - built-in speakers fail
  - wired headphones still work
- The successful root cause was a missing Realtek HDA quirk for subsystem
  `1043:1054`.

## Chrome And Keyring

- Symptom pattern:
  - GDM autologin enabled
  - Google Chrome asks for a keyring password on first launch after boot
- Verified workaround on the reference machine:
  - per-user Chrome desktop overrides with `--password-store=basic`

## BIOS Notes

Safe BIOS changes to consider:

- disable `Network Stack`, `PXE`, or `HTTP Boot` if unused
- minimize any BIOS post delay if such an option exists
- test `Fast Boot`, but do not assume it is better

Do not casually change:

- storage controller mode
- Secure Boot policy
- TPM/fTPM
- GPU mux / hybrid policy
- advanced PCIe or ACPI settings
