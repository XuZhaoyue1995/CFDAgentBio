# Computing environment, resources and resuming runs

Read as needed when preparing to execute, choosing resources, resuming a long job or diagnosing a failure. A pure discussion of physics does not need a cluster to be configured; reuse the project's existing environment, scripts and authorization first.

## Environment record

Write the environment facts that will affect later runs or results into the existing [working notes](working-notes.md), and update them when the environment changes:

- The solver name, release branch, version or source commit; the location of the actual executable, module, container or virtual environment.
- The backend actually compiled and run, the numerical precision, the CPU threading or MPI capability, the GPU model and the related runtime libraries.
- Verify single-GPU support, multi-GPU support and cross-node MPI support separately; a machine with several cards does not mean the program can use them together.
- Record the local machine, the login node and the compute nodes separately. A GPU that cannot be detected on the login node does not mean the compute partition has none, and a local success does not prove that the remote system can run the case.
- The partition, account, way of requesting resources, working directory and data-transfer location actually used on the cluster, recorded as the current task requires.

Documentation describes potential capability; the installed version and one successful run each provide more specific evidence. Leave fields that have not been verified as unknown; do not install unrelated solvers just to fill in a table. Record reproducible environment information, and do not write credentials into the notes.

## What a startup trial run can show

A limited startup trial run mainly checks whether the input, the mesh and the boundaries load as expected, whether the solution procedure can advance, and whether the output and the necessary checkpoints are usable, and it measures a first resource cost. There is no fixed number of steps to run, and the first few steps are not required to reach a developed flow field, steady loads or statistical convergence. Report the checks completed and the physical time actually reached; passing the startup check does not mean that long-time stability or the accuracy of the target quantity has been established.

The startup phase may show large variations in pressure, forces or residuals. Judge them together with the initialization, the physical time advanced, the solution quality at the end of each step, conservation and boundary constraints, and the subsequent trend; oscillation or a high residual at the start of a step is not enough to declare failure. The numerical solution requirements that each step should meet still need checking, and NaN/Inf values, solver failure, constraints that keep deteriorating or unexplained growth cannot all be put down to a normal startup. When the evidence is insufficient, schedule a limited continuation within the known resource scope, with a clear target of observation and a stopping condition; do not add dissipation arbitrarily or change the physical conditions to flatten a curve.

If the research target is itself the startup, an impact or a transient peak, that period must be kept and validated; it cannot be discarded wholesale as an irrelevant transient. For the difference between initialization and advancement methods, see the [NASA Wind-US notes on numerical modeling](https://www.grc.nasa.gov/www/winddocs/user/numerical.html); the specific numerical parameters there are not general settings for other solvers.

## Resource choice and scope of computation

Look first at measured records of similar existing cases. When evidence is lacking and the scale of resources would affect a decision, a short trial run may be made within the existing authorization. Record the mesh or particle count, the time step, the physical models, the hardware, the parallel configuration, the time per step, the memory and the output volume. Distinguish the initialization phase from the settled phase of the computation, and state which conditions the extrapolation covers.

Give an estimated range and its basis. Mesh refinement may change the time step, the iteration count and the communication cost at the same time, and more cores are not necessarily faster; do not make linear promises by cell count or device count without a basis. Where needed, compare a small number of feasible resource configurations instead of requesting the maximum outright. A monetary cost can be calculated only when the charging basis is known.

Keep to the scope of computation the project already has, including any applicable total budget, time limit per job, maximum resources, maximum concurrency and permitted number of retries; these are project agreements, and there is no general default allowance. Seek clarification only when missing information would change the decision for this execution; within the existing authorization, carry out preparation, trial runs and fixes on your own.

Base a retry on a correction that can be tested. When the same cause recurs with no new correction, or a known retry or resource limit is reached, stop rerunning that item and record the conditions the next step needs. A long-term research budget cannot automatically be treated as the whole allowance available to the current task.

When moving from the startup check to a long production run, or when enlarging the mesh, the physical duration or the concurrency, first turn the existing data into a short execution plan for this run: why the local CPU, a GPU or the cluster is chosen, how long it is expected to take at what scale, how much memory and disk it needs, what the uncertainties of the estimate are, and how it will be monitored and checkpointed. When only the startup phase has been timed and there are no representative advancement data, a conditional extrapolation can reveal the order of magnitude of the cost but must not be presented as a reliable completion time; any additional trial run that is needed should also have a defined scope.

When significant costs have not yet been discussed, give a small number of feasible options and let the user choose the execution arrangement; when a plan has been settled and authorized, run it directly without asking again. "Start the computation" keeps to the scope of execution already discussed; it does not mean unlimited duration, resources or retries. A mesh or step count written in the configuration likewise does not mean that the user has understood and accepted its actual cost.

Make clear what ends this round: a number of steps or a physical time, the applicable convergence or statistical target, and the agreed limits on time and resources. Record separately that this round's steps have been reached and that the research goal has been reached; if an extension is still needed, handle it within the existing authorization, and when it clearly exceeds the original scope, present the new evidence and settle the adjusted plan.

## Monitoring during execution, and ending

The execution plan should state the method and the scope of monitoring. When the user asks for continuous tracking, or has accepted an execution plan that includes continuous monitoring, use the scheduled checks, scheduler information or monitoring programs that the host actually supports, and track until the job ends or is explicitly stopped; record the job or process identifier, the logs, the checkpoints and the way the next check will be made. Merely saving logs is not monitoring; when continuous querying is not possible, state the actual coverage and arrange a workable way to resume, and do not promise background tracking that has not been implemented.

Choose the checking interval from the expected run time and the risk; avoid querying every second or repeatedly sending a status with no change. Look at progress and the actual time per step, memory and storage, solver diagnostics and the quantities relevant to the goal; update the completion estimate, and distinguish a normal startup, physical unsteadiness and numerical anomalies. When divergence, resource exhaustion or the reaching of an agreed limit is confirmed, save the state as planned and stop or adjust in a controlled way; oscillating loads alone are not a reason to terminate. Stay quiet when there is no change that calls for action, and report on completion, on failure or when the plan needs to change.

After the job ends, check the exit status, the logs, the completeness of the output and the physical time or iteration range actually reached, and then carry out the checks of target quantities and physical results. Continuous tracking does not authorize unlimited automatic reruns or extensions of the computation.

## Slurm jobs

Use the submission scripts provided by the cluster and the help of the installed Slurm version. Before submitting, associate this case, the run directory and the script with a unique run record; after the submission returns, save the raw response, the job ID, the cluster and the time. When the same run record already exists, check the status of its job first, so that resuming a conversation does not cause a duplicate submission.

The following only illustrates the shape of the commands; when running on the cluster concerned, replace the file and `JOB_ID`, and keep to the existing execution authorization:

```sh
sbatch --parsable job.sbatch
squeue --jobs=JOB_ID --format="%i|%T|%R"
sacct --jobs=JOB_ID --parsable2 --format=JobID,State,ExitCode,Elapsed,MaxRSS,AllocTRES
```

`sbatch --parsable` returns the job ID, followed by a semicolon and the cluster name when there is one; save the complete response and then parse the parts separately. A successful submission command means that the scheduler accepted the job, not that the solver has run. [Official sbatch documentation](https://slurm.schedmd.com/sbatch.html)

Keep the job identifier when querying. Array tasks and job steps such as `.batch`, `.extern` and `.0` are not at the same level, and the state on the first line cannot stand for the state of all the computation. `sacct --allocations` looks only at the allocation level and omits the steps; checking the exit code and resource statistics of the actual computation needs the corresponding steps. `MaxRSS` is the maximum observed for a single task within a step, not the total memory of all MPI processes. [Official sacct documentation](https://slurm.schedmd.com/sacct.html)

When `squeue` does not find the job, judge from the accounting records, the logs and the outputs together; an empty result, a failed connection or delayed accounting is first recorded as an undetermined state, and is not automatically classed as a failure or resubmitted. `COMPLETED` still has to be judged together with the computation steps, the solver log and the checks of the results; keep the specific cause for `OUT_OF_MEMORY`, `TIMEOUT`, `NODE_FAIL` and the like. The scheduler state is no substitute for physical validation. [Official squeue documentation](https://slurm.schedmd.com/squeue.html), [official job state codes](https://slurm.schedmd.com/job_state_codes.html)

A long job keeps running under the scheduler and does not need the model to hold the session open. Tie the job ID, the directory and the resuming action to the monitoring plan described above; on the next resumption, read the real state first. Do not set up scheduled automation on your own merely because a task takes a long time; when the user asks for continuous tracking or has confirmed a plan that includes continuous monitoring, carry it out with the capabilities the host provides.

## Failures and lessons learned

First keep the standard output, the error output, the input version and the last usable state at the time of failure. Distinguish input or boundary errors, dependency or startup errors, insufficient memory, termination by the scheduler, hardware faults and numerical divergence, and then choose the correction; do not treat every failure by reducing the time step or adding resources.

After a correction, record it as a linked new attempt: what was changed, on what basis, which checkpoint it inherits and whether the result improved. Before restarting, check the solver's conditions for checkpoint compatibility; keep the evidence of the previous attempt and prevent new output from overwriting it.

For a lesson worth reusing, keep the triggering condition, the solver version, the evidence, the correction that worked and the range in which it applies. A measure that succeeded once remains a lesson under that condition; it is not promoted to a fixed rule for all solvers. Geometry and kinematics data conversion stays within the specific project and is verified against the actual data.
