# Troubleshoot Product Manager Next

## Identify the failing operation

Record the user's intended action, executable path/version, operating system,
execution identity and sanitized command output. Keep the process exit code,
application error code and displayed message separate. Establish whether the
failure concerns the Installer, a Product Manager Next operation or LMD.

Start with the checks relevant to that operation:

```text
<product-manager-next> version --short
<product-manager-next> status
<product-manager-next> service status
```

If Product Manager Next is not installed or cannot start, use the Installer's
help/version output and [setup guidance](pmn-setup.md) instead. If a status query
fails, retain its error; do not treat missing output as an inactive license.
See [runtime](runtime.md) for initialization and [licensing](licensing.md) for
account, activation and remote-authorization commands.

## Choose the next check

| Symptom | Check first | Next action |
| --- | --- | --- |
| Executable exists, but the manager reports Not Installed | Executable path, current identity and user/administrator installation scope | Query in the intended scope; do not reinstall or delete records solely because another scope cannot see the installation |
| Installation or removal reports access denied | Target path, file ownership, running processes and intended installation scope | Resolve the specific path/process issue; do not switch to administrator installation implicitly |
| Command requires initialization | Whether LMD is registered and whether initialization is part of the requested task | Follow runtime initialization with the authorized administrator context; do not use init as a blanket repair |
| Windows service/status query is denied | Actual process token and service-manager access | Identify the denied query and use an authorized diagnostic context if needed; do not conclude the service is stopped or alter its ACL blindly |
| Service is Running but license is inactive/unavailable | Account and license sections separately | If activation is requested, select online, offline or remote authorization; an unactivated service need not be repaired |
| Login fails | Account/endpoint, exact authentication response and network reachability | Distinguish rejected credentials from network/TLS failures; do not repeatedly submit a password or disable certificate checks |
| Login succeeds but activation fails | Eligible entitlement, current activation state, license role and validity | Follow the specific business response; successful login is not proof of available authorization |
| Import is blocked while already active | Current activation and whether replacement was intended | Explain the protection; deactivate only if replacing authorization is requested |
| Import cannot read the file | Actual path, file existence, caller access and any service-side path error | Correct the supplied path or supported file handoff; do not expose the file broadly or copy it into private service storage by guesswork |
| Activation user key is rejected | Required identity policy and whether the supplied value is an activation key | Use the supported key prompt with the appropriate key; an email, password or login token is not a substitute |
| Remote probe succeeds but the target has not changed | Current target and whether test or connect was used | Apply remote connect only when a target change is intended; test alone does not save configuration |
| Remote port is saved but the client cannot connect | Eligible server license, effective listener, target address/port, firewall and network path | Fix the identified configuration or reachability issue; keep firewall changes limited to required traffic |

For detailed operation steps, follow the relevant reference rather than executing
all diagnosis branches. A proposed change should address the observed failure.

## Handle timeouts and unclear results

After activation, import, deactivation or connection configuration times out,
query the resulting state before repeating the mutation. Report both facts if
the command failed but state changed; do not claim either success or rollback
from the exit code alone. If the state cannot be determined, collect diagnostics
and leave the outcome explicit instead of entering a retry loop.

Check displayed validity periods and the host's actual time/timezone when the
message concerns validity. Overall authorization validity and an individual
product entitlement's dates can differ. Do not move the clock backward, disable
time synchronization or clear service data to make a license appear valid.

Use error meanings documented for the installed component/version. Do not map
an SDK product-checkout code onto a Product Manager Next activation response.
A generic internal error does not establish that an account is bound elsewhere.

## Collect logs when status is insufficient

Inspect the selected executable's log help, then export only the relevant logs:

```text
<product-manager-next> log export --help
<product-manager-next> log export pm
<product-manager-next> log export lmd
```

`pm` and `lmd` are literal CLI arguments. The first exports Product Manager Next
logs; the second exports service logs. Choose one or both according to the failure.
Export creates an archive in the executing user's home directory; use the reported
path, particularly under sudo or an elevated account. An export failure is a
separate diagnostic outcome, not evidence that the original operation succeeded.

Keep archives local unless the user requests sharing. Review and redact excerpts
before including them in a report; logs may contain account details, identifiers,
paths or authorization material. Do not publish passwords, offline IDs, activation
keys or license payloads. Do not assume export implies complete sanitization.

## Resolve or hand off

Summarize the failed operation, relevant version/platform/identity, observed state,
error message, checks performed and the next targeted action. Separate a confirmed
cause from a hypothesis. Do not automatically restart, repair, reinstall, deactivate
or remove data as a final cleanup step. Existing user authorization still applies
when a corrective action is clearly within the requested task.

If the issue is actual product execution, report the management state established
here and hand off to that product's guidance. This skill does not run compilers
or reproduce product checkout behavior.
