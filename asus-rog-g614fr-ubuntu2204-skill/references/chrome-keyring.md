# Chrome And Keyring

Use this reference when:

- GDM autologin is enabled
- Google Chrome asks for a keyring password on first launch after boot

## Status

- known unresolved issue on the reference machine
- the public record for this branch is a failure case, not a recommended fix

Rejected workaround:

- add `--password-store=basic` to every `Exec=` line in:
  - `~/.local/share/applications/google-chrome.desktop`
  - `~/.local/share/applications/com.google.Chrome.desktop`
- outcome on the reference machine:
  - Chrome stopped using GNOME keyring
  - existing saved secrets were no longer available in the expected way
  - the workaround was rolled back

## Public Guidance

- keep the stock Chrome launcher
- if `--password-store=basic` was applied, remove the user-level overrides above
- do not present this workaround as a general fix for this model

## Retest Rule

- fully exit Chrome before retesting
- after rollback, retest from the stock system launcher

## Tradeoff

- current public state:
  - keyring prompt under autologin remains a known issue
  - the previously tested Chrome override is documented as rejected
