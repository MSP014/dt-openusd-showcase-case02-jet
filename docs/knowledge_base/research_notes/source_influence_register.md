# Source Influence Register

This register records external sources that influenced Case 02 thinking without
turning those sources into project framing dependencies.

Each entry should state what was useful, what was explicitly not adopted, and
whether the source should drive any durable documentation or implementation
change.

## HiLiftAeroML

- **Title:** HiLiftAeroML: High-Fidelity Computational Fluid Dynamics Dataset
  for High-Lift Aircraft Aerodynamics
- **Primary links:** [arXiv:2605.19565](https://arxiv.org/abs/2605.19565),
  [NVIDIA dataset card](https://huggingface.co/datasets/nvidia/HiLiftAeroML)
- **Domain:** high-lift aircraft aerodynamics, CFD dataset packaging, surrogate
  modelling research

### Why It Matters

HiLiftAeroML is not relevant to Case 02 because of wing aerodynamics,
high-lift configuration, WMLES methodology, or machine-learning benchmark
claims. It is relevant as an example of a well-packaged engineering artefact:
motivation, geometry provenance, parameterisation, workflow, validation framing,
dataset contents, and limitations are made explicit.

Case 02 can reuse that documentation discipline while staying within its own
scope: a portfolio-grade L1 digital twin visualisation prototype for a jet
engine and testbed context.

### Take

- **Motivation-first framing:** explain what the artefact proves before listing
  files or implementation details.
- **Reference geometry provenance:** state what geometry is based on, what is
  public, and what is inferred.
- **Parameterisation matrix:** make the state space explicit instead of leaving
  view modes, engine regimes, visual modes, and module selection implicit.
- **Workflow transparency:** describe how source references, Houdini, USD,
  Omniverse, telemetry, and HUD layers connect.
- **Validation and limitations:** separate validated artefact behaviour from
  claims that are outside scope.
- **Package contents discipline:** make repository contents, external asset
  package boundaries, placeholders, and missing artefacts clear when they become
  concrete.

### Do Not Take

- CFD methodology, WMLES credibility, or solver-specific claims.
- RANS, LES, or high-fidelity simulation framing for Case 02.
- Machine-learning benchmark or surrogate-modelling claims.
- Scientific contribution framing.
- Any implication that Case 02 contains proprietary Rolls-Royce geometry, real
  Trent 1000 telemetry, or certification-grade propulsion modelling.

### Park

- Dataset packaging ideas may become useful when Case 02 has a real external
  asset pack and visible proof assets.
- Parameter split ideas are useful as analogy only; Case 02's state matrix is
  engine regime, view mode, visual mode, and module selection, not aircraft
  geometry variation and angle of attack.

### Possible Follow-Up

- Keep durable Case 02 policy and architecture documents source-agnostic.
- Use this note only as a record of the structural inspiration.
- Revisit package-manifest work after the USD, cache, telemetry, and visual proof
  artefacts are concrete enough to describe honestly.
