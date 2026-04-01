<div align="center">

# StatsClaw

### Paper in, package out.

**An open-source framework that turns statistical papers, formulas, and conversations into production-ready packages.**

[Website](https://statsclaw.ai) · [Roadmap](https://github.com/statsclaw/statsclaw/blob/main/ROADMAP.md) · [Contributing](https://github.com/statsclaw/statsclaw/blob/main/CONTRIBUTING.md) · [Discussions](https://github.com/statsclaw/statsclaw/discussions)

---

</div>

## What is StatsClaw?

StatsClaw is a framework for [Claude Code](https://claude.ai/code) that uses **AI agent teams** to automate statistical package development. You describe a method — through a paper, a conversation, or a set of formulas — and StatsClaw builds, tests, documents, and ships the package for you.

## How It Works

StatsClaw orchestrates a team of **8 specialized AI agents**, each operating under strict information isolation:

| Agent | Role |
|:------|:-----|
| **Leader** | Orchestrates the workflow, dispatches agents, enforces isolation |
| **Planner** | Reads your paper/formulas, executes deep comprehension protocol, produces specifications |
| **Builder** | Writes source code from `spec.md` (never sees the test spec) |
| **Tester** | Validates independently from `test-spec.md` (never sees the code spec) |
| **Simulator** | Runs Monte Carlo studies from `sim-spec.md` (never sees either spec) |
| **Scriber** | Documents architecture, generates tutorials, maintains audit trail |
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

### Your First Task

Just tell StatsClaw what you want. It auto-detects the language, selects the right workflow, and starts working:

```
work on https://github.com/your-org/your-package resolve the issues
```

That's it. You'll be asked targeted clarification questions when the system encounters genuine ambiguity — answer with domain expertise, and StatsClaw handles all the engineering.

---

## Learn by Example

We provide three complete, real-world examples that demonstrate the full range of StatsClaw's capabilities. Each example is a real repository you can inspect, clone, and learn from.

### Example 1: Resolving Issues in an Existing Package (1→10)

> **Repo:** [`statsclaw/example-fect`](https://github.com/statsclaw/example-fect)
>
> **What it demonstrates:** Sustained, multi-day refactoring of an R package for causal panel data

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

Over **5 days and ~20 workflow runs**, StatsClaw:

- Grew the test suite from **131 to 590 tests** (4.5x increase, 100% pass rate)
- Made **33+ commits** on the `cfe` branch
- Caught **11+ genuine bugs** through adversarial BLOCK signals
- Discovered a **balanced-panel algebraic degeneracy** nobody anticipated — where two-way demeaning absorbs group structure on balanced panels, making extra FEs a no-op
- Improved C++ EM convergence by **43–2,249x** through component-wise monitoring
- Produced a **12-chapter Quarto book** with DGPs and companion scripts
- Generated **28 CHANGELOG entries** across three version milestones
- Ended with **zero known bugs**

The user provided ~10 substantive interactions — all domain decisions like "use `interaction(region, time)` instead of simple region FE" and "reduce treatment effect to tau=1 for realism."
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
> **What it demonstrates:** Greenfield construction of a complete Python package from an R reference implementation

<details>
<summary><b>The Task</b></summary>

Build a Python equivalent of the `interflex` R package for conditional marginal effect estimation — from zero lines of Python code to a shipped, validated product. No formal math document provided; the R source is the only specification.

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

**Final product:** 14 modules, ~3,500 lines, 5 estimators, 3 inference methods, 4 covariance estimators, 5 GLM links, 34 tests, 10-chapter Quarto book
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
> **What it demonstrates:** PDF manuscript → R/C++ package + Monte Carlo simulation study (three-pipeline architecture)

<details>
<summary><b>The Task</b></summary>

Implement three probit estimation methods from a PDF manuscript in C++ via Rcpp/Armadillo, then run a comprehensive Monte Carlo study comparing all three.

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
- **Single-prompt delivery:** Complete package + tests + simulation + documentation from one prompt
</details>

---

## What Can StatsClaw Do?

| Capability | Description | Example |
|:-----------|:------------|:--------|
| **Paper to Package** | PDF/LaTeX equations → production code | Probit methods from manuscript |
| **Cross-Language Translation** | R→Python, Python→R with numerical equivalence | interflex R→Python |
| **Package Refactoring** | Multi-day campaigns with cross-session continuity | fect 5-day refactoring |
| **Monte Carlo Evaluation** | Design and run simulation studies comparing methods | Probit bias/RMSE/coverage study |
| **Issue Resolution** | Fix bugs with adversarial verification | `fix issue #42 in my-package` |
| **Documentation** | Quarto books, API docs, architecture diagrams | 10–12 chapter tutorials |
| **Bug Discovery** | Find bugs that pass tests through independent verification | 6 bugs in passing code (interflex) |

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

# Documentation
update the documentation for v2.0
```

## Get Involved

We are building StatsClaw in the open. Everyone is welcome.

| | |
|:--|:--|
| **Share an idea** | [Discussions](https://github.com/statsclaw/statsclaw/discussions/categories/ideas) |
| **Submit a paper** to turn into a package | [Paper-to-Package request](https://github.com/statsclaw/statsclaw/issues/new?template=paper-to-package.yml) |
| **Report a bug** | [Bug report](https://github.com/statsclaw/statsclaw/issues/new?template=bug-report.yml) |
| **Contribute code** | [Contributing guide](https://github.com/statsclaw/statsclaw/blob/main/CONTRIBUTING.md) |
| **See what is planned** | [Roadmap](https://github.com/statsclaw/statsclaw/blob/main/ROADMAP.md) |

---

<div align="center">

**[statsclaw.ai](https://statsclaw.ai)**

*Built for statisticians and econometricians who want their methods to reach the world.*

</div>
