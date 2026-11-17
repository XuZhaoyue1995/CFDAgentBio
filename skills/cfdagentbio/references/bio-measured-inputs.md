# Inputs that come from measurement

Read when the geometry, the motion, the material properties or the surrounding conditions of a case come from measuring a living system. In most CFD the inputs come from a design. Here they come from scans and recordings, and they are incomplete and uncertain. This reference covers what to establish about such inputs. It prescribes no input format and no conversion procedure: data are read and converted for the actual files of each project.

## Say where each input comes from

- For the geometry, the motion, the properties and the conditions, record which are measured, which are derived from measurements and which are assumed.
- Record which individual and which trial or occasion the data belong to, and the source files.
- Do not silently fill in something that was not measured. Ask when it would change a decision; otherwise state the assumption that was used.

## Confirm conventions before using the data

- Units, the coordinate frame and its origin, sign and angle conventions, the time base, and what each data channel represents. Confirm them; do not assume them.
- A cheap check is to recover from the converted data a quantity that the source itself reports.

## Processing changes the result

- Smoothing, differentiation, interpolation, resampling and making a record periodic are all choices. The velocity and acceleration of a moving boundary come from differentiating the data, so the computed loads depend on how the data were processed.
- Say what was done, and keep the raw data.
- Where the choice could matter for the target quantity, show its effect instead of assuming it is negligible.

## Uncertain inputs need their own sensitivity study

- Vary an uncertain input within the range the measurements allow, and see how the target quantity responds. This stands beside the mesh and time-step sensitivity; it does not replace them and is not replaced by them.
- When the uncertainty of an input dominates, refining the mesh does not improve the answer. Say which uncertainty limits the result.
- Report the result with the range that the uncertain inputs produce.
- Choose the inputs to vary by which ones the target quantity is most likely to depend on, and keep the number of runs within the scope the user has agreed.
