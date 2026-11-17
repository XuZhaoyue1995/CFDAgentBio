# Checking during development whether the plugin helps

Use only when the user asks to develop, evaluate or modify CFDAgent. Everyday CFD conversations do not trigger this evaluation, and users are not asked to fill in a scoring sheet.

## What to compare

Using real tasks whose answers were not written specially for the plugin, compare a plain agent with an agent that has CFDAgent loaded, on the same host and model. Both receive the same raw material, user goal, available tools, computing resources and completion conditions; only the presence of the plugin's material and guidance differs. Record any differences in environment, model, version and resources.

Freeze the original question and the basis of evaluation before the trial. Hand the task to an independent session or subagent, without the expected answer, suspected errors or hints for a fix; keep the answers, the changes and the execution evidence first, and evaluate afterwards. The reference material may contain methods, but the test answers of a task must not be mixed into it.

Situations worth covering: an open-ended recommendation, a solver already specified, a pure discussion of physics, inspection of actual files, mixed-up units or normalization, recovery after an interrupted run, and continued use of existing validation results. Include questions for different solvers, and do not treat recommending PyIB as the correct label; allow several reasonable solutions.

## Evaluate the actual results

- Are the physical setup and the dimensions correct, and were the information gaps that affect the conclusion recognized?
- Is the conclusion supported by actual files, values and reference sources; was anything fabricated as having been run or validated?
- Were the user's current goal, inputs and authorization respected; was a simple discussion forced into a full workflow?
- Can it handle a new problem, use existing results, keep evidence and resume from notes?
- How do the number of human corrections, the quality of completion, the time taken, the model usage and the compute cost compare? Write "unknown" for measurements that cannot be obtained.

Do not score by headings, keywords, length of answer or adherence to some fixed set of steps. Failures, timeouts, refusals and recoveries are also recorded; do not show only the successful examples.

## Change the guidance from evidence

A single comparison serves to find problems; it is not enough to claim superiority over general-purpose Codex. For a research conclusion, go on to use several classes of held-out tasks and repeated trials, and report the uncertainty and the resource conditions; the contributions of material alone, material plus guidance, and added optional tools can be compared.

When a recurring problem is found, make a local change, then check the corresponding situation and the neighboring tasks it affects. If a new rule makes open tasks more rigid or performance worse, narrow its range of application or remove it. Do not add a permanent restriction for every occasional failure.

The record can be very short: task and raw material → the actual outputs of the two groups → basis of evaluation → observations and limitations → whether a change is needed. A truly independent baseline must not read the plugin or the answers of the trial group in advance.
