---
name: product-manager-next
description: Install and operate Terapines Product Manager Next through its official Installer and CLI for Terapines 5.x and later products. Use for product installation/removal, account login/logout, online activation, offline license import, and LMD service or remote-connection management. Excludes legacy Product Manager and running installed products.
---

# Product Manager Next

Determine whether the user needs to install Product Manager Next itself or use an
existing installation. Choose the reference for that task; do not execute all workflows
as a single setup sequence.

## Scope and command conventions

Product Manager Next (`product-manager-next`) manages Terapines 5.x and later
products. The legacy Product Manager (`product-manager`) only installs Terapines
4.x products and is outside this skill. Confirm the requested product generation
before choosing the executable; do not apply these commands to the legacy manager.
These ranges refer to managed products, not the manager's own version number.

This skill covers product management and online/offline license administration. It does
not run installed products: ZCC compilation and product checkout belong to
product-specific guidance. For account registration, use the product-supported
registration flow; do not invent a register command.

Examples use `<product-manager-next>` for the selected ProductManagerNext executable (or
ProductManagerNext.exe on Windows), and `<installer>` for the selected Installer.
Substitute and quote actual paths; these angle-bracket placeholders are not literal
shell syntax. In PowerShell use `&` to invoke a quoted path. Apply the chosen privilege
context consistently, not an automatic sudo prefix.

## Establish context

Identify the intended outcome, OS, actual token/UID, executable path, version and
existing installation scope. A Windows administrator-group member may still have an
unelevated token. If setup is requested and the executable is absent, use the official
[setup workflow](references/pmn-setup.md).

Inspect `--help` and relevant subcommand help before mutations. Confirm the available
options on the installed version. Runtime queries may require initialized LMD; a
prerequisite error does not itself authorize initialization or repair.

## Choose the workflow

- Install or update Product Manager Next itself: [setup](references/pmn-setup.md).
- Initialize LMD or inspect service/license status: [runtime](references/runtime.md).
- Install or remove a managed product: [installation](references/installation.md).
- Login/logout, online activation, offline import, activation identity or remote authorization: [licensing](references/licensing.md).
- Investigate a failed management operation: [troubleshooting](references/troubleshooting.md).

## Execute and verify

Preserve the selected Cloud environment, installed products and user data. Use supported
noninteractive options only within the user's authorization. Uninstall, deactivate,
logout, service removal and firewall cleanup are separate operations; do not perform
them as automatic cleanup after a successful task.

Prefer structured output only if the actual CLI offers it; do not assume a JSON flag
exists. Keep process exit status, business response and observed state separate. Never
reinterpret a generic failure as success. Redact credentials, offline IDs, activation
keys and license payloads from shared reports. After an uncertain mutation result,
inspect state before deciding whether another attempt is appropriate; do not reset
caches or retry activation in a loop.
