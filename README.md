English | [繁體中文](README.zh-TW.md)

# Codex Pilot

**Better defaults for how Codex works.**

`codex-pilot` is a reusable global `AGENTS.md` for people who want Codex to carry ordinary authorized work forward without pausing to confirm model or effort settings before every large task.

It changes the instructions Codex follows. It does not change the model picker, grant permissions, override higher-priority rules, or install a runtime. There is no package or daemon.

Large tasks can stall when Codex asks about model or effort before touching the work. This file tells it to start with the setting already chosen, carry authorized work through each phase, and save questions for decisions that need the user's authority.

## Install

By default, Codex reads user-level instructions from `~/.codex/AGENTS.md`. On Windows, the equivalent location is `%USERPROFILE%\.codex\AGENTS.md`. If you set `CODEX_HOME`, use `AGENTS.md` in that directory instead. See [OpenAI's Codex guidance for AGENTS.md](https://developers.openai.com/codex/guides/agents-md).

Back up or merge an existing global `AGENTS.md`; do not overwrite your personal rules. If the file does not exist, copy this repository's `AGENTS.md` to the global location.

macOS or Linux, from the repository directory:

```sh
codex_home="${CODEX_HOME:-$HOME/.codex}"
mkdir -p "$codex_home"
if [ -e "$codex_home/AGENTS.md" ]; then
  printf '%s\n' 'Merge AGENTS.md with the existing global file; no file was overwritten.'
else
  cp AGENTS.md "$codex_home/AGENTS.md"
fi
```

Windows PowerShell, from the repository directory:

```powershell
$codexHome = if ($env:CODEX_HOME) { $env:CODEX_HOME } else { Join-Path $env:USERPROFILE ".codex" }
$target = Join-Path $codexHome "AGENTS.md"
New-Item -ItemType Directory -Force $codexHome | Out-Null
if (Test-Path -LiteralPath $target) {
    throw "Merge AGENTS.md with the existing global file; no file was overwritten."
}
Copy-Item -LiteralPath .\AGENTS.md -Destination $target
```

Start a new Codex session after installing. If `AGENTS.override.md` exists in your Codex home, Codex reads it instead of `AGENTS.md`; merge your policy there. Project-level `AGENTS.md` files can add more specific instructions.

## Model and effort

The policy starts with the model and effort already selected. For ordinary work, Codex proceeds and adjusts its investigation and verification to the task. It can briefly recommend one better configuration when the current one is materially limiting the result, and should keep working when useful work remains possible.

The model supplies the underlying capability; effort controls reasoning depth. Codex Pilot sets the default working policy. A Skill adds an optional workflow for a particular task.

```text
Task → current model and effort → work → proportional verification
                    └─ materially insufficient? → recommend one change
                                                    continue if possible
```

The policy does not change the model picker automatically.

## What changes in practice

For a refactor, Codex can inspect the relevant code, make the change, and run the useful tests using the current configuration. It does not need to stop first to ask whether it should switch models.

For a request to delete production data, Codex still needs clear authorization. Fewer interruptions for computation choices do not remove safeguards for actions that affect the user or other people.

For long tasks, the policy asks Codex to carry forward the objective, constraints, decisions, open questions, and current artifact state. It does not require a visible plan or status checkpoint at every phase.

## Skills and related projects

`codex-pilot` is a global operating policy. Skills are optional, task-specific workflows; use them according to the user's choice and the host's rules. This repository does not require users to disable Skills globally.

[Opus Mode for Codex](https://github.com/ncusspm25/opus-mode-for-codex) is a separate optional Skill for long-horizon work. It is not a dependency.

For a shorter policy, see the [minimal example](examples/minimal.md). The [Skill opt-in example](examples/skill-opt-in.md) shows how to require explicit activation for a particular Skill.

## Limits

`AGENTS.md` is instruction text, not an enforcement mechanism. Its effect depends on the Codex surface, version, and higher-priority instructions. It cannot grant tool permissions or guarantee a particular model response. No benchmark or performance improvement is claimed here.
