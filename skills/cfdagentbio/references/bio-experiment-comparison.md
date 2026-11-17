# Comparison with experiments and reliability

Read when comparing a result with experimental data, or when the user asks how close a result is to reality. The general method for judging results is in [result-validation.md](result-validation.md). This reference adds what is particular to living systems. Not every case needs every point, and a quick question gets a direct answer.

## What can be compared

- Quantities measured directly, measured flow fields, and integrated quantities. Check that the quantity is defined in the same way on both sides before comparing values.
- When there are no data to compare with, use relations that must hold physically, such as the balance of forces on a body whose motion is steady or periodic, or the balance of mass or energy. A balance that fails is a reason to check the setup before interpreting the result.
- With imposed motion a balance need not hold. Report the imbalance as a result; do not tune inputs to remove it without a reason.

## Whether the conditions match

- Is it the same individual, the same motion and the same surroundings as in the experiment?
- When the simulation uses one data set and the experiment another, the difference between them includes that mismatch. Say so.

## Keep the sources of difference apart

A difference between computation and experiment can come from four places, and each calls for something different:

- numerical error, addressed by mesh and time-step studies;
- uncertainty of the inputs, addressed by varying them, see [bio-measured-inputs.md](bio-measured-inputs.md);
- variation between individuals and between trials, which no refinement removes;
- measurement error in the experiment.

State which individual and which trial was computed. One computed case is not a population, and repeated cycles of one individual are not independent samples. Do not adjust parameters to match an experiment without justification.

## Periodic motion

- Compare like with like: cycle-averaged values with cycle-averaged values, or phase-resolved curves with the phase aligned.
- Use cycles after the start-up transient, and say which cycles were used.

## Dimensional results and similarity

- By default give results in dimensional form, in the units of the user's data.
- When dimensionless numbers are needed, confirm the choice with the user instead of choosing silently:
  - which numbers are wanted;
  - which reference length, velocity and area they use;
  - what the case is meant to be similar to, such as another individual, a scaled model or an experiment in a different fluid;
  - which similarity parameters can only be matched approximately, and what that implies for the comparison.
- Give the definition together with every dimensionless number that is reported.
