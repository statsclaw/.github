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

StatsClaw orchestrates a team of specialized AI agents, each with a clear role:

| Agent | Role |
|:------|:-----|
| **Leader** | Plans the work, dispatches teammates |
| **Planner** | Reads your paper/formulas, produces specifications |
| **Builder** | Writes source code from the spec |
| **Tester** | Validates independently (never sees the code spec) |
| **Simulator** | Runs Monte Carlo studies to verify finite-sample properties |
| **Scriber** | Documents architecture and process |
| **Reviewer** | Cross-checks all pipelines before shipping |
| **Shipper** | Commits, pushes, opens PRs |

The **code** and **test** pipelines are fully isolated — they never see each other's specs. If both pipelines converge independently, confidence is high. This is **adversarial verification by design**.

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

## Get Involved

We are building StatsClaw in the open. Everyone is welcome.

| | |
|:--|:--|
| 💡 **Share an idea** | [Discussions → Ideas](https://github.com/statsclaw/statsclaw/discussions/categories/ideas) |
| 📄 **Submit a paper** to turn into a package | [Paper-to-Package request](https://github.com/statsclaw/statsclaw/issues/new?template=paper-to-package.yml) |
| 🐛 **Report a bug** | [Bug report](https://github.com/statsclaw/statsclaw/issues/new?template=bug-report.yml) |
| 🔧 **Contribute code** | [Contributing guide](https://github.com/statsclaw/statsclaw/blob/main/CONTRIBUTING.md) |
| 🗺️ **See what is planned** | [Roadmap](https://github.com/statsclaw/statsclaw/blob/main/ROADMAP.md) |

---

<div align="center">

**[statsclaw.ai](https://statsclaw.ai)**

*Built for statisticians and econometricians who want their methods to reach the world.*

</div>
