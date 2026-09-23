# Maintaining Terapines Production Skills

## Purpose and document ownership

This is a public collection of skills that help agents complete user tasks with
Terapines products. Every skill must be usable without private repository access
or knowledge of the maintainers' environment.

| Location | Audience and responsibility |
| --- | --- |
| README.md / README.zh-CN.md | Users: what the repository offers, available skills, installation and usage |
| INSTALL.md | Installing agents: obtain and install complete skill directories, handle updates and check installation |
| skills/<name>/SKILL.md | Operating agents: when to use the skill, task selection, shared conventions and reference links |
| skills/<name>/references/ | Operating agents: task-specific commands, prerequisites, expected behavior and troubleshooting |
| AGENTS.md | Contributors and editing agents: maintenance conventions for this repository only |

Publish instructions that affect how a user task should be performed. Keep
implementation details, development history, execution reports and project
planning outside the skill content. Do not add credentials, license payloads,
private endpoints, personal paths or machine inventories.

## Content conventions

- Maintain skill instructions in English and keep both README versions aligned.
- Use `Product Manager Next` in prose. Preserve exact executable names, CLI
  arguments and skill identifiers. Its setup reference is `pmn-setup.md`.
  Use `<product-manager-next>` as the executable placeholder. Distinguish its
  Terapines 5.x-and-later scope from the legacy Product Manager's 4.x scope;
  these are managed-product versions, not manager versions.
- Check command syntax and behavior against the relevant CLI help and official
  product documentation. Do not invent flags, output fields or error meanings.
- State a version/platform constraint where it changes the user's next action;
  avoid broad compatibility promises or unsupported guarantees.
- Keep shared instructions in SKILL.md and task-specific detail in references.
  Link each reference from its caller; do not duplicate command catalogs.
- Treat remote authorization as part of licensing. Product Manager Next guidance
  covers management operations, not running installed products such as compiling
  with ZCC.
- Describe an expected outcome and a practical way to check it. Troubleshooting
  should connect the observed symptom to the next useful check or corrective action.
- Respect user intent and existing authorization. Preserve existing installations
  and data; do not add routine permission questions or automatic teardown steps.

## Structure and maintenance

Each skill directory contains SKILL.md with a precise YAML name and description.
Add supporting scripts or assets only when a concrete user operation needs them.
Keep skills independent of a particular developer checkout or installation tool.

After edits, check Markdown links, skill frontmatter, command examples, translated
README consistency and installation instructions. Run any executable helpers you
change in an appropriate isolated environment. Remove obsolete references and
update their callers together; avoid parallel documents describing the same task.
