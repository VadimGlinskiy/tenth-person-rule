# Tenth-Person Rule

A practical, evidence-driven method for challenging assumptions in software work.

The skill selects one realistic worst-case scenario, traces it through the reachable production path, and checks it with focused evidence: synthetic fixtures for reproducible regressions and, when project rules allow, real data used read-only. It requires a minimal fix and regression test when a defect is reproduced, records evidence and limits when the hypothesis is disproved, and leaves the check open when evidence is inconclusive. It explicitly avoids unbounded fuzzing and unrelated repository audits.

## Install

Install with the [Skills CLI](https://github.com/vercel-labs/skills):

```sh
npx skills add VadimGlinskiy/tenth-person-rule --skill tenth-person-rule --global --agent codex --yes
```

To install into a project instead, remove `--global`. To use another supported agent, replace `codex` with its Skills CLI agent identifier.

The skill file is at [skills/tenth-person-rule/SKILL.md](skills/tenth-person-rule/SKILL.md). You can also copy that file into your agent's skill directory under `tenth-person-rule/SKILL.md`.

## How it works

1. Set the task scope and invariants.
2. Challenge the assumptions the result depends on.
3. Pick the most severe plausible edge case within scope.
4. Trace it from its input through the production route to any write or external effect.
5. Verify it with focused tests and suitable data: synthetic fixtures for reproducible regressions; real data read-only when project rules allow.
6. Fix a reproduced defect, record evidence that disproves the hypothesis, or state the precise blocker.

The skill is guidance for an agent. It does not override user instructions, project rules, safety boundaries, or access controls.

## Field evidence

The skill was applied to real archive, talktable, and document-import readers in a modding tool. Across 12 scoped checks, it helped surface three defects that typical inputs had missed:

- **ERF header over-read:** a minimal 32-byte, zero-entry archive caused an end-of-stream error because the reader consumed 32 bytes after the 16-byte signature instead of only the remaining header bytes. The read was corrected. ErfArchiveTests passed 5/5, including a read-only compatibility check against an 851-entry archive.
- **Valid empty TLK rejected:** a real 100-byte empty talktable was rejected by a 104-byte minimum-size guard. The minimum was lowered to 32 bytes, and a missing string block now returns an empty result instead of parsing metadata as strings.
- **TLK block detection confused metadata with dialogue:** numeric GFF fields could be mistaken for a string-block start, producing phantom entries and hiding early dialogue. The detector was tightened; focused fixtures were adjusted to contain plausible text. TlkReaderTests passed 3/3, and a regular 1,048-string talktable remained readable.

Three other hypotheses were disproved: empty StrictJson and binary garbage with a .dlg extension were rejected cleanly; malformed pack.mcmeta produced no segments and no crash. These were recorded as expected guard behavior, without adding code.

This is one reported application of the method, not a benchmark claim. Real game data used during these checks remained read-only and is not included in this repository.
## License

The skill is distributed under the MIT License. See [LICENSE](LICENSE).
