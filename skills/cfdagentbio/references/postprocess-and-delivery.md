# Post-processing conventions, figures and reproducible delivery

Read as needed when extracting metrics, comparing data or preparing a delivery. Use the solver's native tools, existing scripts and the host's programming ability; when no suitable tool exists, write and check a script for the data at hand. No unified post-processing format is required.

## First confirm what the output quantities mean

- Read the unit, the dimension, the location (cell, node, face, particle, IB marker) and the coordinate system of each field. Two-dimensional force per unit span, three-dimensional total force and an axisymmetric ring integral cannot be mixed directly. Keep the method of any interpolation or resampling, and confirm before integrating whether area or volume weights are needed.
- For pressure, distinguish Pa, kinematic pressure `p/rho` (m²/s²) and lattice or nondimensional quantities, as well as static, total, gauge and absolute pressure. Do not conflate "a pressure in Pa" with "the dynamic pressure q=0.5*rho*U²". With constant density, Pa can be recovered from `p=rho*p_k`; with variable density, `p_rgh` or another transformed pressure, recover it from the equations of the specific solver.
- Record the zero of the pressure reference and the basis of comparison; a pressure difference cancels a common constant, but the reference pressure of an open surface or of a one-sided load usually cannot be changed at will. Determine which way the surface normal points, whether the force acts on the fluid or on the body, and whether the pressure, viscous and body or IB forces are already included, to avoid a flipped sign or a double integration.
- Absolute coordinates, body-fixed coordinates and force directions in an experiment may differ. The directions of lift and drag, and the origin and positive direction of moments, need to be explicit; for velocity in a rotating frame, first confirm whether it is relative or absolute.

The pressure-field examples are based on the [OpenFOAM definition of kinematic pressure](https://doc.openfoam.com/2312/tools/processing/solvers/algorithm-kinematic-pressure/) and on [pressure conversion and the hydrostatic term](https://doc.openfoam.com/2312/tools/post-processing/function-objects/field/pressure/). These describe a specific distribution; other solvers or versions need their own definitions to be checked.

## Coefficients, averages and comparisons

The common conventions are `q_ref = 0.5*rho_ref*U_ref²`, `C_D = F_D/(q_ref*A_ref)`, `C_L = F_L/(q_ref*A_ref)` and `C_p = (p-p_ref)/q_ref`, with the denominator of a moment coefficient multiplied in addition by `L_ref`. Keep the value, the unit and the basis of choice of each reference quantity; a two-dimensional force per unit span uses the corresponding reference area per unit span. When the reference velocity is zero, choose another physically meaningful convention; do not divide regardless.

Different studies may use the mean wing speed, the freestream speed, the projected area or the total wing area; check against the definition of the study, and do not compare coefficients directly because they share a name. Keep correction methods for force, pressure and stress separate from the raw quantities and record the source of each correction; do not force agreement with a reference result by shifting or scaling.

For an average, state the exclusion of the startup phase, the time window, the sampling interval and the method of time weighting or phase averaging. Non-uniformly sampled data cannot be averaged arithmetically as if the samples were equally spaced. If the normalizing denominator varies with time, `mean(F/Q)` generally differs from `mean(F)/mean(Q)`; first establish which quantity the user wants. RMS and the RMS of the fluctuation after removing the mean also need to be distinguished. Align phase and window when comparing across periods, and take the temporal correlation into account in the statistical uncertainty.

## Figures and reports

Scientific figures in the final delivery aim at the quality of expression of a figure in a paper: at the intended display size, the key phenomena, the relations being compared and the data that support the conclusion should be clearly distinguishable. Choose the type of figure, the layout and the visual style on your own according to the content, arrange the information in the figure and in the caption sensibly, and keep the conditions, sources and limitations needed to understand the result.

After exporting, open the actual file, check both the scientific expression and the visual legibility, and fix on your own any overflow, occlusion, clipped text and presentation that could be misread. Simple plots are fine during exploration and diagnosis; the demand on figure quality does not enlarge the scope of computation or validation.

Deliver curves, flow fields, explanations or reports according to what the user currently needs; do not generate long documents automatically. Label the variables, the units or nondimensional definitions, the operating conditions and the time or phase in figures; comparison figures use consistent axes and color scales that can be interpreted, with different color-scale ranges noted where necessary. The vector direction, the slice location and the vortex-identification threshold should be traceable.

Keep the plotting scripts and the sources of their inputs. Give the basis for filtering, cropping, interpolation, smoothing or removal of data, and keep the original data; display settings should not be presented as the numerical result itself. A report distinguishes observation, physical interpretation and hypotheses still to be verified; validation evidence cites actual files, and an attractive flow-field picture is not validation.

Before delivery, use the [completion judgment](../SKILL.md#completion-and-delivery) to check against the user's goal, the scope of execution already discussed and the existing evidence. Explain how the remaining limitations affect the current use: gaps that need resolving are tied to the next step of work, and extensions that serve only higher accuracy or future research are optional. When no explicit accuracy requirement exists, state, in light of the current use, which conclusions can be supported; do not silently invent an acceptance threshold or impose paper-level validation. One main target quantity having settled is no substitute for judging the other target quantities the user cares about.

## Reproducible delivery

When sharing or submitting is needed, assemble the inputs sufficient to recompute the target results, the mesh or discretization data, the solver version and source commit or patches, the dependencies and build environment, the execution commands, the run logs, the analysis scripts and a list of results. Tie figures to run identifiers and file hashes or versions, and keep the previous version of the evidence without overwriting it.

For large data, the storage location, the checksum information and the way to obtain them may be provided instead; there is no need to pack every intermediate field into the archive. State the permissions for using the data and the resources needed for reproduction; before sending anything out, check that the package contains no credentials and no data that should not be public. The interpretation and conversion of geometry and kinematics are handled within the specific project, with the scripts and the basis used kept with the project.

If the package has only been assembled, label it "assembled"; report exactly those commands and metrics that were actually reproduced. When more evidence of credibility is needed, read [result-validation.md](result-validation.md).
