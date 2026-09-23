# Install Terapines skills for your user

Use this guide when the user requests skill installation or an update. Follow
their target agent, destination and revision. Installing a skill copies agent
instructions; it does not install or run Terapines products or require a product
account.

## Select the source and skill

- Repository: https://github.com/Terapines/terapines-production-skills
- Clone URL: https://github.com/Terapines/terapines-production-skills.git
- Default revision: `main`, unless the user specifies a branch, tag or commit.

| Skill | Repository directory |
| --- | --- |
| product-manager-next | skills/product-manager-next |

Download the complete skill directory, including its references and any other
bundled resources, from the same revision. Reading a web page alone does not
install the skill. If you cannot write to the user's machine, provide the manual
instructions in [README.md](README.md) or [README.zh-CN.md](README.zh-CN.md).

## Choose the installation method

Prefer the target agent's supported skill installer. For Codex, if skill-installer
is available, follow its local instructions with these inputs:

```text
repository: Terapines/terapines-production-skills
path: skills/product-manager-next
ref: main
```

Substitute the user's requested revision when provided. Codex's bundled installer
uses `$CODEX_HOME/skills`, defaulting to `~/.codex/skills`; on Windows the default
is `%USERPROFILE%\.codex\skills`. Other agents may use different discovery paths:
consult that agent's instructions rather than assuming the Codex location.
Ask for the intended agent or scope only if context does not establish it.

## Install from a checkout when needed

1. Download or clone the selected repository revision into a fresh temporary
   location, or use a checkout supplied by the user. Preserve existing checkout
   changes; do not reset or clean their repository.
2. Read the selected SKILL.md and ensure all its bundled references are present.
3. Check the destination. If it already matches the selected source, report that
   the skill is installed. For an update, inspect and preserve local modifications
   according to the user's request. Resolve conflicting changes before replacing
   them, and avoid merging directories in a way that retains obsolete files.
4. Copy the complete skill directory into the supported skills location. Stage
   and check the copy before replacing an existing installation; do not overwrite
   an installation that appeared concurrently. Remove only your own temporary files.

Install the skill directory, not the repository root. README files, this guide
and the repository's AGENTS.md are not part of the installed skill. Do not copy
maintenance instructions into the user's project or global agent configuration.

## Check and report the installation

Confirm that `<skills-directory>/product-manager-next/SKILL.md` exists, its
frontmatter name is `product-manager-next`, and all bundled reference links resolve.
Compare the installed files with the selected source, including the complete
references directory; report missing files rather than claiming partial success.

Tell the user the installed skill, location, source revision and any handling of
an existing copy. Distinguish successful file installation from the agent actually
discovering the skill. In Codex, start a new conversation and invoke:

```text
$product-manager-next Inspect my Product Manager Next version and current license status.
```

If discovery has not refreshed, restart the agent and inspect its skill settings.
Provide the example to the user; do not execute product operations as part of
checking skill installation.
