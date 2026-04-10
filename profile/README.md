<div align="center">

# StatsClaw

### A workflow framework for statistical package development.

**An open-source tool that helps researchers build, test, and document statistical software packages with AI agent teams.**

[Website](https://statsclaw.ai) · [Roadmap](https://github.com/statsclaw/statsclaw/blob/main/ROADMAP.md) · [Contributing](https://github.com/statsclaw/statsclaw/blob/main/CONTRIBUTING.md) · [Discussions](https://github.com/statsclaw/statsclaw/discussions)

---

</div>

## What is StatsClaw?

StatsClaw is a framework for [Claude Code](https://claude.ai/code) that uses **AI agent teams** to assist with statistical package development. You describe what you need — a bug fix, a new feature, a cross-language translation — and StatsClaw coordinates multiple AI agents to help you build, test, and document the result. It works best when a domain expert stays in the loop to guide decisions.

## How It Works

StatsClaw orchestrates a team of **9 specialized AI agents**, each operating under strict information isolation:

| Agent | Role |
|:------|:-----|
| **Leader** | Orchestrates the workflow, dispatches agents, enforces isolation |
| **Planner** | Reads your paper/formulas, executes deep comprehension protocol, produces specifications |
| **Builder** | Writes source code from `spec.md` (never sees the test spec) |
| **Tester** | Validates independently from `test-spec.md` (never sees the code spec) |
| **Simulator** | Runs Monte Carlo studies from `sim-spec.md` (never sees either spec) |
| **Scriber** | Documents architecture, generates tutorials, maintains audit trail |
| **Distiller** | Extracts reusable knowledge for the shared brain (brain mode only) |
| **Reviewer** | Cross-checks all pipelines, audits tolerance integrity, issues ship/no-ship verdict |
| **Shipper** | Commits, pushes, opens PRs, handles package distribution |

The **code**, **test**, and **simulation** pipelines are fully isolated — they never see each other's specs. If all pipelines converge independently, confidence in correctness is high. This is **adversarial verification by design**.

## Supported Languages

<table>
<tr>
<td align=center><b>R</b></td>
<td align=center><b>Python</b></td>
<td align=center><b>Stata</b></td>
<td align=center><b>TypeScript</b></td>
<td align=center><b>Go</b></td>
<td align=center><b>Rust</b></td>
<td align=center><b>C</b></td>
<td align=center><b>C++</b></td>
</tr>
</table>

More languages coming — [Julia is next](https://github.com/statsclaw/statsclaw/issues/3)! Want another? [Let us know](https://github.com/statsclaw/statsclaw/issues/new?template=feature-request.yml).

---

## Quick Start

### Prerequisites

1. **Claude Code** — [Install Claude Code](https://claude.ai/code)
2. **GitHub access** — Push access to your target repository
3. **Workspace repo** — A GitHub repo for storing workflow artifacts (auto-created if needed)

### Install as Plugin (Recommended)

Install once, use everywhere. Open **your own project** and start working:

```bash
cd ~/my-project
claude

# Inside Claude Code:
/plugin marketplace add statsclaw/statsclaw
/plugin install statsclaw@statsclaw
```

After installation, the leader agent activates automatically in any project. Also available in the [Claude Plugin Directory](https://claude.com/plugins).

### Clone the Repo (Alternative)

Clone **statsclaw/statsclaw**, run Claude Code inside it, then point it at your target repo:

```bash
git clone https://github.com/statsclaw/statsclaw.git
cd statsclaw && claude
```

Then inside Claude Code, say `work on <your-repo-url>`.

Also works with Claude Desktop App (open the cloned `statsclaw` folder) or your IDE (open folder, run `claude` in terminal).

### Your First Task

Just tell StatsClaw what you want. It auto-detects the language, selects the right workflow, and starts working:

```
work on https://github.com/your-org/your-package resolve the issues
```

StatsClaw will auto-detect the language, select a workflow, and start working. It will ask you clarification questions when it encounters ambiguity — your domain expertise guides the process. Results vary depending on task complexity; expect to iterate.

---
## Learn by Example

We provide three examples from our own usage. Each is a real repository you can inspect and learn from. Your mileage may vary — these represent what worked for us with active researcher involvement.

### Example 1: Iterative Refactoring of an Existing Package (1→2)

> **Repo:** [`statsclaw/example-fect`](https://github.com/statsclaw/example-fect)
>
> **What it demonstrates:** Multi-day, researcher-guided refactoring of an R package for causal panel data

<details>
<summary><b>The Task</b></summary>

The `fect` R package (fixed effects counterfactual estimators) needed simultaneous work on six interdependent fronts: structural refactoring, CV unification, C++ convergence conditioning, plot overhaul, a 12-chapter Quarto user manual, and bug fixes.

**Prompt:**
```
work on https://github.com/statsclaw/example-fect resolve the issues,
and document any related files to my workspace
```
</details>

<details>
<summary><b>What Happened</b></summary>

Over **5 days and ~20 workflow runs**, with ~10 substantive researcher interactions guiding the process, StatsClaw helped:

- Grow the test suite from **131 to 590 tests** (100% pass rate)
- Make **33+ commits** on the `cfe` branch
- Catch **11+ bugs** through adversarial BLOCK signals — several would have been hard to find manually
- Surface a **balanced-panel algebraic degeneracy** through behavioral testing
- Improve C++ EM convergence accuracy through component-wise monitoring
- Produce a **12-chapter Quarto book** with DGPs and companion scripts

The researcher's domain decisions (e.g., "use `interaction(region, time)`", "reduce tau to 1") were essential — the system could not have made these calls on its own.
</details>

<details>
<summary><b>Key StatsClaw Features Demonstrated</b></summary>

- **Adversarial BLOCK signals as discovery:** The balanced-panel degeneracy was found purely by the Tester's behavioral test — neither the Planner nor Builder anticipated it
- **Sequential validation:** Phase 1 verified before Phase 2 starts, preventing error compounding across interdependent changes
- **Cross-session continuity:** 5 days of coherent development thanks to automatic HANDOFF.md
- **Book-derived tests:** Quarto chapter examples became test cases (+98 tests)
- **Mathematical comprehension:** The convergence fix required understanding the IFE decomposition Y = mu*11' + alpha*1' + 1*xi' + F*Lambda' + epsilon
</details>

---

### Example 2: Building a Python Package from R Source (0→1)

> **Repo:** [`statsclaw/example-R2PY`](https://github.com/statsclaw/example-R2PY)
>
> **What it demonstrates:** Building a Python package from an R reference, with researcher review at each stage

<details>
<summary><b>The Task</b></summary>

Build a Python equivalent of the `interflex` R package for conditional marginal effect estimation, starting from zero Python code. No formal math document provided; the R source is the only specification. Required 3 rounds of researcher feedback to get right.

**Prompt:**
```
Read https://github.com/xuyiqing/interflex This is one R package.
Now build a Python package from it. Only translate the estimator = "linear"
part and all related functions into the new Python package. Ship everything
to statsclaw/example-R2PY and related documents into my workspace.
```
</details>

<details>
<summary><b>What Happened</b></summary>

In **3 iterative rounds** with ~150 words of user input:

**Round 1 — Initial Construction:**
- Planner reverse-engineered all contracts from R source (no math doc existed)
- Builder implemented 14-module dispatcher architecture
- Tester built 34 independent validation tests

**Round 2 — API Refinement:**
- User requested `import interflex; interflex(data, ...)` (callable module pattern)
- Required DML as a mandatory dependency using Python DoubleML
- Builder implemented a `types.ModuleType` subclass to make the module callable

**Round 3 — Quality Audit:**
- Found **6 bugs in code that passed all 34 tests**
- 2 bugs would have produced **silently wrong statistical results**:
  - Bootstrap inference silently fell through to delta method (`elif vartype == "bootstrap": pass`)
  - HC1 sandwich operator precedence (NumPy `*` binds before `@`)

**Final product:** 14 modules, ~3,500 lines, 34 tests, 10-chapter Quarto book. The audit step was critical — without it, 2 silent bugs would have shipped.
</details>

<details>
<summary><b>Key StatsClaw Features Demonstrated</b></summary>

- **Specification recovery:** Planner extracted implicit contracts from R source — what each function guarantees, what invariants hold, what edge cases exist
- **Cross-language translation:** Handled R/Python idiom differences (formula interface, factor handling, freq_weights routing)
- **Bug discovery beyond testing:** 6 bugs in passing code caught by Reviewer's cross-pipeline audit
- **Iterative specification emergence:** The full spec emerged through 3 rounds of dialogue, not upfront documentation
</details>

---

### Example 3: Paper to Package with Monte Carlo Simulation

> **Repo:** [`statsclaw/example-probit`](https://github.com/statsclaw/example-probit)
>
> **What it demonstrates:** PDF manuscript → R/C++ package + Monte Carlo simulation (three-pipeline architecture)

<details>
<summary><b>The Task</b></summary>

Implement three probit estimation methods from a [4-page PDF specification](https://github.com/statsclaw/example-probit/blob/main/probit_spec.pdf) in C++ via Rcpp/Armadillo, then run a comprehensive Monte Carlo study comparing all three.

**Prompt:**
```
Build the R works from this PDF. Three probit estimation methods in C++ via
Rcpp/Armadillo: MLE (Newton-Raphson), Bayesian Gibbs sampler (Albert-Chib
data augmentation), and random-walk Metropolis-Hastings. After building, run
a Monte Carlo simulation comparing all three on bias, RMSE, coverage, and
computational speed across N={200,500,1000,5000} with 500 replications per
scenario. Show all results. Target repo: statsclaw/example-probit. Ship it.
Save the documents in my workspace.
```
</details>

<details>
<summary><b>What Happened</b></summary>

StatsClaw activated **Workflow 11 (Simulation Study)** — the full three-pipeline architecture:

1. **Planner** ingested the PDF, comprehended all three estimation methods (Newton-Raphson MLE, Albert-Chib Gibbs with truncated normals, random-walk MH with proposal tuning), and produced three independent specifications

2. **Builder** (from `spec.md` only) implemented three C++ functions using Armadillo, with R wrappers providing a consistent API

3. **Tester** (from `test-spec.md` only) validated MLE against `glm(family=binomial(link="probit"))` at 10^-6 tolerance, tested Bayesian posterior convergence, verified edge cases

4. **Simulator** (from `sim-spec.md` only) ran **6,000 total simulations** (4 sample sizes x 500 reps x 3 methods), producing comparison tables for bias, RMSE, coverage, and speed

The Simulator treated the estimator as a **black box** — calling the implementation without knowing how it was built, providing a third independent verification pathway.
</details>

<details>
<summary><b>Key StatsClaw Features Demonstrated</b></summary>

- **Paper-to-package pipeline:** PDF equations → working C++ code through the comprehension protocol
- **Three-pipeline architecture:** Code, test, and simulation pipelines all isolated — a bug must fool all three
- **C++ integration:** Rcpp/Armadillo compilation, export attributes, numerical stability (overflow in Phi, Mills ratio near tails)
- **Monte Carlo as verification:** Simulation results independently confirm theoretical properties (unbiasedness, coverage, efficiency)
- **Reduced manual effort:** Much of the boilerplate (Rcpp setup, simulation harness, documentation) was handled by the agents
</details>

---

### Example 4: Paper-Driven Feature Addition (Paper→Feature)

> **Repo:** [`statsclaw/example-panelView`](https://github.com/statsclaw/example-panelView)
>
> **What it demonstrates:** Reading a methodology paper (Correia 2016) to design a new feature, with prerequisite refactoring

<details>
<summary><b>The Task</b></summary>

Add a `type = "network"` visualization to the `panelView` R package: build a bipartite graph from the panel's observation matrix (units × time periods), highlight singletons (degree-1 nodes), draw convex hulls around connected components, and support k-partite graphs for multi-way FE. Based on Figure 2 of Correia (2016).

**Prompt:**
```
Read this paper: Correia (2016), "A Feasible Estimator for Linear Models
with Multi-Way Fixed Effects". I want panelView to make figures like
Figure 2 and identify degree-1 nodes (singletons). Add type = "network":
build a bipartite graph from the panel's observation matrix. Units and
time periods as differently shaped/colored nodes, edges = "unit observed
in period". Highlight singletons, draw convex hulls around connected
components. Support 2+ sets of fixed effects (k-partite). igraph in
Suggests, not Imports. Plan first.
```
</details>

<details>
<summary><b>What Happened</b></summary>

The **Planner** comprehended Correia (2016) and identified that the existing codebase — a monolithic 37KB file — needed refactoring before a new feature could be safely added. StatsClaw made the judgment to **refactor first**:

1. **Refactored monolith** → 4 focused modules (`plot-treat.R`, `plot-outcome.R`, `plot-bivariate.R`, + dispatch core)
2. **Added test suite** covering all plot types, sub-modes, both formula and explicit-variable interfaces
3. **Fixed 3 ggplot2 deprecation bugs** (`size` → `linewidth`, unsafe `class()` checks)
4. **Replaced stale vignette** with 5-chapter Quarto manual
5. **Added ARCHITECTURE.md** (13.9 KB) documenting dispatch architecture and 8-stage data pipeline
6. **CRAN prep**: zero errors, zero warnings in `R CMD check`, version bumped to v1.2.1
7. **Produced specs** for the `type = "network"` feature as a clean new module

</details>

<details>
<summary><b>Key StatsClaw Features Demonstrated</b></summary>

- **Paper comprehension → feature design:** The Planner read a methodology paper not to implement the estimator, but to extract a *visualization concept* from its exposition
- **Proactive refactor-first judgment:** The system identified refactoring as a prerequisite before the user asked for it
- **Test coverage from zero:** Added tests for a package that previously had none
- **CRAN compliance:** Handled warnings, DESCRIPTION fields, stale vignettes systematically
</details>

---

### What Does the Workspace Look Like?

> **Repo:** [`statsclaw/example-workspace`](https://github.com/statsclaw/example-workspace)

Every StatsClaw run automatically generates structured process records — comprehension artifacts, dual specifications, audit trails, run logs, and handoff documents. These are synced to a dedicated **workspace repository**, keeping your target repos clean. See `example-workspace` for the actual artifacts produced during all four examples above, including:

- `comprehension.md` — auditable evidence the system understood your methodology before writing code
- `spec.md` / `test-spec.md` / `sim-spec.md` — independent specifications (Builder, Tester, Simulator never see each other's)
- `review.md` — cross-pipeline convergence audit with per-test results
- `HANDOFF.md` — cross-session continuity (each new session picks up where the last left off)

---

## What Can StatsClaw Help With?

| Task | How it helps | Limitations |
|:-----|:-------------|:------------|
| **Implementing methods** | Assists with translating specs into code | Requires researcher to validate mathematical correctness |
| **Cross-language translation** | Handles R↔Python idiom differences | May miss subtle numerical edge cases without careful review |
| **Testing & validation** | Independent test pipeline catches bugs tests miss | Empirical verification, not formal proofs |
| **Monte Carlo studies** | Automates simulation harness and reporting | Researcher must design meaningful DGPs and metrics |
| **Paper-driven features** | Reads methodology papers to design new functionality | Extracts concepts, not full estimator implementations |
| **Bug fixing** | Adversarial architecture helps find hidden bugs | Complex domain bugs still need human insight |
| **Documentation** | Generates Quarto books, API docs | Needs researcher review for accuracy |

## Shared Brain — Collective Knowledge

StatsClaw has a shared knowledge system where techniques discovered during workflows — mathematical methods, coding patterns, validation strategies, simulation designs — are extracted, privacy-scrubbed, and contributed to a collective knowledge base. When you enable Brain mode, your agents get smarter by reading knowledge contributed by all users.

**How it works:**

1. **Read** — Your agents automatically access relevant knowledge entries from [`statsclaw/brain`](https://github.com/statsclaw/brain)
2. **Contribute** — After noteworthy workflows, the distiller agent extracts reusable knowledge. You review everything and approve or decline — nothing is shared without your explicit consent
3. **Earn badges** — Accepted contributions earn virtual badges on the [Contributors leaderboard](https://github.com/statsclaw/brain/blob/main/CONTRIBUTORS.md)

**Privacy guarantee:** All contributions are automatically scrubbed of repo names, file paths, usernames, proprietary code, and any identifying information. Only generic, reusable knowledge is shared.

| Repo | Purpose |
|:-----|:--------|
| [`statsclaw/brain`](https://github.com/statsclaw/brain) | Curated knowledge — agents read from here |
| [`statsclaw/brain-seedbank`](https://github.com/statsclaw/brain-seedbank) | Contribution staging — users submit PRs here |

Brain mode is optional — you choose at session start. See the [Brain System Documentation](https://github.com/statsclaw/statsclaw/blob/main/.github/BRAIN.md) for full details.

## Example Prompts

```
# Fix a specific issue
fix issue #42 in my-package

# Build from scratch
build a Python package from this R code

# Cross-language migration
rewrite the Python backends in pure R and ship it

# Simulation study
run a Monte Carlo study comparing these three estimators

# Paper to package
build the R works from this PDF

# Paper-driven feature
read Correia (2016) and add network visualization to panelView

# Documentation
update the documentation for v2.0

# Enable shared knowledge
enable brain

# Share what you learned
/contribute
```

## Citation

If you use StatsClaw in your research or software development, please cite:

> Qin, Tianzhu and Yiqing Xu. 2026. "[StatsClaw: An AI-Collaborative Workflow for Statistical Software Development](https://arxiv.org/abs/2604.04871)." arXiv preprint arXiv:2604.04871.

```bibtex
@article{qinxu2026statsclaw,
  title={StatsClaw: An AI-Collaborative Workflow for Statistical Software Development},
  author={Qin, Tianzhu and Xu, Yiqing},
  year={2026},
  eprint={2604.04871},
  archivePrefix={arXiv},
  primaryClass={cs.SE},
  doi={10.48550/arXiv.2604.04871},
  url={https://arxiv.org/abs/2604.04871}
}
```

## License

The StatsClaw framework is released under the [MIT License](https://github.com/statsclaw/statsclaw/blob/main/LICENSE). Brain knowledge entries are shared under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

## Get Involved

We are building StatsClaw in the open. Everyone is welcome.

- **Share an idea** — [Discussions](https://github.com/statsclaw/statsclaw/discussions/categories/ideas)
- **Report a bug** — [Bug report](https://github.com/statsclaw/statsclaw/issues/new?template=bug-report.yml)
- **Contribute code** — [Contributing guide](https://github.com/statsclaw/statsclaw/blob/main/CONTRIBUTING.md)
- **Contribute knowledge** — Enable Brain mode and your discoveries help everyone. Use `/contribute` to share lessons learned. [Learn more](https://github.com/statsclaw/statsclaw/blob/main/.github/BRAIN.md)
- **See what is planned** — [Roadmap](https://github.com/statsclaw/statsclaw/blob/main/ROADMAP.md)

---

<div align="center">

**[statsclaw.ai](https://statsclaw.ai)**

*A tool for statisticians and econometricians. Works best with an expert in the loop.*

</div>
