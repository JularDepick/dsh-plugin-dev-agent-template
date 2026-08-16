<div align="center">

# dsh-plugin-dev-agent-template

[![Version](https://img.shields.io/badge/Version-0.1.0-green)](https://github.com/JularDepick/dsh-plugin-dev-agent-template/tree/v0.1.0)
[![Copyright](https://img.shields.io/badge/Copyright-JularDepick-0066AA)](./COPYRIGHT)
[![License](https://img.shields.io/badge/License-MIT-yellow)](./LICENSE)

[English]
| [简体中文](./README.md)

</div>

Still writing your own dsh-plugin template?

Try this template and let your Agent do everything for you.

[Jump to Quick Start](#quick-start)


## What does this template do?

This repository is a general-purpose blank template (presets) for dsh plugin development, not bound to any specific plugin target.

Stance of this repository: it ships **no concrete plugin template code** (no runnable plugin implementation). Instead, it **sets the standards** for dsh plugin development — preset development assets plus an initialization prompt — and lets an **Agent drive the initialization and development of your plugin** (highly Agent-oriented development). This template is **Agent-driven at its core and is not intended for manual use by developers**.

It does two things: provide presets for a dsh plugin project, and use an initialization prompt to guide the Agent in transforming this repository in place into your concrete plugin project.

- `template/` presets (released to the repository root during initialization):
  - dsh official plugin development docs: `docs/dsh-dev-docs/<version>/` (start with the `index.agent.md` quick reference, then read the basics and framework chapters as needed)
  - Repository-level spec `docs/repo-spec/tag-release-spec.md` (version, tag and release naming) and technical spec `docs/tech-spec/translation-ini.md` (translation file spec)
  - Translation file presets `src/translation/` (only `xx-YY.ini` and the base template; no loader source)
  - README templates: `README_zh-CN.md`, `README_en-US.md` (placeholders plus language cross-links; replace them for the actual project and decide the final naming by the core language)
  - Blank `AGENTS.md` template and `version.index.md` version index template
- Root `init-prompt.md` (initialization prompt): guides the Agent to confirm design information in Q&A with you (plugin goal, naming `dsh-<core-name>-plugin`, target dsh version, core documentation language, etc.) and perform the in-place transformation (clear repository-bound content and `.git/`, release `template/`, `git init`, confirm the built-in blank `AGENTS.md`, scaffold the project, adapt README and `AGENTS.md`). This file is one-time only and is removed after initialization.

## Why do you need this template?

- dsh plugin development comes with a whole set of conventions: plugin essentials (`name`/`inject`/`apply`), config schemas, tool registration, lifecycle and service dependencies, event patterns, bundle packaging and layer rules, plus repository-level specs for version, tag, release and translation files. Building from scratch is error-prone and hard to keep consistent across projects.
- This template bakes those conventions into reusable presets and writes the initialization process into a prompt: the Agent confirms design information in Q&A and transforms in place, producing a project skeleton and documentation system that follow the conventions; without your confirmation it installs no dependencies, runs no build and runs no tests.
- New projects inherit the `AGENTS.md` rule system (from the AGENTS.md-Best-Practices best practices). During initialization they use the built-in blank template directly and fill in the project-bound sections; later sessions only need to read `AGENTS.md` to continue development.
- dsh ships frequent updates and the official documentation explicitly warns of breaking changes: because this template is Agent-driven, when the official docs are updated you only need to pull the latest official plugin development docs (`docs/dsh-dev-docs/<new version>/`), and the Agent can iterate and adapt the project accordingly — no need to manually diff the docs item by item.

## Quick start

1. Clone this repository into a new directory (no need to hand-pick files), e.g. `git clone https://github.com/JularDepick/dsh-plugin-dev-agent-template dsh-<core-name>-plugin`
2. Open a new Agent session pointed at that directory and have the session read the root `init-prompt.md` fully first.
3. The Agent performs initialization according to the prompt (deletion and git operations are confirmed with you first): clear repository-bound content (including `.git/`), release `template/`, `git init`, confirm the built-in blank `AGENTS.md`, and after confirming core information in Q&A, scaffold `src/`, fill in packaging config, and adapt README and `AGENTS.md`.
4. After initialization, as prompted: rename the working directory to the actual plugin repository name (e.g. `dsh-github-plugin`), re-point the workspace, and open a new session reading `AGENTS.md` to continue development.

> Note: initialization is a one-time in-place transformation; it removes this repository's git history and `init-prompt.md` itself. If you need to keep the original template for reuse, clone a separate copy.

## License

This repository is licensed under the **MIT License**. See [LICENSE](./LICENSE).

The preset content built into this repository is licensed under its source repositories' licenses and is not bound by this repository's license; see Related links.

## Copyright

Copyright &copy; 2026 JularDepick

See [COPYRIGHT](./COPYRIGHT).

## Related links

- dsh official repository: https://github.com/deepseek-ai/deepseek-harness
- dsh official plugin development docs: https://github.com/deepseek-ai/deepseek-harness/tree/main/docs/user/develop
- AGENTS.md best practices built into this repository: https://github.com/JularDepick/AGENTS.md-Best-Practices/tree/main/src/develop/general-methodology/JularDepick/AGENTS.md

## Friendly links