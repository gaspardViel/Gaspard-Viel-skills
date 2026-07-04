# Gaspard Viel Skills

A personal library of agent skills, mirrored from upstream sources (mainly `mattpocock/skills`) and kept in sync via a lock file, for use with Claude Code and other agent harnesses.

## Language

**Source**:
The upstream repository a skill is pulled from, e.g. `mattpocock/skills`. Recorded per-skill in the lock file.
_Avoid_: origin, upstream

**Skill Path**:
The location of a skill's `SKILL.md` inside its source repo, used to find the file when syncing.
_Avoid_: source path

**Computed Hash**:
A hash of a synced skill's content, stored in the lock file so a future sync can detect whether the upstream skill has changed since it was last pulled.
_Avoid_: checksum, version

**Lock File**:
`skills-lock.json` at the repo root — the record of every installed skill's source, skill path, and computed hash.
_Avoid_: manifest, registry

**Skill Mirror**:
One of the two identical skill directories (`.claude/skills/` and `.agents/skills/`) kept in sync so both Claude Code and other agent harnesses can discover the same skills.
_Avoid_: skill folder, skill copy

**Triage Role**:
One of the five canonical states an issue moves through under `/triage`: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`. Mapped to this repo's actual label strings in `docs/agents/triage-labels.md`.
_Avoid_: label, state
