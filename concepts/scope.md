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

model = VPDEModel(params)
ode = discretisation(model, params) 
solver = ODESolver(params)

solve(ode, solver)
```

The following types are involved
- `params` is just a dictionary like type which corresponds almost 1:1 to the JSON content.
- `model` keeps the mathematical description in a programming language agnostic fashion without any reference to numerical methods. For example, `model.reaction_terms` might be a function suitable to compute the reaction terms, etc. It contains no data regarding the used numerical method.  
- 