English | [繁體中文](README.zh-TW.md)

# Codex Pilot

**Better defaults for how Codex works.**

`codex-pilot` is a reusable global `AGENTS.md` for people who want Codex to carry ordinary authorized work forward without pausing to confirm model or effort settings before every large task.

It changes the instructions Codex follows. It does not change the model picker, grant permissions, override higher-priority rules, or install a runtime. There is no package or daemon.

## Install

Codex reads user-level instructions from `~/.codex/AGENTS.md`. On Windows, the equivalent location is `%USERPROFILE%\.codex\AGENTS.md`. See [OpenAI's Codex guidance for AGENTS.md](https://developers.openai.com/api/docs/guides/latest-model#using-agentsmd).

Back up or merge an existing global `AGENTS.md`; do not overwrite your personal rules. If the file does not exist, copy this repository's `AGENTS.md` to the global location.

macOS or Linux, from the repository directory:

```sh
mkdir -p ~/.codex
if [ -e ~/.codex/AGENTS.md ]; then
  printf '%s\n' 'Merge AGENTS.md with the existing ~/.codex/AGENTS.md; no file was overwritten.'
else
  cp AGENTS.md ~/.codex/AGENTS.md
fi
```

Windows PowerShell, from the repository directory:

```powershell
$target = Join-Path $env:USERPROFILE ".codex\AGENTS.md"
New-Item -ItemType Directory -Force (Split-Path $target) | Out-Null
if (Test-Path -LiteralPath $target) {
    throw "Merge AGENTS.md with the existing global file; no file was overwritten."
}
Copy-Item -LiteralPath .\AGENTS.md -Destination $target
```

Start a new Codex session after installing. Project-level `AGENTS.md` files can add more specific instructions.

## Model and effort

The policy starts with the model and effort already selected. For ordinary work, Codex proceeds and adjusts its investigation and verification to the task. It can briefly recommend one better configuration when the current one is materially limiting the result, and should keep working when useful work remains possible.

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

## Limits

`AGENTS.md` is instruction text, not an enforcement mechanism. Its effect depends on the Codex surface, version, and higher-priority instructions. It cannot grant tool permissions or guarantee a particular model response. No benchmark or performance improvement is claimed here.
