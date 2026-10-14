# Kevin's Product Mind

[中文](README.md) · **English** · [한국어](README.ko.md)

A product management Agent Skill inspired by practices at leading Chinese internet companies. It turns vague requests into clear product decisions and PRDs ready for review and development, for products across markets and languages.

## What it does

- Clarifies needs, compares approaches, and defines the MVP and current scope.
- Writes, updates, and reviews PRDs, including flows, business rules, failure handling, and acceptance criteria.
- Supports prioritization, roadmaps, metrics, and experiments.
- Preserves agreed decisions, separates facts from assumptions, and keeps scope changes consistent.

When information is incomplete, it offers a recommendation and identifies key gaps. When requirements are clear, it proceeds. Existing templates and the requested scope take priority. Transcription, translation, format conversion, and implementation of an already agreed design are outside its main purpose.

The Skill and references are currently written in Chinese. You can request another output language. The English and Korean READMEs translate the project guide and request template; behavior across languages still needs testing in your client.

## What's new in V2.0

- **Diagram guidelines:** Cover flowcharts, mind maps, information architecture, product architecture, user journeys, and state diagrams.
- **Clearer diagrams:** Improve layouts, connectors, and complex diagram splitting while preserving business rules, branches, and exception paths.
- **Diagram checks:** Verify consistency with the document, complete paths, and rendered output. Disclose any visual checks that could not be completed.
- **A focused upgrade:** Preserve V1 capabilities and load the new guidelines only for diagram tasks.

## Installation

Download the [V2.0.0 installation package](https://github.com/Kevin-SHANG/kevins-product-mind/releases/download/v2.0.0/kevins-product-mind-v2.zip) and extract it. See the [release notes](https://github.com/Kevin-SHANG/kevins-product-mind/releases/tag/v2.0.0) for details. Place the complete folder containing `SKILL.md` in your client's configured skills directory, keeping the folder name `kevins-product-mind`.

For a setup that uses `~/.codex/skills`, the entry point should be `~/.codex/skills/kevins-product-mind/SKILL.md`. Use your client's path if different. Preserve local changes before updating. Reload skills or start a new session as required.

The Skill, references, request template, and scripts described below are included in the ZIP.

## Request template

This is a translation of the complete template in `需求提示词.md`. Fill in what you know; leave unknown details blank.

```text
Use $kevins-product-mind to help me with the following product request.

Need: [What problem should be solved, and for whom?]
Context and materials: [Current situation, feedback, data, documents, or links]
Agreed decisions and constraints: [Rules to preserve and items outside this release]
Deliverable: [Recommendation / MVP / PRD / proposal review / other]

Read the materials first, then give your judgment. If there is enough information, complete the task. Ask about gaps that affect direction or correctness together; use your judgment for other low-risk, reversible details. Do not reconfirm agreed decisions. Separate assumptions from facts and stay within the requested scope.
```

Add your preferred output language if needed. `$skill-name` invocation depends on the client. If automatic loading is unavailable, provide `SKILL.md` and the references needed for the task explicitly.

## Contents

| File | Purpose |
| --- | --- |
| `SKILL.md` | Entry point, constraints, and reference routing |
| `references/decisions-and-planning.md` | Product decisions, planning, and validation |
| `references/prd-structure.md` | PRD structure, flows, fields, and business rules |
| `references/prd-style.md` | Naming, writing, and delivery checks |
| `references/prd-cases.md` | Before-and-after examples and their limits |
| `references/diagram-layout.md` | Diagram layout, connectors, splitting, and checks |
| `scripts/` | Priority scores and experiment sample sizes |
| `evals/evals.json` | 18 behavioral evaluation scenarios and expectations |

“SYX” is retained as a source label in the original writing guidance. Business parameters in the examples belong to their original context, not default rules for new projects.

## Helper scripts

Python 3.10+ is required only for the scripts, which use the standard library. Use `--help` to view arguments.

```bash
# UTF-8 CSV headers: item,reach,impact,confidence,effort
python3 scripts/score_priorities.py priorities.csv --method rice

# Two independent proportions: 10% baseline, +2 percentage points
python3 scripts/experiment_math.py --baseline 0.10 --mde 0.02 \
  --alpha 0.05 --power 0.8 --allocation 0.5 --sides two --direction increase
```

Scoring supports RICE, ICE, and weighted scores. Use a 0–1 decimal for `confidence`; missing inputs are not filled in. For example, RICE inputs A=(1000,2,0.8,4), B=(2000,1,0.5,5), and C=(500,3,1,2), in header order, yield C=750, A=400, B=200. Results go to standard output or a file specified with `--output`.

The experiment example returns 3,841 per group, 7,682 total. The normal approximation excludes adjustments for clustering, sequential testing, multiple comparisons, and attrition. Collection time alone is not the full experiment duration.

## Evaluation and contributions

`evals/evals.json` defines 18 evaluation scenarios. It retains the original 12 and adds 6 diagram scenarios covering semantic fidelity, complex diagram splitting, information architecture, state loops, rendering limitations, and unknown action order. They must be run and recorded in an actual Agent environment. Their presence does not mean they have passed.

Issues and pull requests are welcome. Include a minimal input, the actual result, and the expected behavior. Use public or anonymized materials. For script changes, include actual execution results; for writing rules, preserve business meaning.

## Acknowledgments

**Special thanks to wami, author of `wami-prd-style`.** Its approach to PRD writing and structure was an important reference in assembling this Skill.

## License

Published by [Kevin-SHANG](https://github.com/Kevin-SHANG). The declared license is MIT; a standalone `LICENSE` file has not yet been provided in the repository.
