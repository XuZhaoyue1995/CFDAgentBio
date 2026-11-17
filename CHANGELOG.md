# Changelog

## 0.1.0 — 2026-11-16

First release.

- One skill entry point (`cfdagentbio`): the CFDAgent entry point with an added section for biological flow problems.
- Three references read on demand for flows that involve living systems: simplification and cost, inputs that come from measurement, and comparison with experiments.
- The nine general CFD references of CFDAgent 0.3.0, so the plugin can be installed on its own. Eight are included unchanged; the solver map has two added rows, WaterLily and WABBIT, on equal terms with the other candidates.
- Manifests for Claude (`.claude-plugin/plugin.json`) and Codex (`.codex-plugin/plugin.json`) sharing one `skills/` folder.
