# Chrome And Keyring

Use this reference when:

- GDM autologin is enabled
- Google Chrome asks for a keyring password on first launch after boot

## Reference-Machine Result

The reliable convenience fix on the reference machine was not a GNOME keyring
password change. It was a per-user Chrome launcher override:

- add `--password-store=basic` to every `Exec=` line in:
  - `~/.local/share/applications/google-chrome.desktop`
  - `~/.local/share/applications/com.google.Chrome.desktop`

## Why This Branch Won

- it preserves GDM autologin
- it is per-user, not system-wide
- it stops Chrome from depending on GNOME keyring on this machine

## Retest Rule

- fully exit Chrome before retesting
- if the prompt persists, verify Chrome is launching from the user-level
  `.desktop` override, not the system desktop file

## Tradeoff

- Chrome uses its own local basic password store instead of GNOME keyring
