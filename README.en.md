# Kevin's Product Mind

[中文](README.md) · **English** · [한국어](README.ko.md)

**A product-manager Agent Skill built on the product practices of China's major internet companies. It turns vague requests into well-grounded product decisions and writes PRDs that are reviewable, buildable, and consistent with the prototype. It works across markets and languages.**

Requirement clarification, trade-offs, prototype reconciliation, scope control, and PRD writing run in one workflow. When information is missing, it proposes a direction and names the key gaps. When the request is settled, it writes directly. When work spans several stages or sessions, it follows a file-backed path — feature breakdown, rule baseline, prototype reconciliation, one module at a time — instead of relying on the model's memory.

## What it does

| Scenario | Main output |
| --- | --- |
| Idea, feedback, or request still unclear | Brief, recommendation, key trade-offs, open questions |
| Core solution and scope agreed | PRD, feature rules, main flow, failure handling, acceptance |
| Prototype already exists | Page mapping, state matrix, row-by-row rule/prototype comparison, prototype issue list, and a PRD that matches the prototype |
| A prototype is needed first | Prototype blueprint (pages, state matrix, interaction surfaces, copy, forbidden content), confirmed before screens are drawn |
| Large work across stages or sessions | Feature breakdown, decision baseline, stage-close audit, handoff template; the PRD is delivered one module per round |
| Revising or reviewing a proposal | In-scope revisions, logic issues, concrete suggestions |
| Planning and validation | MVP, priorities, roadmap, metric definitions, experiments |

The writing focuses on condition-and-result rules, a single home for each rule, a complete chain from inputs to page fields, and the boundary between product rules and implementation. PRDs are written for people first: a reader can follow one user task from start to finish, while cross-module policies such as deletion scope are consolidated into one table. Facts, assumptions, unknowns, and true zeros are kept apart; sample data and unverified effects are never written as commitments.

Pure transcription, translation, format conversion, and coding of an already-decided solution are not its main purpose. Your templates, writing samples, and task scope remain important inputs.

The Skill and references are written in Chinese; you can request another output language. The English and Korean READMEs translate the repository description and request template; cross-language results should be verified in your own client.

## What's new in V5.0

V5 is a rework aimed at the problems V3 showed in the prototype stage and the full-PRD stage: prototypes and PRDs that did not match, decisions lost between stages, and checks that counted items instead of testing readability.

- **Prototype reconciliation**: New `references/prototype.md`. With a prototype as input, the Skill builds a page mapping, a state matrix, and two row-by-row comparison tables before writing. Visible interactions are referenced to specific pages; invisible rules stay in the body. Temporary notes, sample data, and choice buttons with no observable difference are reported as prototype issues, not written as product rules.
- **Long-running work**: New `references/long-project.md`. Staged work starts with a feature breakdown; confirmed decisions are written to a decision baseline in the same turn and re-read before each step. Stage-close audits, file-based handoffs, and per-feature-line subagents are supported.
- **No silent overrides**: When a request conflicts with a recorded decision, an explicit change is applied and logged; only unclear intent triggers a question. No repeated confirmations.
- **Readability vs. consolidation**: How a rule is written depends on its shape. Single-task rules stay local, one-line shared rules are restated where used, and policies compared across objects and cases become one table.
- **Honest checks**: Counts, mappings, and passing syntax do not prove that a flow works or a document reads well. A two-way check for linear readability, consolidation, and consistency is added.

See the [V5.0.0 release notes](https://github.com/Kevin-SHANG/kevins-product-mind/releases/tag/v5.0.0).

## Installation

Download the V5.0.0 package and extract it. Place the complete folder containing `SKILL.md` in your client's configured skills directory, keeping the folder name `kevins-product-mind`.

For a setup that uses `~/.codex/skills`, the entry point should be `~/.codex/skills/kevins-product-mind/SKILL.md`. Use your client's path if different. Preserve local changes before updating. Reload skills or start a new session as required.

The Skill, references, request templates, and scripts described below are included in the ZIP.

## Request template

This is a translation of the new-request template in `需求提示词.md`. Fill in what you know; leave unknown details blank.

```text
Use $kevins-product-mind to help me with the following product request.

Need: [What problem should be solved, and for whom?]
Context and materials: [Current situation, feedback, data, documents, or links]
Existing prototype: [Where it is; leave blank if none. Which wins when UI and rules conflict]
Agreed decisions and constraints: [Rules to preserve and items outside this release]
Working mode: [Write in one go / proceed in stages]
Deliverable: [Recommendation / MVP / PRD / proposal review / other]

Read the materials first, then give your judgment. If there is enough information, complete the task. Ask about gaps that affect direction or correctness together; use your judgment for other low-risk, reversible details. Do not reconfirm agreed decisions. Separate assumptions from facts and stay within the requested scope.
```

The same file also contains a stage-handoff template for compressing context or starting a new session: current stage, baseline files, first task, and what not to do.

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
| `references/prototype.md` | New in V5: prototype reconciliation and prototype output |
| `references/long-project.md` | New in V5: staged and cross-session work |
| `scripts/` | Priority scores, experiment sample sizes, and a PRD trace scanner |
| `evals/evals.json` | 34 behavioral evaluation scenarios with 11 fixtures |

“SYX” is retained as a source label in the original writing guidance. Business parameters in the examples belong to their original context, not default rules for new projects.

## Helper scripts

Python 3.10+ is required only for the scripts, which use the standard library. Use `--help` to view input formats.

```bash
python3 scripts/score_priorities.py priorities.csv --method rice
python3 scripts/experiment_math.py --baseline 0.10 --mde 0.02 \
  --alpha 0.05 --power 0.8 --allocation 0.5 --sides two --direction increase
python3 scripts/scan_prd_traces.py PRD.md --allow-codes F-A,Q1 --allow-terms report_id
```

The experiment example returns 3,841 per group, 7,682 total, under a normal approximation without clustering, sequential-testing, multiple-comparison, or attrition adjustments. The scanner reports process traces, invented codes, English identifiers, and bare references such as “see prototype” that name no page; `--unfinished` also reports placeholder and TODO markers. It reports matches only and exits with code 1 when it finds any.

## Evaluation and contributions

`evals/evals.json` defines 34 scenarios; V5.0 adds 13, covering PRDs written from prototypes, prototype/rule conflicts, prototype updates, persisting and recalling asset names, explicit vs. unclear changes, structural changes, new-session handoffs, module-by-module writing, blueprint-first and breakdown-first prototyping, and the balance between local rules and consolidated tables.

These scenarios were actually run for this release: 34 trial runs with Kimi K3 and DeepSeek as the tested models. The 13 new scenarios passed 8/13 on the first round and 13/13 after the rules were fixed, plus an old-vs-new comparison on a real hardware project. Details are in the release notes. Scenario files alone do not guarantee results in your client; reruns in your own environment are welcome.

Issues and pull requests are welcome. Include a minimal input, the actual result, and the expected behavior. Use public or anonymized materials. For script changes, include actual execution results; for writing rules, preserve business meaning.

## Acknowledgments

**Special thanks to wami, author of `wami-prd-style`.** Its approach to PRD writing and structure was an important reference for this Skill.

## License

Published by [Kevin-SHANG](https://github.com/Kevin-SHANG) under the MIT license. The repository does not yet include a separate `LICENSE` file.
