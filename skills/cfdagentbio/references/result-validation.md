# Credibility of results and sensitivity studies

Read as needed when interpreting results, deciding whether to continue computing or planning validation. First establish the target quantity (quantity of interest, QoI) the user cares about and what it will be used for, and reuse the evidence that already exists for the same physical settings, version and parameter range. After the model, boundaries, implementation or resolution change, judge which evidence still applies; do not mechanically rerun every check.

## Choose the evidence that answers the current question

| Evidence | Useful checks | What it cannot establish on its own |
| --- | --- | --- |
| Execution and data integrity | Normal termination, finite values, complete time series and output, no duplicated segment after a restart | A normal exit does not mean the physical solution is correct |
| Conservation and constraints | The applicable balance of mass, momentum, energy or phase volume, including storage terms, boundary fluxes and source terms | A small residual is no substitute for a complete budget; for IB reaction forces, the sign, the control volume and the treatment of the interior fluid must be stated |
| Convergence of target quantities | The trend of forces, pressure drop, flow rate, heat transfer and the like; for steady problems, residuals together with target quantities | One residual threshold does not prove that all target quantities have converged; a genuinely unsteady flow should not be forced to a constant |
| Sufficiency of periods or statistics | Discard the startup phase, compare statistics by period or by block, and consider the correlation time and the effective sample size | Adjacent outputs cannot automatically be treated as independent samples; no fixed count of repeated periods is a universal acceptance criterion |
| Numerical sensitivity | Mesh, particles or order, time step, computational domain and the applicable iteration tolerances | Mesh convergence does not remove physical model error |
| Physical comparison | An analytical solution, an experiment or reliable literature under matching conditions, with the reference uncertainty recorded | Agreement between two solvers is not the same as independent experimental validation |

For a conservation error, keep both the raw budget and the normalizing denominator. When the net flux or the target value is close to zero, choose a physically meaningful scale and report the absolute error, so that dividing by a near-zero number neither manufactures a "huge error" nor hides a problem. A moving control volume must use the velocity relative to the mesh and the corresponding storage term.

Thresholds come from the research goal, the accuracy of the reference or an existing project agreement; do not silently invent a universal "pass line". When the evidence is insufficient, state the specific gap and the next step that would best resolve it; a preliminary trial run can deliver a preliminary conclusion.

## Mesh and time-step studies

- Compare the target quantity over a set of resolutions that has explanatory power; keep the physical parameters, the boundaries, the computational domain and the statistical conventions comparable. Mesh, IB marker spacing, SPH particle spacing, LBM unit mapping and high-order polynomial order each have their own meaning, and the total cell count alone cannot stand in for them.
- When studying spatial error, control the temporal error, the iteration error and the sampling error; when studying temporal error, use sufficient spatial resolution. If they can only be changed together, report the result explicitly as a joint sensitivity.
- Having three data points does not mean that Richardson extrapolation or the GCI is appropriate. First check the refinement relation, the convergence trend and the asymptotic assumption; when the behavior is non-monotonic or oscillatory, or statistical noise dominates, diagnose and add evidence first instead of forcing a three-point formula.
- High-order p refinement, local AMR and strongly anisotropic refinement need an error analysis that applies to them. If only two settings are compared, report the change in the target quantity and its limitations; do not automatically call the result "mesh independent".
- When using the GCI, keep the definition of resolution, the refinement ratio, the order used and its basis, the safety factor, the source of the formula and the judgment of asymptotic behavior. A target quantity close to zero can be reported with a meaningful absolute scale.

Entry points for the method: [NASA spatial convergence and GCI](https://www.grc.nasa.gov/www/wind/valid/tutorial/spatconv.html); [NASA note that convergence cannot be judged from the convergence keyword alone](https://www.grc.nasa.gov/WWW/winddocs/user/keywords/converge.html). Check the conditions of a formula before actually using it, and do not take the example parameters in a guide as the standard for this project.

## Parameter sweeps and cross-solver comparisons

Choose the range and the number of points of a sweep around the scientific question; use existing results first, then add computations according to their information value. The scope of execution follows the resources and budget the user has already authorized. Keep a record of every condition, attempt and failure, so that favorable results are not the only ones selected.

A comparison table records the actual physical parameters and the nondimensional definitions, the models, the boundaries, the resolution, the time step or order, the statistical window and the data source. Across solvers, first align the problem definition, the two-dimensional, three-dimensional or per-unit-span convention and the definitions of force and pressure, and then compare the results; different discretizations are not required to have the same cell count. A comparison of run time should state the hardware, the precision, the number of cores or GPUs, the extent of preprocessing included and the accuracy reached.

Each conclusion points to the raw results and the analysis script, and states briefly what was measured, what judgment it supports and what remains undetermined. When figures, coefficients or exports are needed, read [postprocess-and-delivery.md](postprocess-and-delivery.md); when looking for reference cases, read [benchmark-library.md](benchmark-library.md).
