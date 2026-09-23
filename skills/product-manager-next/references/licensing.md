# Accounts and licensing

Use [runtime](runtime.md) if LMD is not initialized. Keep the intended account,
privilege scope and Cloud environment throughout the operation.

Choose the authorization workflow before changing state:

- Local online authorization: log in, then activate the account's entitlement.
- Local offline authorization: generate an ID for issuance, then import the supplied license.
- Remote authorization: configure an eligible server and connect the client's LMD to it.

Remote authorization uses the server's license; it does not require activating
the same license locally on every client.

## Login and online activation

```text
<product-manager-next> login --user <email>
<product-manager-next> status
<product-manager-next> activate online
<product-manager-next> status
```

Login and activation are separate operations. Check the account identity after
login, then activation source, role and license validity after activation.
A valid login alone does not establish an eligible entitlement. If the user needs
an account, use the product-supported registration flow.

Use the supported password prompt. If the agent cannot safely supply the secret,
let the user complete that step. Do not invent password flags, expose credentials
in commands or create accounts, organizations or orders outside the requested task.

## Offline ID and license import

```text
<product-manager-next> gen-id
```

This generates an opaque ID and saves a file; it does not issue or activate a
license. Use the reported output path and retain the file for the user's offline
license request. Do not publish the ID, overwrite a previous ID automatically,
or assume sudo uses the invoking user's Downloads directory.

Once the user has obtained the license file:

```text
<product-manager-next> activate import --file <absolute-license-file>
<product-manager-next> status
```

Use the supplied file without changing its extension just to match an example.
A .json or .tpt suffix alone does not establish a valid payload. Check caller
read access, then use the supported import entry point. If access fails, diagnose
the actual file handoff; do not make private files world-readable or edit LMD caches.

Check activation, source, role, validity and intended entitlement after import.
An active license can prevent another import. Treat that protection as a reason
to clarify replacement intent, not to force import. Deactivation before reimport
is a separate operation that changes license availability.

## Activation identity

When the license policy requires a supplied activation user key:

```text
<product-manager-next> set-user-key
<product-manager-next> status
```

Enter the key through its private prompt. An activation user key is not an email
address, password or Cloud login JWT. Importing a file and setting identity are
separate operations; do not assume --file also configures identity or invent an
import --key flag. Check the resulting identity as needed and redact it in shared
output.

## Remote authorization

### Configure the server

Inspect the server's service mode, activation role and license validity. Establish
an eligible Server license through online activation or offline import as required.
A saved port alone does not establish a usable remote license service.

```text
<product-manager-next> status
<product-manager-next> config lmd.server.port
<product-manager-next> config lmd.server.port <port>
<product-manager-next> status
```

Changing the port requires administrator privileges. Check the effective listener
and reachability from the intended client after configuration. On Windows, inspect
product-managed firewall rules and the active profile rather than assuming a saved
port is reachable. Prefer the product's supported firewall management; if a manual
rule is needed, limit it to the intended traffic. Do not disable the firewall.

### Connect a client

Inspect the current mode and target first. Switch to remote mode only when needed;
the switch reconfigures and restarts the native service.

```text
<product-manager-next> status
<product-manager-next> config lmd.client.address
<product-manager-next> service switch --mode remote --yes
<product-manager-next> remote test <server-IPv4>:<port>
<product-manager-next> remote connect <server-IPv4>:<port> --yes
<product-manager-next> config lmd.client.address
<product-manager-next> status
```

Check the installed CLI's accepted address syntax before using hostnames or IPv6.
`remote test` probes without saving a target; `remote connect` validates and saves
it. A successful probe alone does not reconfigure the client.

A client uses the remote server's authorization rather than requiring a separate
Cloud login and local activation. Respect the server's identity policy: a target
address and an activation user key are different inputs, and a key is needed only
when the policy requires it. Do not invent a configuration key to supply one;
use the supported interface for the selected mode.

Check remote details through status: target, license source, product and dates
should match the selected server. A persisted target may survive restart while
the remote version is temporarily unknown; inspect the address and connection
before declaring configuration lost. Product checkout is outside this skill.

## Deactivation and logout

For an explicitly requested deactivation:

```text
<product-manager-next> deactivate --yes
<product-manager-next> status
```

Expect inactive licensing after success; the account can remain logged in.
Logging out is a separate operation:

```text
<product-manager-next> logout --yes
<product-manager-next> status
```

Do not treat logout as deactivation, or perform either as automatic task cleanup.
For remote authorization, distinguish disconnecting/reconfiguring a client from
deactivating the server's license, which affects its clients. If an operation
times out, inspect state before retrying or moving authorization to another
machine. Preserve both the response and observed state while the outcome is unclear.
