<div align="center">

# dsh-plugin-dev-agent-template

[![Version](https://img.shields.io/badge/Version-0.1.0-green)](https://github.com/JularDepick/dsh-plugin-dev-agent-template/tree/v0.1.0)
[![Copyright](https://img.shields.io/badge/Copyright-JularDepick-0066AA)](./COPYRIGHT)
[![License](https://img.shields.io/badge/License-MIT-yellow)](./LICENSE)

[English]
| [简体中文](./README.md)

</div>

Still building your own dsh-plugin template?

Just clone this repository and let an Agent do the initialization and development for you, guided by one prompt.

[Jump to Quick Start](#quick-start)


## What this template does for you

This repository is a general-purpose dsh plugin development template: it ships the specs, translation presets, README templates and a blank AGENTS.md, plus an initialization prompt.

You only need to talk to an Agent and confirm requirements and key decisions (plugin goal, naming, target dsh version, documentation language, etc.); the Agent transforms the repository in place into your plugin project skeleton — source, config and documentation in one go. Later, every new session reads AGENTS.md and picks up development right away.

## Why do you need it?

- dsh plugin development has many conventions; starting from scratch is error-prone;
- The conventions are baked into presets and a prompt, so the Agent produces a compliant project based on your decisions;
- The official docs update frequently and warn of breaking changes; after updating the official docs the Agent iterates and adapts;
- No manual bookkeeping of development state: the AGENTS.md rule system lets every new session take over.

## Quick start

1. Clone this repository: `git clone https://github.com/JularDepick/dsh-plugin-dev-agent-template <plugin-repo-name>`
2. Open a new Agent session pointed at the directory and tell the Agent to read `init-prompt.md` first;
3. Follow the Agent's guidance to complete the remaining steps.

> Note: initialization removes this repository's git history and `init-prompt.md`; clone a separate copy to keep the original.

## License

**MIT License** — see [LICENSE](./LICENSE). Built-in preset content follows its source repositories' licenses.

## Copyright

Copyright &copy; 2026 JularDepick — see [COPYRIGHT](./COPYRIGHT).

## Related links

- dsh official repository: https://github.com/deepseek-ai/deepseek-harness
- dsh official plugin development docs: https://github.com/deepseek-ai/deepseek-harness/tree/main/docs/user/develop
- AGENTS.md best practices: https://github.com/JularDepick/AGENTS.md-Best-Practices/tree/main/src/develop/general-methodology/JularDepick/AGENTS.md


## Friendly links