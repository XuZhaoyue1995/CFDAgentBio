# Physical modeling and case setup

Use as needed when discussing models, creating or modifying a case, or troubleshooting a setup. Keep existing data and confirmed choices; ask only about gaps that would change a judgment, and do not turn what follows into a checklist to be filled in every time.

## Let the target quantity decide the model

- First establish which quantity needs to be explained or predicted: mean loads, peak impact, frequency, local stress, mixing or heat flux place different demands on the model and the resolution. Discuss compressibility, laminar or turbulent flow, interphase interaction, heat transfer and moving boundaries on the basis of the relevant scales; do not enable a model merely because the solver supports it.
- Distinguish established facts, provisional assumptions and model choices that need a comparison. When the information is enough to compare methods first, the discussion can go ahead; when a material property or boundary condition that decides the physical problem is missing, do not silently substitute a "typical value".
- Whether a steady computation is appropriate depends on the flow physics and the target quantity. Residuals or forces that keep oscillating may be physical unsteadiness or a numerical problem; check the evidence first, and do not add dissipation arbitrarily to obtain a flat curve. A preliminary trend and a transient peak may need different computational plans.

## Units, reference scales and coordinates

- When connecting data across files, check the original units, the units after entering the solver and the convention of the output. Common confusions include mm/m, ms/s, degree/radian, revolutions per minute/angular velocity, and dynamic/kinematic viscosity. A correct dimension label does not mean the value has been converted correctly.
- For a nondimensional case, record the reference scales adopted for length, velocity, density and so on, and their definitions. For example, with `t*=t Uref/Lref`, the time step, the motion frequency and the output times must be converted accordingly; whether the pressure scale is `ρU²`, `ρU²/2` or another definition should be looked up in the configuration, not guessed. When the reference velocity is close to zero, choose a meaningful scale from what actually drives the flow.
- Changing units is a change of representation of the same physical case; changing the real geometric scale is a different operation. When converting geometry, check the dimensional displacements, velocities, times and material properties together and treat each according to its dimension; if only the geometry is scaled and the other physical quantities are kept unchanged, state explicitly that the Reynolds number and similar parameters may change with it.
- Record the coordinate system, handedness, origin, center of rotation, rotation order and positive directions. After the geometry is rotated, the velocity or motion direction, gravity, the axes on which forces are projected and the boundary normals must correspond to it. Distinguish a rigid-body coordinate transformation from entering a body-fixed or rotating reference frame; the latter also involves relative velocity and the corresponding terms in the equations.
- The name of a pressure field is no substitute for its definition. Check whether it denotes absolute or gauge pressure, kinematic pressure, or pressure with the hydrostatic term removed; for an incompressible constant-density model, the difference between `p/ρ` and Pa directly affects the setup and the interpretation of loads. [OpenFOAM official example](https://doc.openfoam.com/2312/tools/processing/solvers/algorithm-kinematic-pressure/)

## Check that the files describe the same problem

- When editing boundaries, look at the actual boundary names in the mesh, the geometric types and the field configuration together. After renaming, remeshing or switching regions, check whether old names have become invalid, and whether anything has been left out or a default condition applied by mistake. For periodic or coupled interfaces, also check the pairing and the mapping direction.
- Boundary conditions should be compatible with the equations solved, the definitions of the variables and the inflow or outflow direction; avoid improperly imposing interrelated quantities at the same location. Check how the pressure reference is handled, and which physical quantities must be specified under backflow; the specific combination depends on the solver and the model manual.
- The moving-wall velocity, the mesh motion, the embedded-boundary motion and the reference-frame settings should be consistent, so that the same motion is not applied twice. Interpret thin-wall, two-sided or one-sided treatments according to the actual method; the geometric limits of some ready-made example are not a substitute for the methodological capability of the solver.
- When changing a model or the thermophysical properties, check the required initial values, auxiliary equations and boundary fields together. When restarting from old results, check that the mesh, the fields and the physical settings are still compatible; when mapping or reinitialization is needed, keep the original results.

## Numerical settings that support the current goal

- Choose the time step or CFL number according to the actual discretization, time integration and mesh. Convection, acoustics, diffusion, interfaces and motion may each bring a different limit; do not carry one universal threshold across methods.
- An implicit method running stably does not mean that the temporal accuracy is sufficient, and the physical time step is not the same thing as a steady-state pseudo-time step or a dual-time inner iteration. When motion, vortex shedding or impact must be resolved, judge from the relevant time scales and from the sensitivity of the target quantity to the time step, and also check whether each physical step is solved adequately.
- The near-wall treatment should match the turbulence model and the local mesh. Discretization schemes and limiters need both stability and dissipation to be considered; if a temporary stabilizing measure changes the target quantity, say so in the notes and review it. A lower residual alone does not show that the physical result has improved.
- Reliable existing settings can be kept as they are. When proposing new parameters, state whether the basis is the manual of the matching version, a similar validated case or an estimate still to be tested; adjust according to the problems a short trial run exposes, rather than scanning all parameters indiscriminately.

External geometry or kinematics data are handled case by case: read the project's actual files and definitions, and write and check conversion programs as needed. This reference does not set up a unified data-conversion framework.

## When further checking is needed

- [OpenFOAM file format and dimensions](https://www.openfoam.com/documentation/user-guide/2-openfoam-cases/2-2-basic-inputoutput-file-format): check field dimensions and the meaning of inputs; for other solvers, verify against their actual version.
- [NASA notes on temporal convergence](https://www.grc.nasa.gov/www/wind/valid/tutorial/tempconv.html): unsteady results need attention to sensitivity to the physical time step.
- [NASA Wind-US notes on time advancement](https://www.grc.nasa.gov/www/winddocs/user/keywords/temporal.html): a concrete example of physical time steps versus pseudo-time inner iterations; the parameter syntax and defaults there do not carry over to other programs.
