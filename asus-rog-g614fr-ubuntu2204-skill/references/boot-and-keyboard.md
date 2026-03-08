# Boot And Keyboard Workflow

Use this reference for two cases:

- Ubuntu 22.04 + `6.8.0-85-generic`: `30-40s` blank screen before `/init`
- `ROCCAT Vulcan II Max` (`1e7d:2ee2`): boot changes or hangs when attached

## Core Diagnosis

The repaired `30-40s` boot delay on the reference machine was not a GNOME or
GDM problem. It happened before `Run /init as init process`.

The confirmed slow initcall was:

- `acpi_gpio_handle_deferred_request_irqs`

The fix was:

```text
gpiolib_acpi.run_edge_events_on_boot=0
```

## Branch 1: Long Blank Screen Before `/init`

Use this branch if the machine again spends tens of seconds apparently doing
nothing before the normal initramfs or login path starts.

1. Confirm whether the current cmdline already includes:
   - `gpiolib_acpi.run_edge_events_on_boot=0`
2. If not, restore it before deeper tuning.
3. If the stall is still present, boot once with:

```text
initcall_debug ignore_loglevel printk.time=1
```

4. Check for a new slow initcall instead of assuming the old one still applies.

Interpretation rule:

- if the long gap is still before `/init`, stay focused on kernel or ACPI
- do not jump straight to GNOME, GDM, or user services

## Branch 2: Keyboard Changes Boot Behavior

Use this branch when boot behavior changes only when a USB keyboard is attached.

Start narrow:

1. Use the more stable cable first if the keyboard offers two USB leads.
2. Keep the same USB port during comparison.
3. Remove other nonessential USB devices.
4. Reproduce on the current known-good Linux baseline before changing BIOS
   settings.

If the hang becomes reproducible:

1. Boot without the keyboard
2. Boot with the keyboard attached
3. If needed, boot with:
   - `systemd.unit=multi-user.target`

Use that split to decide whether the problem is:

- early USB or enumeration behavior
- or only the graphical boot chain

Do not default to testing the worse cable unless proving a power or enumeration
boundary is actually useful.

## Branch 3: BIOS And GRUB Tuning

Once the pre-`/init` stall is fixed and userspace boot is already fast, prefer
these lower-risk tuning steps:

- keep GRUB hidden with a short timeout
- remove `splash` if a cleaner boot path is preferred
- disable NVIDIA CDI refresh only if the machine does not need NVIDIA GPU containers
- disable BIOS `Network Stack`, `PXE`, or `HTTP Boot` if unused

Avoid deeper BIOS changes until the machine is already stable.

## Branch 4: Fast Boot

Treat BIOS `Fast Boot` as optional and test-only.

Why:

- it may reduce firmware time on some units
- it may do nothing
- it may increase variance or interact poorly with USB initialization

Test it like this:

1. Change only `Fast Boot`
2. Keep the same keyboard cable and USB port
3. Compare at least two cold boots
4. Compare firmware time, not just subjective feel

If the result is not clearly better and stable, leave it off.

## Residual Noise

`PCIe / AER` or NVMe errors such as `BadTLP`, `BadDLLP`, `RxErr`, or `Timeout`
are separate from the confirmed `acpi_gpio_handle_deferred_request_irqs` boot stall.
