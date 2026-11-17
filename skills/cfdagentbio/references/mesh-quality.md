# Mesh and spatial discretization quality: use what the problem needs

For generating a mesh, reviewing an existing case or locating a computational anomaly; an ordinary discussion of physics does not need the whole set of checks.
Judge which checks are worthwhile from the target quantity and the discretization method in use; existing trustworthy evidence may be used.
When there is no mesh or log, say which quantities have not been measured and carry on discussing the design and the trade-offs.

## First separate the questions to be answered

| Question | Evidence that can be used | What cannot be concluded from it directly |
| --- | --- | --- |
| Are the files and the geometry valid? | Reading by the actual solver, connectivity, volume or mapping checks | That the target physical scales are resolved |
| Does the discretization suit this task? | Local resolution, distance to boundaries, fit to the model, coverage of the motion | That the mesh error is small enough |
| Is the target quantity sensitive to the spatial discretization? | A refinement or order study with comparable settings | That temporal error and model error have been eliminated |
| Are the time advancement and the statistics sufficient? | A time-step study, periodic or statistical convergence | Agreement with experiment, or a correct physical model |
| Can the result be trusted? | Conservation, comparison with theory or experiment, and their uncertainties | That any new operating condition has been validated |

"The mesh check passed" covers only the checks that were run. The look of a mesh, a single stable run or low residuals cannot replace the evidence above.
Comparing total cell counts alone is often not enough: consider the key regions, the distribution of resolution, the near-wall strategy and the discretization order together.

## Read the actual files, then choose the checks

- Confirm the solver family, the distribution and version, the spatial discretization, the number of dimensions, the length unit and the geometric scale.
- Check the boundary names, physical types, normals and directions, periodic pairing, multi-region interfaces and motion definitions against the input configuration.
- Prefer the native reader and mesh diagnostics of that version; record the commands, the input files and the location of the complete output.
- When more is needed, write a script on the spot or call an existing mesh library; there is no need to build a unified checking program in advance.
- Look at where the problems are and how many there are, not only at the maximum value; the effect may differ at a wing leading edge, a trailing edge, a gap or an interface.
- The geometry, or the mesh after conversion or after motion, may have changed; what is checked should be the file or the instant that will actually be solved.

## Body-fitted meshes: finite volume, finite element and high-order methods

**Geometry and connectivity.** Check for degenerate or inverted cells, duplicate or dangling connectivity, face coverage and missing boundaries; interpret signed volumes and mapping directions according to the solver's definitions.
For finite volume, also consider non-orthogonality, skewness, the relation between face and cell centers, abrupt changes of scale and similar metrics, and how the discretization corrects for them.
Whether high-aspect-ratio cells are appropriate depends on direction, on the physical gradients and on the method; a stretched near-wall mesh is not invalid simply because its aspect ratio is high.

**Near-wall region and growth.** Design the first-layer height, the number of layers, the total thickness and the growth rate around the target: laminar flow, wall functions, wall-resolved RANS or LES.
`y+` usually needs an estimate of wall shear or the result of a trial run; state whether it is an a-priori estimate or an actual distribution, and avoid reporting only the mean.
There is no pass line for `y+`, skewness or growth rate that holds across models; cite the applicable version, the model documentation or a validated benchmark before giving a numerical target.

**Boundaries and key structures.** Place resolution according to the wake, shocks, separation, thermal boundary layers, the free surface or narrow gaps.
Check the effect on the target quantity of the far-field and outlet positions, blockage, periodic length and boundary types, with a domain-size study where necessary.
Moving, deforming, sliding or overset meshes need the cell quality, the interfaces and the valid interpolation coverage to be checked over the range of motion; a static initial check is not enough.

**High-order geometry.** A valid linear mesh does not guarantee a valid high-order mapping of curved surfaces; check the mapping Jacobian of curved elements, the boundary geometry and the order according to the method.
Distinguish the geometry order from the solution order, and where necessary compare the target quantity under `h` refinement, `p` refinement or a combination.
The official Nektar++ documentation describes invalid curved elements and the repair mechanisms, and can serve as a basis for judgment; commands should be those of the version installed locally.
[Nektar++ 5.8 NekMesh](https://doc.nektar.info/userguide/5.8.0/user-guidese19.html)

### Native diagnostics: examples

The OpenCFD OpenFOAM v2306 documentation confirms the options below; before use, check the local distribution and `checkMesh -help`.
Run it in the case directory and save the output to a log file chosen by the project:

```sh
checkMesh -allTopology -allGeometry
```

When the user already has a suitable `system/meshQualityDict`, `-meshQuality` can be used as documented for that version.
Dynamic meshes, multiple regions, parallel decomposition and specific times need the corresponding options; one default check cannot stand for all of them.
[OpenCFD v2306 checkMesh official description](https://api.openfoam.com/2306/checkMesh_8C.html)

In SU2, the marker names in the mesh must correspond to the boundary conditions in the configuration; the physical boundary conditions carried in a CGNS file cannot be assumed to have been adopted by SU2.
Read the actual mesh and configuration, and verify them against the preprocessing log of the matching version. For SU2, PyFR and others, do not invent a unified `checkMesh` command.
[SU2 mesh file notes](https://su2code.github.io/docs_v7/Mesh-File/), [SU2 boundary settings](https://su2code.github.io/docs_v7/Markers-and-BC/)

## Immersed boundary: look at the background mesh and the surface discretization together

- Confirm that the background mesh is valid, and that the level transitions and the operator stencils are usable; a geometry that is not body-fitted does not remove the need to check spatial resolution.
- Look at the relation between the local mesh spacing, the surface marker spacing, the area or volume weights, the normals and the support of the kernel in use.
- Check the sampling at curvature, at tips, in narrow gaps and on both sides of thin walls; take the spacing ratio from the basis given by that implementation and kernel, not from a fixed rule of thumb.
- For moving cases, check the whole envelope of motion and representative extreme poses, to ensure that interpolation, spreading and the required fine-mesh band remain supported during the motion.
- When pressure or stress reconstruction needs an additional sampling or correction band, check the coverage and same-side sampling against the actual post-processing implementation; a solve that runs does not guarantee that the data needed for reconstruction are complete.
- When a trial run exists, the boundary velocity deviation, penetration or leakage and the variation of forces can be checked, with refinement of the discretization used to judge their effect.

Thin walls in PyIB can be handled with the author's one-sided, locally infinitely thick approach; confirm against the actual geometry, normals, side and the corresponding implementation.
The geometric range of the standard examples or of the current post-processing templates must not be taken as the upper limit of what the IB method as a whole can do, and a new geometry cannot inherit the validation conclusions of an existing template.
[PyIB current post-processing notes](https://github.com/XuZhaoyue1995/PyIB/blob/main/docs/POSTPROCESSING.md)

## Key points for other discretization methods

**SPH (e.g. DualSPHysics).** Check the particle spacing, the kernel or smoothing length, the neighbor coverage, the particle arrangement and the placement of boundary particles.
In violent motion, watch the change in particle distribution, local voids or clustering and the chosen boundary model, instead of copying cell non-orthogonality metrics.
A resolution study should record the particle spacing and the parameters that change with it, so that a change of model is not attributed to resolution alone.
[DualSPHysics official SPH formulation](https://github.com/DualSPHysics/DualSPHysics/wiki/3.-SPH-formulation)

**LBM (e.g. OpenLB, Palabos).** Check the conversion of length, time, velocity and viscosity from physical units to lattice units.
Resolution, lattice Mach number, relaxation parameter and boundary placement are interrelated; judge stability and error from the collision model and the boundary implementation.
When refining, keep the target physical problem the same and state the conversion strategy; an increase in lattice nodes is not a complete validation.
[OpenLB 1.8 user guide: UnitConverter](https://www.openlb.net/wp-content/uploads/2025/08/olb_ug-1.8r0.pdf)

**Basilisk adaptivity.** Check the fields used for adaptation, the error tolerance of each field, the minimum and maximum levels, the representation of geometry and interfaces, and the updating of fields after adaptation.
Save the level distribution at representative instants; a flow can still be under-resolved at the maximum level. Compare the effect on the target quantity of changing the tolerances and the maximum level.
The adaptive error indicator controls the representation error of the specified fields; it is not a direct error guarantee for the target lift or for an interface-breakup result.
[Basilisk adapt_wavelet implementation](https://basilisk.fr/src/grid/tree-common.h)

**Structured grids and special-purpose models.** For example, with the Cartesian grid of FDS the emphasis shifts to resolution of the physical scales, discretization of the geometry and the interfaces between mesh blocks.
Gas-solid, reacting or multiphysics computations also need checks chosen with the particle treatment, the source terms and the coupling method in mind; geometric quality alone cannot decide suitability.

## A short record, so that the work can be picked up next time

Record in the project's existing notes; the fields below can be merged into a few sentences, and there is no need to create a new table each time.

| Check | Evidence | Observation | Effect on the current goal | Next step |
| --- | --- | --- | --- | --- |
| Specific metric or assumption | File, version, command or log location | Measured value and location, or not yet measured | Whether it affects this judgment, and why | Repair, local refinement, trial run, comparison, or no action for now |

Distinguish measured conclusions, estimates and assumptions still to be verified; when there are structurally invalid cells, locate and repair them before the corresponding computation is used as a valid result.
When the user currently needs only an explanation or a plan, give the relevant judgment; do not block the discussion because complete checking material is missing.
