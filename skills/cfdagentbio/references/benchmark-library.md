# Entry points to classic cases and validation material

Consult when you need to learn a configuration, check an installation, verify a change or find a physical comparison. The following is an index of official upstream material whose retrievable content was checked on 2026-09-22; **it does not mean that CFDAgent has run or accepted these cases locally**, and it does not restrict other solvers or new cases.

| Official entry point | Cases or content to be found | What to watch when using it |
| --- | --- | --- |
| [OpenFOAM backward-facing step, v2212](https://doc.openfoam.com/2212/examples/verification-validation/turbulent/backward-facing-step/) | Turbulent separation and reattachment, with experimental comparison of velocity and wall quantities | Record the release branch, the model and the near-wall settings; matching Re alone is not enough |
| [SU2 tutorial collection](https://su2code.github.io/tutorials/home/) | Laminar flat plate, cylinder, supersonic wedge, ONERA M6 and others | Take the configuration according to the equations, the compressibility and the tutorial version; an example that runs to completion is not a completed validation |
| [FDS official manuals](https://pages.nist.gov/fds/manuals.html) | The separate Verification Guide and Validation Guide | Choose data sets that match the target fire or smoke-and-heat transport phenomenon, and read the experimental conditions and uncertainties |
| [DualSPHysics official test cases](https://github.com/DualSPHysics/DualSPHysics/wiki/7.-Testcases) | Dam break, sloshing, wave generation and notes on the accompanying reference data | Tell apart two- and three-dimensional cases, boundary treatments and gauge positions; check file names against the installed version |
| [Basilisk official test directory](https://www.basilisk.fr/src/test/README) | Tests of VOF advection, capillary waves, a static droplet, embedded boundaries and others | Take the adaptation criterion, the interface quantities and the error norm from the definition of the test concerned; cylindrical Couette flow is not plane Couette flow |
| [Nek5000 official tutorials](https://nek5000.github.io/NekDoc/tutorials.html) | Fully developed laminar flow, periodic hill, conjugate heat transfer and others | Both the number of elements and the spectral-element order affect resolution; do not take these directly as evidence that nekRS has been accepted |
| [PyFR v2.1 official examples](https://pyfr.readthedocs.io/en/v2.1/examples.html) | Euler vortex, Couette flow, incompressible cylinder, Taylor–Green vortex and others | Record the high-order scheme and the backend version; these small examples are not suitable for direct use in performance or scaling studies |

For other solvers, search from the official entry points in [solver-selection.md](solver-selection.md); a gap in this index is not a lack of capability in that solver. Project evidence for PyIB comes from its repository and from actual run records; its flagship position does not earn it an automatic validation conclusion.

## Turning a reference into evidence for this project

Bring in a case only when it serves a real purpose. Record the source URL or DOI, the version or commit, the case name, the equations and parameter range, the boundary and geometry assumptions, the definition of the reference quantity, the units, the reference data files and the uncertainty. When quoting numbers from a figure or table, keep the number of the original figure or table and the extraction method; do not write an impression of an image as a precise experimental value.

Distinguish four things: an official example; official validation data; a local reproduction; and validation of the current operating condition. Record the actual changes, the run identifier, the error in the target quantity, the result files and the range of applicability. The evidence of a reproduced case can be reused, but with a new version, model or parameter change, judge whether it is still covered.

Prefer analytical or manufactured solutions with a defined error to check an implementation; when comparing with experiment, consider input, numerical and model errors separately. A cross-solver comparison can expose differences but cannot automatically prove that a shared result is correct. For how to judge, and how to plan additional computations, see [result-validation.md](result-validation.md).
