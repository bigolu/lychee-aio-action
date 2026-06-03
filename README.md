# lychee-aio-action

An all-in-one action for checking links with lychee and opening an issue if any are broken.

## Features

- All in one: You only need this action in your workflow.

- Issue creation: If there are broken links, an issue is opened. If a future run doesn't find any broken links, the issue will be closed.

- Easy local debugging: You can run the exact script that gets run in CI against your local repo to debug issues.

## Differences from [lychee-action][lychee-action]

- Closes an open issue if a future doesn't find any broken links

- This action doesn't take arguments to pass to lychee. Instead, you should specify everything in the `lychee.toml` config file. This way, the exact same settings are used in CI and locally when debugging.

- By default, this action runs on all files tracked by git. You can exclude files in `lychee.toml` using the `exclude_path` setting.

## Usage

Example workflow

```yaml
name: Check links
on:
  workflow_dispatch:
  schedule:
    # Run once a month.
    - cron: "43 0 1 * *"
permissions:
  # This is needed by the `cache-nix-gc-roots-action` which is used internally.
  actions: "write"
  # This is needed by the `gh` CLI to open/close issues.
  issues: "write"
jobs:
  check-links:
    # Linux or macOS is fine
    runs-on: ubuntu-24.04
    steps:
      - uses: bigolu/lychee-aio-action@main
```

## Debugging (requires [nix][nix])

To run it locally, run this command:

```bash
nix run github:bigolu/lychee-aio-action
```

You can also pass arguments that will be appended to the lychee command:

```bash
# See what files it would run on
nix run github:bigolu/lychee-aio-action -- --dump-inputs
```

It will only open/close issues if the `CI` environment variable is set to `true`.

[lychee-action]: https://github.com/lycheeverse/lychee-action
[nix]: https://nixos.org/
