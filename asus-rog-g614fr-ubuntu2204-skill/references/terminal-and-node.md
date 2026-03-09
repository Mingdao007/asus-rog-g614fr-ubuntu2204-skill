# Terminal And Node

Use this reference when:

- opening a terminal prints the `nvm` warning about user `.npmrc`,
  `prefix`, or `globalconfig`

## Reference-Machine Result

The warning on the reference machine was resolved by clearing the conflicting
user npm config.

Working state:

- `~/.npmrc` absent
- `~/.nvm/.npmrc` contains only `package-lock=false`

Reliable fix used on the reference machine:

```bash
nvm use --delete-prefix v24.13.0 --silent
```

## If It Reappears

Check these first:

- `~/.npmrc`
- `PREFIX`
- `NPM_CONFIG_PREFIX`
- `NPM_CONFIG_GLOBALCONFIG`

Do not start by patching `nvm` or shell startup files.
