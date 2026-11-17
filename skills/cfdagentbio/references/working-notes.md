# Working notes: so that the next conversation can pick up the work

Notes serve the research work; they are not a form that must be filled in. A purely conceptual discussion or a small one-off question usually needs no file.

## Finding the notes and resuming

On entering an existing project, look first at the records the user points to and at the working log already in the project, and avoid starting a second set. If there is none, substantive case work can create `CFD_WORKLOG.md` in that project directory. Several cases can be separated by adjacent files or by the existing project structure, chosen according to scale.

When resuming, locate the case, the inputs, the configuration, the logs and the results from the notes, and then check the current facts. A record that "the job was running yesterday" is not evidence that it is still running today; key states should be queried from the real process or scheduler. If no query is possible in this round, keep the time and mark the state as not re-verified, and do not resubmit blindly on that account.

## What to record

Keep the front page short. It is enough that it answers the following questions, and each need not have its own heading:

- What is being studied now, what are the target quantities, and which decisions has the user already made?
- Where do the geometry, data, units, coordinates, material properties and kinematics come from? Which are still assumptions awaiting confirmation?
- Which solver and version were chosen, and why; which options were compared?
- How far has the work got, and where are the configuration, the mesh, the scripts, the run directory or the Slurm job ID?
- For this round, what are the settled scope of computation, the basis of the resource and time estimates, the applicable upper limits, the stopping conditions and the monitoring arrangement; which extensions or retries have not yet been decided?
- What has been checked, and which file and metric support each conclusion; what has not yet been measured?
- What is the next step, and is there any information the user really needs to supply?

The content changes with the task. If no supercomputer was used, leave out the job ID; if nothing was computed, do not write "computation complete". Avoid copying whole stretches of conversation, long logs, credentials or large arrays into the notes.

## Tracing results back to inputs

It is enough for a run to have its own location or a run identifier the project already uses. Record a snapshot or hash of the key inputs and configuration, the solver version or commit, the necessary commands and environment, and the output directory. A small project does not need a new database, and an existing run list can simply be linked.

When modifying a case, keep the relation between before and after: which physical or numerical parameters were changed, why, and which result belongs to which version. A figure cites the data, the script and the statistical interval that produced it; when the source is lost, mark it as to be verified first, and do not guess the conditions of an old figure from the latest configuration.

Environment information can be kept in the project's existing records and updated when it changes. Write lessons from failures with the version they apply to, the symptom, the evidence, the actual correction and its effect, and keep candidate causes apart from confirmed ones. A case that runs after one parameter change does not automatically prove that the root cause was found. Keep local run records of classic cases separate from the upstream reference data; see [benchmark-library.md](benchmark-library.md).

## When to update

Update briefly at meaningful decisions, run submissions, major checks, corrections of failures and handovers. After submitting a long computation, record the job ID and the directory promptly so that the work can be recovered; there is no need to write an entry for every tool call.

Keep a current summary, and append the key progress below it with times. When correcting a past mistake, note the correction and its basis, and keep failed attempts that are of value. A rerun uses its own run location or the project's existing version management, so that evidence for comparison is not lost. Notes the user already has should be merged and updated, not overwritten wholesale.

## An optional minimal starting point

```markdown
# CFD working notes

Updated: date and time zone
Current goal: ...
Current conclusions and choices: ... (distinguish confirmed, assumed, to be verified)
Related files: ...
Execution status: ... (time of the latest query; if nothing has run, simply write "not run")
Validation evidence: ... (log or data location, metric, applicable criterion; if not yet measured, say so)
Next step: ...

## Key progress
- Time: what was done, on what basis, and where the result files are.
```

"The work can be continued accurately next time" matters more than complete fields. When reporting to the user, summarize the changes and give a link to the notes when needed.
