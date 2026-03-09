# ASUS ROG G614FR Ubuntu 22.04 Skill

Model: `ASUS ROG Strix G16 G614FR`

Validated baseline:

- Ubuntu `22.04`
- kernel `6.8.0-85-generic`
- NVIDIA driver `580.95.05`

Problems:

- Ubuntu 22.04 + `6.8.0-85-generic`: `30-40s` blank screen before `/init`
- `ROCCAT Vulcan II Max` (`1e7d:2ee2`): boot failure when attached at power-on
- Realtek `1043:1054`: built-in speakers silent while wired headphones still work
- `nvm` warning on terminal startup from conflicting npm prefix or `.npmrc`
- Known unresolved issue: GDM autologin + Google Chrome keyring password prompt on first launch after boot

Skill:

- [`asus-rog-g614fr-ubuntu2204-skill`](./asus-rog-g614fr-ubuntu2204-skill)

Chinese:

- [README.zh-CN.md](README.zh-CN.md)
