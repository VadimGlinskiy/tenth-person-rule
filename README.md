# Tenth-Person Rule

A practical, evidence-driven method for challenging assumptions in software work.

The skill selects one realistic worst-case scenario, traces it through the reachable production path, and checks it with synthetic data. It requires a minimal fix and regression test when a defect is reproduced, records evidence and limits when the hypothesis is disproved, and leaves the check open when evidence is inconclusive. It explicitly avoids unbounded fuzzing and unrelated repository audits.

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
5. Verify it with a focused synthetic test.
6. Fix a reproduced defect, record evidence that disproves the hypothesis, or state the precise blocker.

The skill is guidance for an agent. It does not override user instructions, project rules, safety boundaries, or access controls.

## License

The skill is distributed under the MIT License. See [LICENSE](LICENSE).
