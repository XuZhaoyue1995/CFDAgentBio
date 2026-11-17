# CFDAgentBio

![CFDAgentBio](assets/logo.png)

A research companion for CFD of biological flows that works in Claude and Codex: flows around, inside or driven by living systems. Author: **Zhaoyue Xu**.

Affiliation: [Experimental Zoology Group, Wageningen University & Research (WUR)](https://www.wur.nl/en/chair-groups/experimental-zoology/contact), Netherlands.

CFDAgentBio is [CFDAgent](https://github.com/XuZhaoyue1995/CFDAgent) with a thin layer for biological flows on top. It contains the complete general CFD guidance of CFDAgent, so you install only one of the two. The added layer is three short references that the agent reads when the task calls for them. They are written around what biological flow problems have in common, not around particular organisms.

## What you can ask

Start from whatever you have: measured data, values from a publication, or a description in words. For example:

> I have measured geometry and motion for a living system. Discuss with me how to compute the flow first.

> My computed force does not match the experiment. Help me find where the difference comes from.

> Estimate what can be simplified and how much computing this biological flow case will need.

You can also call the skill explicitly: `/cfdagentbio:cfdagentbio` in Claude Code, or `$cfdagentbio` in Codex.

## How it works

- **A conversation, not a procedure.** References are read only when the task touches them. There is no required order and no required input. A quick question gets a direct answer.
- **Depth follows the task.** A casual question is answered as asked; a computation meant for publication gets the additional checks it needs.
- **Missing data are not filled in silently.** When something was not measured or its convention is not given, the agent says which assumption it used.
- **Cheap physical checks along the way**, such as whether the forces on a body balance when its motion is steady or periodic.
- **Limits are stated.** A problem that can only be answered by coupling the flow with a deforming structure is identified as such, with possible routes and their cost, instead of being forced into an imposed-motion computation.

Everything in [CFDAgent](https://github.com/XuZhaoyue1995/CFDAgent) applies as well: you choose the solver, long runs are planned with measured estimates, and "done" is judged against your goal and the actual evidence.

## What's inside

The [skill entry point](skills/cfdagentbio/SKILL.md) is the CFDAgent entry point with one added section for biological flow problems.

| When you need to | Reference used on demand |
| --- | --- |
| Estimate the size of a computation and decide what to simplify | [Simplification and cost](skills/cfdagentbio/references/bio-simplification-cost.md) |
| Work with geometry, motion, properties or conditions that come from measurement | [Inputs that come from measurement](skills/cfdagentbio/references/bio-measured-inputs.md) |
| Compare with experimental data, or judge how close a result is to reality | [Comparison with experiments and reliability](skills/cfdagentbio/references/bio-experiment-comparison.md) |
| Do general CFD work: solver choice, case setup, mesh quality, execution, validation, post-processing, working notes | The nine references of CFDAgent, starting from the [solver map](skills/cfdagentbio/references/solver-selection.md) |

The solver map is the CFDAgent one with two added candidates, WaterLily and WABBIT: 13 external solver families and PyIB, on equal terms. No solver is preferred by default. The other eight general references are identical to those of CFDAgent.

The skill and reference notes are written in English. You can discuss your project in other languages as well; responses depend on the host model.

## What the plugin runs, sends and stores

- CFDAgentBio contains instructions, reference notes, manifests, documentation, license text, and a PNG icon. The package has no executable code, conversion scripts, hooks, MCP server, background jobs, or telemetry client. Installing the package does not itself send project data to the author; host tools and external services process data under their own settings and policies.
- It relies on the host application's own tools. When your work calls for it, and within the permissions you grant, the agent may read and write files in your project, write helper scripts, run solvers, and use a scheduler such as Slurm on systems you have set up.
- For substantive case work, it may create or update a working-notes file (usually `CFD_WORKLOG.md`) in your project.
- The reference notes link to official documentation of third-party solvers and benchmark sources. The agent may open these links when it needs version-specific details.

## Scope and limitations

- CFDAgentBio provides professional guidance and pointers to sources. It does not bundle solvers, input templates or a hosted computation service. Actual simulation needs host file and command tools, a suitable solver, and the required computing environment.
- Conversion of measured geometry and motion data is written and verified for the actual files of each project. The plugin prescribes no input format and ships no conversion pipeline.
- The solver map describes what the official documentation of each solver states. It is not a list of capabilities validated by running them.
- Problems that need two-way coupling between the flow and a deforming structure are outside the current scope.

## Install

CFDAgentBio is being submitted to the Claude plugin directory; availability there depends on the directory's review.

- **Claude:** find CFDAgentBio in the plugin directory on the Customize page in claude.ai or the Claude desktop app. A plugin added there is also available in Claude Code.
- **Local use:** in Claude Code, run `claude --plugin-dir ./CFDAgentBio` from the parent of the `CFDAgentBio` folder; in Codex, add this folder through the plugin installation flow.

Both `.claude-plugin/plugin.json` and `.codex-plugin/plugin.json` live at the repository root and share the same `skills/` folder. If you already use CFDAgent, replace it with CFDAgentBio instead of installing both, because the two skills cover the same general CFD work.

## How to cite

The following papers describe earlier CFDAgent research systems. This release is a lightweight skills plugin, not a distribution of the full implementations in those papers. If the work contributes to your research, please cite the relevant papers:

1. Zhaoyue Xu, Long Wang, Chunyu Wang, Yixin Chen, Qingyong Luo, Hua-Dong Yao, Shizhao Wang and Guowei He. CFDAgent: A language-guided, zero-shot multi-agent system for complex flow simulation. *Physics of Fluids* **37**, 117124 (2025). <https://doi.org/10.1063/5.0294696>
2. Zhaoyue Xu, Hua-Dong Yao, Shizhao Wang and Guowei He. CFDAgent with a general kinematics module: Driving biological flapping flight simulation. *Theoretical and Applied Mechanics Letters* **16**, 100734 (2026). <https://doi.org/10.1016/j.taml.2026.100734>

## License

Copyright © 2026 Zhaoyue Xu.

CFDAgentBio is licensed under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/); the full legal code is in [LICENSE](LICENSE). You may use, adapt and share it for any purpose, including commercially, as long as you give appropriate credit. When you share CFDAgentBio or material adapted from it, please credit it as:

> CFDAgentBio by Zhaoyue Xu (https://github.com/XuZhaoyue1995/CFDAgentBio), licensed under CC BY 4.0.

## Privacy, terms and support

- [Privacy policy](PRIVACY.md): package behavior, host processing, and voluntary support correspondence.
- [Terms of use](TERMS.md)
- Questions and bug reports: [GitHub issues](https://github.com/XuZhaoyue1995/CFDAgentBio/issues)
- [Changelog](CHANGELOG.md)
