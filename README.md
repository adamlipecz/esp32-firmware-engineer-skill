# esp32-firmware-engineer

An agent skill for ESP32/ESP-IDF firmware engineering. Works with any agent that supports the [skills.sh](https://skills.sh) format — Claude Code, Codex, and others.

## Install

```bash
npx skills add adamlipecz/esp32-firmware-engineer-skill
```

## What This Is

- A skill for ESP32 firmware engineering (ESP-IDF)
- Focused on correctness, debugging, bring-up, review quality, and reproducible workflows
- Encodes strict rules for hardware context, exact ESP32 variant identification, OTA, security hardening, LVGL integration, partitioning, `sdkconfig`, and build validation

## Key Behaviors the Skill Enforces

- Do not proceed on hardware-facing tasks without clear hardware context
- Do not proceed without exact ESP32 variant
- Require concrete framework compatibility evidence (ESP-IDF / ESP-ADF / ESP-SR, LVGL, etc.) before build/debug
- Prefer reproducible config changes (`sdkconfig` / partition CSV) over ad hoc menu navigation
- Run build wrappers and validate before claiming completion
- Add a service terminal proactively when USB/serial is free and policy allows it
- Ask for example code / minimal repros instead of guessing when details are unclear

## Repo Structure

- `SKILL.md` — agent-facing skill instructions and routing
- `agents/openai.yaml` — UI metadata for OpenAI-compatible agents
- `agents/claude.yaml` — UI metadata for Claude Code
- `references/` — topic-specific guidance:
  - RTOS patterns, communication protocols, memory optimization, power optimization
  - Microcontroller programming, partitions & sdkconfig, logging & observability
  - Display & graphics, LVGL integration, device terminal console
  - OTA workflow, security hardening
  - Dependency compatibility, toolchain & shell setup, panic log triage
  - ESP-IDF checklists, engineering values
- `scripts/` — reusable wrapper scripts (`build.sh`, `flash.sh`, `monitor.sh`, etc.) and compatibility preflight tooling
- `assets/templates/` — reusable templates (console, component skeletons, partitions, shell snippets, compatibility lock example)

## Validation

```bash
bash -n scripts/*.sh
python3 -m py_compile scripts/check_plugin_compatibility.py
git diff --check
```
