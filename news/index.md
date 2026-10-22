# Changelog

## coevolve 1.2.1

#### New Features

- Added further documentation about repeated observations models
  ([\#120](https://github.com/ropensci/coevolve/issues/120))
- Added vignette “Algorithmic scaling”
  ([\#121](https://github.com/ropensci/coevolve/issues/121))
- Models with Gaussian variables and no repeated observations now use a
  conditionally non-centred parameterisation for latent terminal drift,
  i.e. the terminal drift of non-Gaussian variables and of missing
  Gaussian values. Latent drift is written as a standard normal
  innovation conditional on the observed Gaussian residuals of the same
  taxon. This is a change of variables that leaves the model, the
  posterior, and the pointwise log-likelihood unchanged, but removes a
  funnel between the drift parameters and the latent terminal drift that
  caused poor mixing in mixed Gaussian and non-Gaussian models. Thanks
  to [@yuhangxoox](https://github.com/yuhangxoox) for the diagnosis
  ([\#124](https://github.com/ropensci/coevolve/issues/124))

#### Bug Fixes

- Fixed the continuous time intercept term in the expected change along
  a branch, `A^-1 (exp(A t) - I) b`. Since version 1.0.0 this matrix was
  symmetrised before being multiplied by `b`, which is only correct when
  the selection matrix `A` is symmetric (e.g., no cross-selection
  effects). Both the Stan and JAX backends now use the exact expression.
  In our comparisons the selection (`A`) and drift (`Q`) parameters were
  essentially unchanged, because `b` is weakly identified and absorbs
  most of the difference. Posterior estimates of `b` itself can shift;
  in one simulated example they moved by up to about 0.6 posterior
  standard deviations
- Fixed
  [`coev_plot_predictive_check()`](https://docs.ropensci.org/coevolve/reference/coev_plot_predictive_check.md)
  when `nuts_sampler = "nutpie"`. The JAX backend now returns `yrep`
  posterior predictions alongside the other model parameters, matching
  the Stan backend
  ([\#118](https://github.com/ropensci/coevolve/issues/118))
- `log_lik = TRUE` is now supported with `nuts_sampler = "nutpie"`. The
  JAX backend previously ignored the argument and returned no `log_lik`,
  so `loo` and `waic` could not be computed from nutpie fits
  ([\#118](https://github.com/ropensci/coevolve/issues/118))
- Fixed `prior_only = TRUE` for models with Gaussian variables and no
  repeated observations. `terminal_drift` only received a prior inside
  the likelihood block, leaving it improper when the likelihood was
  skipped. Sampling failed to initialise with `nuts_sampler = "nutpie"`
  and was unreliable with `nuts_sampler = "stan"`
  ([\#118](https://github.com/ropensci/coevolve/issues/118))
- Fixed the pointwise `log_lik` and `yrep` for Gaussian variables with
  missing data. The generated quantities block used the `-9999` missing
  value placeholder when constructing residuals, which corrupted the
  conditional densities of the other variables of the same observation

## coevolve 1.2.0

#### New Features

- Added
  [`coev_ancestral_states()`](https://docs.ropensci.org/coevolve/reference/coev_ancestral_states.md)
  for extracting posterior estimates of ancestral trait values at
  internal phylogenetic nodes, on either the latent or response scale
  ([\#86](https://github.com/ropensci/coevolve/issues/86))
- Added vignette “Ancestral state reconstruction”

#### Bug Fixes

- Fixed issue with plotting functions when `nuts_sampler = "nutpie"`
  ([\#114](https://github.com/ropensci/coevolve/issues/114))

#### Other Changes

- Updated documentation
  ([\#111](https://github.com/ropensci/coevolve/issues/111))
- Fixed cmdstanr location in documentation
  ([\#112](https://github.com/ropensci/coevolve/issues/112))
- Responded to [rOpenSci](https://ropensci.org/) package review
  ([\#113](https://github.com/ropensci/coevolve/issues/113))
- Added package citation
  ([\#116](https://github.com/ropensci/coevolve/issues/116))

## coevolve 1.1.0

#### New Features

- Allowed for single traits
  ([\#107](https://github.com/ropensci/coevolve/issues/107))
- Added pure JAX/NumPyro backend via `nuts_sampler = "nutpie"`. Uses
  nutpie’s Rust NUTS sampler with JAX gradients for ~5x faster sampling
  than Stan on typical models. Requires `pip install jax numpyro nutpie`
  ([\#109](https://github.com/ropensci/coevolve/issues/109))

## coevolve 1.0.0

#### New Features

- Added `nutpie` as an alternative sampler for the Stan models
  ([\#92](https://github.com/ropensci/coevolve/issues/92))
- Cached matrix computations when branch lengths are identical
  ([\#93](https://github.com/ropensci/coevolve/issues/93))
- Implemented Hilbert-space approximate Gaussian processes for spatial
  control, adding the `lon_lat` argument and deprecating the `dist_mat`
  argument in
  [`coev_fit()`](https://docs.ropensci.org/coevolve/reference/coev_fit.md)
  ([\#103](https://github.com/ropensci/coevolve/issues/103))

#### Bug Fixes

- Fixed issue with [`summary()`](https://rdrr.io/r/base/summary.html)
  when `estimate_residual = FALSE`
  ([\#95](https://github.com/ropensci/coevolve/issues/95))

#### Other Changes

- Updated license
- Submitted to [rOpenSci](https://ropensci.org/) for package review
- Linted package using [lintr](https://lintr.r-lib.org/)
- Implemented [srr](https://docs.ropensci.org/srr/) compliance checks
- Added continuous integration checks using GitHub Actions
- Reduced cyclomatic complexity for some functions
- Re-factored Stan code generation to use
  [whisker](https://github.com/edwindj/whisker) templates
  ([\#99](https://github.com/ropensci/coevolve/issues/99))
- Implemented automatic test fixture re-generation in GitHub Actions
  ([\#100](https://github.com/ropensci/coevolve/issues/100))

## coevolve 0.1.0

- Initial release version.
