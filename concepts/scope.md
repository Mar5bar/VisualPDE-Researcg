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

params = load_parameters("model.json")

model = VisualPDEModel(params, t_end = 10)
ode = discretisation(model, params) 
solver = ODESolver(params)

sol = solve(ode, solver)
```

Alternatively, the user could use the more verbose form in which the parameters from the json file are only used to define the model:

```
params = load_parameters("model.json")

model = VisualPDEModel(params, t_end = 10)
ode = discretisation(model, grid = [100, 100])
solver = ImplicitEuler(dt = 0.1)

sol = solve(ode, solver)
```
Notice that one can either provide the params object (which contains info about the model and the numerical method), or one can provide the missing numerical data manually.


The following types are involved
- `params` is just a dictionary like type which corresponds basically one-to-one to the JSON content.
- `model` stores the mathematical description in a programming language agnostic fashion without any reference to numerical methods. For example, `model.reaction_terms` might be a function suitable to compute the reaction terms, etc. It contains no data regarding the used numerical method.  
- `ode` contains the model after spatial discretisation, including all initial and boundary conditions.
- `solver` specifies a general ODE solver without any reference to the model
- `sol` contains the time series of the PDE solution evaluate at specified time-steps.

# Discussion points:

- Where do we store `t_end`? The interface doesn't have a `t_end` yet... Should the user input it each time (as in the current interface)?