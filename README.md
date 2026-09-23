# 🌲 Terapines Production Skills

English | [简体中文](README.zh-CN.md)

Skills that help AI agents use Terapines products through their command-line interfaces.
Describe your goal in natural language; the agent uses the relevant skill to inspect the
environment, choose product commands and check results.

This repository distributes agent instructions. Product installers and downloads are
obtained separately through the official product channels described by each skill.

## ✨ Available skills

| Skill | Purpose |
| --- | --- |
| [product-manager-next](skills/product-manager-next/SKILL.md) | Guidance for Product Manager Next setup/update, environment inspection, product installation/removal, account login/logout, online activation, offline license import, remote LMD configuration and Product Manager Next operation diagnosis. |

Product Manager Next (`product-manager-next`) manages Terapines 5.x and later
products. The legacy Product Manager (`product-manager`) only installs Terapines
4.x products and is not covered by this skill. These ranges describe managed
products, not the manager's own version number.

The Product Manager Next skill discovers exact commands from the installed CLI help.
Available operations depend on your installed product version and permissions.

## 📦 Install

### 🤖 Ask your agent to install

Send this request to your agent:

```text
Read https://raw.githubusercontent.com/Terapines/terapines-production-skills/main/INSTALL.md
and install the product-manager-next skill for me.
```

The [agent installation guide](INSTALL.md) describes the source, destination,
installation steps and verification. An agent with local file access can perform the
installation; a browser-only agent can provide instructions for you to run. You do not
need to clone the repository yourself for this route.

### 🛠️ Codex: install manually

First clone this repository, or download and extract it from GitHub:

```bash
git clone https://github.com/Terapines/terapines-production-skills.git
cd terapines-production-skills
```

Codex's bundled skill installer uses `$CODEX_HOME/skills`, defaulting to
`~/.codex/skills`. From the repository root on Linux:

```bash
skill_dir="${CODEX_HOME:-$HOME/.codex}/skills"
mkdir -p "$skill_dir"
if [ -e "$skill_dir/product-manager-next" ] || [ -L "$skill_dir/product-manager-next" ]; then
  echo "Skill already exists; review or back it up before replacing it."
else
  cp -R skills/product-manager-next "$skill_dir/"
fi
```

On Windows, copy `skills/product-manager-next` into the `skills` directory under
`CODEX_HOME`, or `%USERPROFILE%\.codex\skills` if CODEX_HOME is unset. The installed
entry point should be `skills/product-manager-next/SKILL.md`.

After installation, start a new conversation and invoke `$product-manager-next`. If the
skill is not listed, restart your agent and check its skill discovery settings.

### 🔌 Other agents

For agents that support SKILL.md, follow that agent's skill installation instructions
and copy the complete `skills/product-manager-next` directory into its supported
location. Discovery paths and invocation syntax vary by agent.

## 🚀 Use

Installing the skill does not automatically install Product Manager Next, ZCC or LMD. If
Product Manager Next is absent, ask the agent to set it up using the official Installer.
For an existing installation, provide its executable path when it is not discoverable.

Example requests in Codex:

```text
$product-manager-next Help me install Product Manager Next from the official website.

$product-manager-next Inspect my Product Manager Next version and current license status.

$product-manager-next Help me install ZCC. First check the existing installation
and the options supported by this Product Manager Next version.

$product-manager-next Diagnose why Product Manager Next reports an online activation failure.
```

State the target machine, product/version and intended outcome. The agent can inspect
status before changes and identify when an operation needs elevated privileges. Preserve
credentials and license files outside this repository.

## 🔄 Update or remove

To update, obtain the desired repository revision and replace the installed skill
directory after reviewing or backing up local changes. To uninstall, remove only that
installed skill directory; this does not uninstall Terapines products or change license
state.
