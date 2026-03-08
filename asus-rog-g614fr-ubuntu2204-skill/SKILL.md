---
name: asus-rog-g614fr-ubuntu2204-skill
description: "Maintenance workflow for ASUS ROG Strix G16 G614FR on Ubuntu 22.04. Use when diagnosing or maintaining: (1) a 30-40 second blank screen before /init on kernel 6.8.0-85-generic, (2) boot changes or hangs when a ROCCAT Vulcan II Max (1e7d:2ee2) is attached, (3) built-in speakers silent while wired headphones still work on Realtek 1043:1054, or (4) GRUB and BIOS boot tuning on this model family."
---

# ASUS ROG G614FR Ubuntu 22.04 Skill

Validated baseline: Ubuntu `22.04` + kernel `6.8.0-85-generic` + NVIDIA driver `580.95.05`.

Read [`references/current-state.md`](references/current-state.md) when exact
known-good versions, kernel parameters, or confirmed findings matter.

## Quick Start

Pick one branch:

1. Ubuntu 22.04 + `6.8.0-85-generic`: `30-40s` blank screen before `/init`
   read [`references/boot-and-keyboard.md`](references/boot-and-keyboard.md)
2. `ROCCAT Vulcan II Max` (`1e7d:2ee2`): boot changes when attached
   read [`references/boot-and-keyboard.md`](references/boot-and-keyboard.md)
3. Realtek `1043:1054`: built-in speakers silent while wired headphones still work
   read [`references/audio.md`](references/audio.md)
4. Need the known-good baseline before making changes:
   read [`references/current-state.md`](references/current-state.md)

## Workflow

### 1. Boot And Keyboard

Use this branch for:

- `30-40s` blank-screen boot delays
- keyboard-connected boot hangs
- BIOS boot tuning
- GRUB tuning
- collecting comparable boot samples

Follow [`references/boot-and-keyboard.md`](references/boot-and-keyboard.md).

### 2. Audio

Use this branch for:

- built-in speaker regressions
- machine-specific Realtek quirk rebuilds
- rollback of local speaker overrides

Follow [`references/audio.md`](references/audio.md).

### 3. Baseline Verification

Before changing anything, confirm:

```bash
uname -r
cat /proc/cmdline
grep -E '^(GRUB_TIMEOUT_STYLE|GRUB_TIMEOUT|GRUB_CMDLINE_LINUX_DEFAULT)=' /etc/default/grub
systemd-analyze
```

If the running kernel differs from the known-good baseline, treat both boot and
audio work as new integration work. Do not assume the old conclusions still
transfer unchanged.

## Rules

- Keep fixes machine-specific to this model family.
- Warn explicitly before any command that will emit audible sound.
- Treat Bluetooth audio as separate from the built-in speaker path unless the
  user explicitly asks for Bluetooth.
- For boot issues, isolate pre-`/init` stalls before tuning normal userspace
  services.
- Once userspace boot is already around `3s`, firmware and BIOS behavior become
  the main bottleneck.
- Treat BIOS `Fast Boot` as a test item, not a default recommendation.
