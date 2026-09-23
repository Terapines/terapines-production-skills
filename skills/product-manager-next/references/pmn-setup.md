# Install or update Product Manager Next

Use this workflow when Product Manager Next itself is absent or the user requests an
update. Installing the skill alone does not authorize installing products. An explicit
request to set up Product Manager Next authorizes the corresponding installation; do not
ask again unless the destination, privilege scope or replacement decision is unclear.

## Official distribution

- Product page: https://www.terapines.com/products/product-manager-next
- Installer metadata: https://www.terapines.com/api/products/ProductManagerNext_Installer

Fetch metadata at execution time. Select from `versions[]` using `platform` and the
requested `version`; do not assume array order means newest. If no version was
requested, select the newest stable compatible version using semantic version ordering.
Do not substitute a different version silently when one was requested.

Each version entry supplies `version`, `platform`, `marchs`, `tarballName`,
`tarballSize`, `tarballMD5` and `released_at`. The top-level platform is not the
per-download platform. Empty `marchs` does not establish architecture compatibility.
Check the host architecture and the downloaded executable or published requirements
before attempting execution. Unsupported or ambiguous platforms require clarification,
not a guessed download.

Download the selected Installer from:

```text
https://www.terapines.com/api/products/ProductManagerNext_Installer/versions/{version}/archives/{tarballName}
```

URL-encode each substituted path segment independently. Use values from the selected
metadata entry, not a guessed filename or extension. Save the download to a new owned
directory; require successful HTTP transfer and verify the exact byte size and
case-insensitive MD5 before running it. MD5 checks consistency with the catalog; it is
not a cryptographic authenticity signature. Keep HTTPS verification enabled. After a
partial transfer or resume, verify the complete file again. Stop on mismatch; do not
execute or silently accept it.

The Linux artifact is an executable despite the API's `tarballName` field; do not try to
unpack it based on that field name. On Linux, add execution permission with `chmod a+x`
to the downloaded file after validation. Windows uses an `.exe`.

## Inspect before installation

Identify the intended user/system scope and existing Product Manager Next executable
path/version. Run the downloaded installer's help and inspect its embedded product
before any installation. These commands can create installer configuration or logs, even
though they do not install Product Manager Next:

```text
<installer> --help
<installer> version
<installer> product show
<installer> install --help
```

The installer's version identifies the installer; `product show` reports the embedded
ProductManagerNext version and the installation it detects. Do not treat those versions
as interchangeable. Discovery is identity-sensitive: use the intended account and
privilege scope and check a user-supplied existing path even if the installer says Not
Installed. Windows group membership alone does not prove an elevated token. Do not use
sudo merely to inspect a user installation.

## Choose the installer launch identity

Choose user or administrator installation before launching the Installer, based on the
user's intended scope. Do not retry a failed user installation with sudo or elevation
without resolving that change of scope. A custom install directory and the effective
execution identity are separate choices.

Default locations (without redirected Known Folders or XDG overrides):

| Platform / launch | Default executable | Installation record |
| --- | --- | --- |
| Ubuntu ordinary user | `$HOME/Terapines/ProductManagerNext` | `$HOME/.local/share/terapines/product-manager-next/installations.json` |
| Ubuntu sudo / root login | `/opt/Terapines/ProductManagerNext` | `/var/lib/terapines/product-manager-next/installations.json` |
| Windows standard user | `%USERPROFILE%\Terapines\ProductManagerNext.exe` | `%LOCALAPPDATA%\Terapines\product-manager-next\data\installations.json` |
| Windows elevated administrator | `%ProgramFiles%\Terapines\ProductManagerNext.exe` | `%ProgramData%\Terapines\product-manager-next\data\installations.json` |

Resolve actual Windows Known Folders and Linux XDG settings on the target host; these
defaults are not hard-coded universal locations. Run product show, install and later
management commands in the intended identity/scope. Executable location alone does not
choose the Product Manager Next state namespace. A Not Installed result in one scope
does not prove another scope has no installation; do not delete records or move private
directories to combine the scopes.

Linux ordinary-user installation uses user XDG configuration/data/state/cache.
Administrator installation uses `/etc/terapines/product-manager-next`,
`/var/lib/terapines/product-manager-next`, `/var/log/terapines/product-manager-next` and
`/var/cache/terapines/product-manager-next`. Private directories use `0700`,
installation records `0600` and the executable `0755`, owned by the effective user. Do
not loosen those permissions to share account state.

Linux user shortcuts target the user's Desktop and application menu. Root installation
creates the system application shortcut and a desktop shortcut for the root execution
context; it does not promise a shortcut on the invoking user's desktop. Desktop
locations can follow XDG user-directory configuration.

Windows standard-user state is under LocalAppData, with user Desktop/Start Menu
shortcuts. Elevated state is under ProgramData, with Public Desktop/common Start Menu
shortcuts. Private state has a protected DACL using OWNER RIGHTS plus SYSTEM for a
standard user, or Administrators plus SYSTEM for elevated execution. Default
installation ACLs are inherited from the selected install location and differ from
private state.

### Native command forms

After verifying the downloaded artifact, an ordinary Linux user can use:

```bash
"$installer" product show
"$installer" install --yes
```

For an explicitly selected administrator installation, use the same elevated context for
inspection, installation and verification:

```bash
sudo -- "$installer" product show
sudo -- "$installer" install --yes
```

Here `$installer` is the absolute downloaded executable path. A root shell can invoke
the executable directly. Root and sudo use system Product Manager Next storage, but
their environment and desktop context should be inspected separately.

On Windows, use the same commands in the selected standard or elevated PowerShell:

```powershell
& $installer product show
& $installer install --yes
```

Confirm the actual token. Membership in Administrators is not proof of elevation. Do not
assume an SSH session has the same token as an interactive desktop session. If elevation
is needed, arrange an elevated execution channel with the user and complete the
operating system's normal elevation flow.

## Install

To select an explicit installation directory:

```text
<installer> install --install-path <absolute-install-directory> --yes
```

`--install-path` is the directory containing ProductManagerNext, not the path of the
executable itself. On Windows, invoke a quoted executable using PowerShell's call
operator, for example `& $installer install --install-path $pmDirectory --yes`. On
Linux, invoke the quoted executable path directly. Discover the default path from this
artifact's help if the user wants the default. Use `--yes` only for an installation
already authorized by the user. Apply elevation only when required for the agreed
installation scope.

Do not run Product Manager Next with no arguments just to check the result; that may
launch its GUI. Re-run the installer's `product show`, confirm the selected version and
actual installation path, and inspect the installed Product Manager Next's `--help`. Use
the version command advertised by the installed executable to confirm its version. A
zero exit status alone is insufficient: cancellation or an already-installed result may
perform no work. Do not initialize LMD, log in, activate a license or run an installed
compiler as an implicit completion check for Product Manager Next installation.

## Update an existing Product Manager Next

The Installer describes `install` as installing or updating Product Manager Next. Before
using it for an update, inspect the existing installation, intended target version/path
and matching release guidance. Preserve the existing account and scope; do not create a
second user installation by switching identities or accept a downgrade as an update. A
same-version result does not prove that a different build replaced the old bytes. Do not
delete installation records to force replacement.

Check the target release's upgrade and state-retention guidance before replacing an
existing installation. Preserve user configuration and product records; do not assume a
rollback mechanism exists based only on the command name. Verify the resulting Product
Manager Next version/path and relevant existing management state after an update.
Product Manager Next replacement and LMD service upgrade are separate operations; do not
perform a service upgrade merely because the new Product Manager Next carries an LMD
package.
