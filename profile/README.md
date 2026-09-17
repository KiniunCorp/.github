<div align="center" id="user-content-toc">
  <ul>
    <summary>
      <img src="https://github.com/KiniunCorp/assets/blob/main/kiniuncorp/logo_azul.png?raw=true" alt="KiniunCorp" width="460" />
      <h2>Building tools for structured, reliable AI-driven engineering.</h2>
    </summary>
  </ul>
</div>


## What we do

KiniunCorp focuses on improving how software is designed, built, verified, and shipped in the age of AI.

We build tools and systems that add:

- structure to AI-assisted workflows
- clarity to engineering decisions
- evidence instead of claims
- reliability to execution and to what gets shipped
- traceability across the development lifecycle

Our goal is simple:
**turn AI from a powerful assistant into a dependable engineering system.**

Three public tools cover three different points where that breaks down today — the process around the work, the truth of what the work produced, and the artifact that finally runs.


## Projects

| Tool | Problem it solves | Interface | Source |
|---|---|---|---|
| [**Spec-To-Ship** (`s2s`)](https://github.com/KiniunCorp/spec-to-ship) | AI chat sessions have no process, memory, or approval gates | CLI, chat-native | Open source (MIT) |
| [**bramo-verify**](https://github.com/KiniunCorp/bramo-verify) | Agents report success they never verified | CLI, GitHub Action | Free to use, closed engine |
| [**CODI**](https://github.com/KiniunCorp/codi) | Container images ship bloated, unsafe, and unreviewed | CLI, REST API, containers | Open source (MIT) |

Each is usable on its own. Together they cover plan → execute → verify → ship.

---

### Spec-To-Ship (`s2s`)

**A structured engineering workflow for AI-driven development.**

A raw chat session has no memory, no process, and no guardrails: work that needs design gets coded before it's understood, bug fixes skip the spec, and code runs before a human ever sees the plan. `s2s` wraps your existing chat client in a lightweight process layer — stages, approvals, and persistent state — while remaining fully chat-native and tool-agnostic.

- Classifies each request and routes it through the minimum stages needed — a bug fix goes straight to engineering, a new feature routes through product, design, and engineering
- Approval gates before any code executes; execution happens in an isolated git worktree, so your main branch is untouched until you review
- Persistent state and an audit trail across sessions, stored in `.s2s/` as plain files
- Works with Claude Code, Codex, OpenCode, and any AI chat tool that reads file-based governance — no lock-in, no platform dependency

```bash
npm install -g spec-to-ship   # or: brew tap kiniuncorp/s2s && brew install s2s
cd /path/to/your-project && s2s init && s2s doctor
```

→ [Repository](https://github.com/KiniunCorp/spec-to-ship) · [npm](https://www.npmjs.com/package/spec-to-ship) · [Homebrew tap](https://github.com/KiniunCorp/homebrew-s2s)

---

### bramo-verify

**Audits a git diff and reports what it observed — never what an agent claimed.**

Your coding agent says the tests pass. `bramo-verify` spawns them and reads the real exit code. It says the feature is done. `bramo-verify` scans the diff for the shapes unfinished work leaves behind, and cites them at `file:line`. A suite that exits 0 having run zero tests is reported as *inconclusive*, not passed.

- Runs the checks itself rather than trusting the commit message, then issues a verdict backed by cited evidence
- Deliberately tuned to miss things rather than to guess: under-detection is expected, over-detection is treated as a bug
- Available as a CLI today; the GitHub Action for pull-request checks is in progress
- The engine is closed-source, but the *method* is published in full — every check, every finding kind, the verdict JSON schema, and the limits

```bash
npx bramo-verify
# or see it catch a planted agent commit in ~20 seconds:
git clone --depth 2 https://github.com/KiniunCorp/bramo-verify-demo demo && cd demo && npx --yes bramo-verify
```

→ [Repository](https://github.com/KiniunCorp/bramo-verify) · [Documentation](https://bramo.ai/docs/verify) · [Live demo repo](https://github.com/KiniunCorp/bramo-verify-demo)

---

### CODI (Container Dietitian)

**A rules-first, AI-assisted container optimisation toolkit.**

Container images are where good engineering quietly goes to waste — bloated layers, shell-form entrypoints, unsafe patterns, and nobody reviewing any of it. CODI analyses, rewrites, benchmarks, and reports deterministic improvements across Node, Python, and Java stacks. Rules decide; the model only explains and recommends, so results stay reproducible.

- Tolerant Dockerfile parser, stack detection, and stack-specific rewrites sourced from a schema-validated `patterns/rules.yml` catalog
- CMD/ENTRYPOINT analysis that converts shell form to exec form with rationale comments written into the output
- Security gates rejecting risky patterns (privileged, `ADD http://`, sudo) and an air-gap guard enforcing zero outbound calls by default
- Markdown/HTML reports, a reproducible `runs/<timestamp>` artifact layout, a run-aggregating dashboard, and a FastAPI service (`/analyze`, `/rewrite`, `/run`, `/report`)
- Two runtimes: `codi:slim` (rules only, no model dependencies) and `codi:complete` (bundled offline LLM plus lightweight RAG memory), both signed with cosign and published with SBOM attestations

```bash
git clone https://github.com/KiniunCorp/codi.git && cd codi
make setup && source .venv/bin/activate
codi run demo/node && codi report "$(ls -dt runs/* | head -n 1)"
```

> Size and layer metrics in the current release are heuristic estimates from the dry-run build runner; real BuildKit builds land in v0.2.

→ [Repository](https://github.com/KiniunCorp/codi) · [Website](https://codi.kiniun.cloud)


## Philosophy

We believe:

- AI is powerful, but unstructured workflows create risk — so `s2s` adds process
- A claim is not a result — so `bramo-verify` reports only what it observed
- Determinism beats cleverness where correctness matters — so CODI lets rules decide and the model explain
- Engineering requires process, not just output
- Tools should remain simple, transparent, and composable — each of ours is useful alone and better alongside the others
- Open source should be genuinely useful — not crippleware

All three tools reflect these principles, in the same order they appear above.


## Open source and honesty about it

Two of our three tools are open source under MIT: **Spec-To-Ship** and **CODI**. Clone them, read them, fork them.

**bramo-verify** is free to use but its engine is closed-source. We say that plainly in its own README rather than letting you discover it, because a tool about honest reporting should be honest about itself. What we publish instead of the source is the method — every check, the verdict schema, and the limits — the way an auditor publishes its standards rather than its software.

Across all three:

- No core functionality is paywalled
- No artificial limitations between free and paid versions
- Sponsorship supports development — it does not unlock features

If any of this work is useful to you or your team, consider [supporting the projects directly](https://github.com/sponsors/KiniunCorp).


## Working with us

For teams adopting any of our tools — a governed AI workflow with Spec-To-Ship, independent verification of agent output with bramo-verify, or container optimisation with CODI — we may offer:

- setup and enablement guidance
- workflow design
- architecture and process reviews

These are separate from open-source sponsorship and scoped independently.


## Direction

KiniunCorp is focused on a long-term vision:

- making AI-native engineering reliable
- enabling teams to scale AI workflows safely
- building systems that connect planning, execution, validation, and delivery

Spec-To-Ship, bramo-verify, and CODI are three parts of that chain. More projects will be introduced over time, and existing ones keep moving — bramo-verify's GitHub Action and CODI's real BuildKit builds are both in progress.


## Maintained by

KiniunCorp is an independent engineering initiative led and maintained by [Gus Chiriboga](https://github.com/guschiriboga).

Gus is a systems engineer focused on cloud, DevOps, and platform design, with a strong interest in building structured, reliable workflows and solutions for AI-driven development.

### His work centers on:

- turning complex systems into clear, executable processes
- bridging product thinking and engineering execution
- designing tools that scale with real-world usage

Spec-To-Ship, bramo-verify, CODI, and related projects all reflect this approach.

### You can follow his work here:

- GitHub: https://github.com/guschiriboga
- Website: https://gusch.me


## Contact

For project-related topics, use the relevant repository — [spec-to-ship](https://github.com/KiniunCorp/spec-to-ship/issues), [bramo-verify](https://github.com/KiniunCorp/bramo-verify/issues), or [codi](https://github.com/KiniunCorp/codi/issues).

For other inquiries, you can reach out via GitHub.


Building systems, not just tools.
