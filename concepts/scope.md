# Scope

We aim to incrementally port features from the javascript version to other langauges
such as Julia and MATLAB which are suitable to generate data for research and publications.

# Interface 

All backends should share a similar architecture, such that documentation can be shared 
and users can use different backends without too much conceptual changes. 

## Pseudocode example 

The following pseudo code should show how the main script could potentially look like:

```
include VisualPDE-Research

params = load_parameters("problem.json")

prob = VisualPDEProblem(params)
ode = discretisation(prob, params) 
solver = ODESolver(params)

sol = solve(ode, solver)
```

Alternatively, the user could use the more verbose form in which the parameters from the json file are only used to define the model:

```
params = load_parameters("problem.json")

prob = VisualPDEProblem(params, t_end = 10)
ode = discretisation(prob, grid = [100, 100])
solver = ImplicitEuler(dt = 0.1)

sol = solve(ode, solver)

# visualisation
plot(sol[end])          # terminal state
create_video(sol)

# convergence study
res = convergence_test(ode, solver, 10, dt_step = 0.5, grid_step = 1)
plot(res)               # convergence plot
```
Notice that one can either provide the params object (which contains info about the model and the numerical method), or one can provide the missing numerical data manually.


The following types are involved
- `params` is just a dictionary like type which corresponds basically one-to-one to the JSON content.
- `prob` stores the mathematical description of the PDE problem in a programming language agnostic fashion without any reference to numerical methods. For example, `model.reaction_terms` might be a function suitable to compute the reaction terms, etc. It contains no data regarding the used numerical method.  
- `ode` contains the model after spatial discretisation, including all initial and boundary conditions.
- `solver` specifies a general ODE solver without any reference to the model
- `sol` contains the time series of the PDE solution evaluate at specified time-steps.

# Discussion points:

- Where do we store `t_end`? The interface doesn't have a `t_end` yet... Should the user input it each time (as in the current interface)?
  - current state: It will be part of the JSON file.