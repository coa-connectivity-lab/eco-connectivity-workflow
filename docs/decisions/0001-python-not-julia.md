# 0001 — Pure Python instead of Julia/Omniscape.jl

- Status: accepted
- Date: 2026-06

## Context

The first connectivity prototype used circuit theory through
[Omniscape.jl](https://docs.circuitscape.org/Omniscape.jl/stable/), running on
Julia 1.10, with the whole toolchain provisioned by [Guix](https://guix.gnu.org/)
for reproducibility.

In practice that stack was hard to stand up the same way twice:

- (i) Guix overrode the SSL certificate path to a store that Julia could not read,
  so `Pkg` could not clone the General registry over HTTPS.
- (ii) The failure surfaced as an unrelated-looking Git SSL error, which cost time
  to diagnose.
- (iii) A `Pkg.Registry.add("General")` call in the setup script triggered the
  failure and turned out to be unnecessary on a normal Julia install anyway.

The recovery steps are recorded in
[`remove_guix_julia.md`](../../remove_guix_julia.md).

Beyond the specific breakage, the deeper problem: the target users are
resource-limited rewilding and restoration organisations. A stack that needs Guix,
a Julia runtime, and hand-held certificate configuration is not one they can
maintain.

## Decision

Drop Julia, Omniscape.jl, and Guix. Rebuild the pipeline in pure Python on a
standard scientific stack:

- pandas, geopandas, rioxarray/xarray, scikit-learn for data handling and the
  suitability models;
- a `scipy.sparse` moving-window solver for circuit theory, replacing
  Omniscape.jl and the unmaintained `circuitscape` PyPI package;
- PostgreSQL/PostGIS as the shared data store;
- QGIS, driven from PyQGIS, for cartography.

Everything installs from conda-forge or apt. No licence fees, no specialist
runtime.

## Consequences

- The circuit-theory step is a straightforward moving-window solve rather than a
  battle-tested library. Its behaviour is checked against known outputs and its
  limitations are stated in the code.
- The whole analysis for the Côa Valley study area (~10,000 km² at 100 m) runs in
  well under a minute on a laptop, so the simpler solver is not a performance
  problem at this scale.
- Partner organisations can reproduce the pipeline with an environment they already
  know how to run.
