# ESP32 Embedded Engineer Skill (Codex)

This repository contains an ESP32/ESP-IDF-focused Codex skill.

It is a public, human-facing repo that explains what the skill is and what it does. The actual agent instructions live in `SKILL.md`.

## What This Is

- A Codex skill for ESP32 firmware engineering (ESP-IDF)
- Focused on correctness, debugging, bring-up, review quality, and reproducible workflows
- Encodes strict rules for hardware context, exact ESP32 variant identification, partitioning, `sdkconfig`, and build validation

## Key Behaviors the Skill Enforces

- Do not proceed on hardware-facing tasks without clear hardware context
- Do not proceed without exact ESP32 variant
- Require concrete framework compatibility evidence (ESP-IDF / ESP-ADF / ESP-SR, etc.) before build/debug
- Prefer reproducible config changes (`sdkconfig` / partition CSV) over ad hoc menu navigation
- Run build wrappers and validate before claiming completion
- Add a service terminal proactively when USB/serial is free and policy allows it
- Ask for example code / minimal repros instead of guessing when details are unclear

## Repo Structure

- `SKILL.md`: machine-facing skill instructions and routing
- `agents/openai.yaml`: UI metadata for the skill
- `references/`: topic-specific guidance (RTOS, memory, power, partitions, logging, display, compatibility, etc.)
- `scripts/`: reusable wrapper scripts and compatibility preflight tooling
- `assets/templates/`: reusable templates (console, component skeletons, partitions, shell snippets, compatibility lock example)
- `examples/`: reference material used during development of the skill

## Generated / Local Files

- `build/plugin-compatibility-evidence.txt` is a generated preflight report (safe to delete; regenerated as needed)
- `validate.sh` is a local helper that runs the `skill-creator` validator in a Python virtualenv

## Validation

Typical local checks used for this repo:

```bash
bash ./validate.sh .
bash -n scripts/*.sh
python3 -m py_compile scripts/check_plugin_compatibility.py
git diff --check
