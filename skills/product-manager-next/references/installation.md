# Managed product installation

For Product Manager Next itself, use [setup](pmn-setup.md). This reference covers
management of installed products, not running those products.

## Install or remove through Product Manager Next

Confirm the requested product/version, platform, destination and execution scope.
Use Product Manager Next for Terapines 5.x and later products. A request for a
Terapines 4.x product belongs to the legacy Product Manager; do not substitute
these commands for its workflow.
Inspect available versions and the current installation before a mutation:

```text
<product-manager-next> product list
<product-manager-next> product show <product>:<version>
<product-manager-next> product install <product>:<version>
<product-manager-next> product uninstall <product>:<version>
```

Install and uninstall are alternatives selected by user intent, not a sequence.
Check the chosen subcommand's help for path and confirmation options. If a command
requires initialized LMD, use [runtime](runtime.md) to resolve the prerequisite
within the requested task's scope.

## Use a dedicated product Installer

A dedicated Installer bundles a particular product/version and has a different
command surface. Do not interchange its `install` with the manager's
`product install`. Inspect the Installer's help and embedded product information:

```text
<product-installer> product show
<product-installer> install --help
```

Where supported, this form selects explicit product and manager directories:

```text
<product-installer> install --install-path <product-directory> --pm-install-path <manager-directory> --skip-pm-update --yes
```

Use absolute directories. To preserve an existing Product Manager Next, confirm
that it is registered in the same execution scope and select its directory.
`--skip-pm-update` skips updating an existing manager; it does not suppress a
bundled manager's initial installation when none is recorded. Check the bundled
manager's identity and Cloud environment before accepting its installation.
Omit the skip option when the user intends to accept that update.

Choose PATH integration according to user intent. Add `--no-env` only when PATH
should remain unchanged and the Installer supports it. For removal, inspect
`uninstall --help` and confirm the exact bundled product/version before using
`uninstall --yes`. Do not remove Product Manager Next or other products implicitly.

## Check the result

Use the selected manager or Installer's product query to check identity, version
and path. Their queries have different runtime prerequisites; use the Installer's
own query when managing its bundled product without an initialized LMD.
Check installation-owned files and requested PATH integration. After removal,
check the selected product's registration and files while preserving other
products and retained user data. Do not compile with ZCC or perform product
checkout as part of this management workflow.
