# Current State

This reference captures a known-good Ubuntu baseline for ASUS ROG 魔霸9 / ROG
Strix G16 G614FR systems based on a successful repair workflow completed on
March 8, 2026.

## Machine Identity

- Model family: `ASUS ROG Strix G16 G614FR`
- Common China-market name: `ROG 魔霸9`
- External keyboard used during boot diagnosis:
  - `ROCCAT Vulcan II Max`
  - USB ID `1e7d:2ee2`

## Known-Good Ubuntu Baseline

- Ubuntu kernel: `6.8.0-85-generic`
- Ubuntu package version: `6.8.0-85.85~22.04.1`
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

## Timing Interpretation

After the fix:

- Linux-side boot can be normal even when total wall-clock boot still feels
  longer because firmware time dominates.
- A healthy Linux-side target is roughly:
  - `/init` reached in under `1s`
  - `userspace` around `3s`

## Fast Boot

- BIOS `Fast Boot` was not consistently better on the reference machine.
- Treat it as a controlled experiment, not an assumed improvement.

## Audio Findings

- The working audio fix was machine-specific.
- The important symptom pattern was:
  - built-in speakers fail
  - wired headphones still work
  - hardware is known good from Windows or another OS
- The successful root cause was a missing Realtek HDA quirk for subsystem
  `1043:1054`.

## BIOS Notes

Safe BIOS changes to consider:

- disable `Network Stack`, `PXE`, or `HTTP Boot` if unused
- minimize any BIOS post delay if such an option exists

Do not casually change:

- storage controller mode
- Secure Boot policy
- TPM/fTPM
- GPU mux / hybrid policy
- advanced PCIe or ACPI settings
