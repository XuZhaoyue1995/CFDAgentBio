# Simplification and cost for biological flow problems

Read before computing, or when the user asks what can be simplified, how fine a case should be, how long it will take or what it will cost. Living systems have complex shapes, move actively and span a very wide range of scales, so computing one exactly as it is often cannot be afforded. This reference covers what to weigh. The way to estimate cost from measurements is in [execution.md](execution.md), under "Resource choice and scope of computation", and is not repeated here. None of this is a required sequence: a quick question gets a direct answer.

## Start from the scales

- From the size, the speed and the fluid, first estimate the flow regime and the orders of magnitude: the governing dimensionless numbers, the thinnest layer or smallest feature that must be resolved, and the time scales of the motion. These decide the model and the size of the computation before any detail does.
- Let the estimate decide between laminar and turbulent, steady and unsteady. Do not adopt one by habit. Where the regime is borderline, say so, because the choice of model is then itself an uncertainty.

## What can be simplified

For each simplification a plan relies on, say what it saves and what the result can then no longer answer.

- **Geometric detail.** Which features are kept and which are smoothed away or left out.
- **Two dimensions or three, and symmetry.** A reduced dimension or a symmetry plane removes whatever happens in the direction that was dropped.
- **Surroundings and domain.** Whether the object is computed alone or together with what is around it, and how large the domain is.
- **Given or coupled motion.** The motion and deformation of a boundary can be imposed from data, or computed together with the flow.
  - With imposed motion, the result is the flow and the loads of that motion. It cannot say how the motion would change under other conditions.
  - When the question can only be answered by coupling the flow with the structure, and that is beyond what can be done here, say so plainly and give the feasible routes with their cost. Do not run an imposed-motion case and present it as the answer.
- **Periodic motion.** The first cycles start from fluid at rest and differ from the periodic state. Compare the target quantity from one cycle to the next instead of assuming a fixed number of cycles. Cost grows in proportion to the cycles computed.
- **Transport and mixing.** When the question is about a transported quantity, the scalar field sets the resolution, not the flow. Scalar layers are thinner than velocity layers whenever the quantity diffuses more slowly than momentum, and the cost can then be far higher. Estimate this from the diffusivity before committing to a plan.

## Stating resolution and cost

- Give the resolution relative to the object: cells per characteristic length and across the thinnest feature that is kept. Say what sets the time step.
- Two or three tiers are usually more useful than one number. For each tier give the resolution, the simplifications it uses, the error to expect and what that expectation rests on, the compute time, and the money cost if the price is known.
- Mark every figure as measured or estimated. For a case that has not been computed, do not state a specific cost: propose a short timing run, extrapolate from the measured cost per step, and replace the extrapolation with the measurement once the run exists. The price per core-hour or GPU-hour comes from the user and is not assumed.
