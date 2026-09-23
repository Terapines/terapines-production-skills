# Runtime initialization and status

## Inspect before changing the service

```text
<product-manager-next> version --short
<product-manager-next> status
<product-manager-next> service status
```

Read version, account, service and activation state as separate facts. Identify
fields within their sections rather than interpreting any Status line as the
overall result. Distinguish an absent or inaccessible service from a running
service with an inactive license.

Some commands require an initialized runtime. Explain that prerequisite before
proceeding; do not initialize a machine merely to answer a read-only question.
On Windows, a service-manager permission error does not establish that LMD is
stopped or a license is missing. Check the caller's token and permitted query
scope rather than weakening service ACLs.

## Initialize when requested

Installing Product Manager Next and initializing LMD are separate operations:

```text
<product-manager-next> init
<product-manager-next> status
```

Initialization installs required components and registers/starts the native
service. It requires Linux root/sudo or an elevated Windows token. An ordinary
user installation of Product Manager Next does not grant service-administration
rights. Use the authorized administrator context for initialization.

Check the operation response and service readiness. A fresh local service can
be Running with inactive activation and unavailable license: this is a healthy
unactivated runtime. Choose the next authorization step from [licensing](licensing.md)
only if the user requested it.

Do not use repeated init as a generic repair procedure. Inspect service status,
repair/service help and the actual failure before proposing repair or restart.
Product Manager Next updates and LMD component upgrades are separate tasks.
