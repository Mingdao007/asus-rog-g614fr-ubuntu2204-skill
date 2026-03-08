# Audio Workflow

Use this reference for:

- Ubuntu 22.04
- Realtek `1043:1054`
- built-in speakers silent
- wired headphones still work

## Confirmed Symptom Pattern

The successful repair on the reference machine matched this pattern:

- built-in speakers had no sound on Ubuntu
- wired headphones still had sound
- hardware itself was not dead
- jack detection was working normally

That symptom pattern pointed away from generic PulseAudio routing problems and
toward a machine-specific codec or amp initialization issue.

## Root Cause Summary

For the reference machine, the missing piece was a Realtek HDA quirk for
subsystem:

- `1043:1054`

The working fix was a narrow override of:

- `snd-hda-codec-realtek.ko`

Do not broaden the fix casually to other models.

## Recommended Workflow

1. Confirm the symptom pattern before changing anything.
2. Keep Bluetooth troubleshooting separate unless explicitly requested.
3. Confirm the exact running kernel first.
4. If the machine is still on the known-good kernel family, check whether a
   machine-specific Realtek override is already installed.
5. If not, rebuild or install the override only for that exact kernel.
6. Reboot and test again.

## Rebuild Strategy

When rebuilding for another kernel:

1. Match the exact Ubuntu kernel source package and headers
2. Keep the override narrow
3. Replace only `snd-hda-codec-realtek.ko`
4. Install it under:
   - `/lib/modules/<kernel>/updates/...`

Do not assume a module built for one kernel remains valid for another.

## Playback Rule

Warn the user before any command that will emit audible sound.

## Scope Boundary

- Keep Bluetooth audio separate from the built-in speaker path
- Keep the fix machine-specific
