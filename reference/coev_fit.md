# Fit Bayesian dynamic coevolutionary model in Stan

Fit Bayesian dynamic coevolutionary models in Stan for full Bayesian
inference. The model allows users to assess causal directionality (X -\>
Y vs. Y -\> X) and contingencies (X, then Y) in evolution. Several data
types are supported, including binary, ordinal, count, continuous, and
positive real variables. The model can additionally account for missing
data, repeated observations, and controls for spatial proximity. Model
fit can be assessed and compared with posterior predictive checks and
cross-validation.

## Usage

``` r
coev_fit(
  data,
  variables,
  id,
  tree,
  effects_mat = NULL,
  complete_cases = FALSE,
  lon_lat = NULL,
  dist_k = NA,
  dist_cov = "exp_quad",
  measurement_error = NULL,
  prior = NULL,
  scale = TRUE,
  estimate_correlated_drift = TRUE,
  estimate_residual = TRUE,
  log_lik = FALSE,
  prior_only = FALSE,
  adapt_delta = 0.95,
  nuts_sampler = "stan",
  backend = deprecated(),
  dist_mat = deprecated(),
  ...
)
```

## Arguments

- data:

  An object of class `data.frame` (or one that can be coerced to that
  class) containing data of all variables used in the model.

- variables:

  A named list identifying variables that should coevolve in the model
  and their associated response distributions as character strings (e.g.
  `list(var1 = "bernoulli_logit", var2 = "ordered_logistic")`). Variable
  names must refer to valid column names in data. Currently, the only
  supported response distributions are `bernoulli_logit`,
  `ordered_logistic`, `poisson_softplus`, `negative_binomial_softplus`,
  `normal`, and `gamma_log`. Bernoulli variables must be 0/1 integers,
  ordered variables must be ordered factors, Poisson and negative
  binomial variables must be positive integers, normal variables must be
  continuous numeric, and gamma variables must be positive numeric.

- id:

  A character of length one identifying the variable in the data that
  links rows to tips on the phylogeny (strictly case-sensitive). Must
  refer to a valid column name in the data. The id column must exactly
  match the tip labels in the phylogeny.

- tree:

  A phylogenetic tree object of class `phylo` or `multiPhylo`. The
  tree(s) must be rooted and must include positive non-zero branch
  lengths. All trees in `multiPhylo` objects must have the same number
  of internal nodes and branches.

- effects_mat:

  (optional) A boolean matrix with row and column names exactly matching
  the variables declared for the model. If not specified, all
  cross-lagged effects will be estimated in the model. If specified, the
  model will only estimate cross-lagged effects where cells in the
  matrix are TRUE and will ignore cross-lagged effects where cells in
  the matrix are FALSE. In the matrix, columns represent predictor
  variables and rows represent outcome variables. All autoregressive
  effects (e.g., X -\> X) must be TRUE in the matrix.

- complete_cases:

  (optional) Logical. If `FALSE` (default), all missing values are
  imputed by the model. If `TRUE`, taxa with missing data are excluded.

- lon_lat:

  (optional) A `data.frame` containing longitude and latitude values for
  all tips on the phylogeny. The data frame must contain the following
  columns: a column labelled "id" that exactly matches the tip labels on
  the phylogeny; a column labelled "longitude" that declares longitude
  values in decimal degrees; and a column labelled "latitude" that
  declares latitude values in decimal degrees. If specified, the model
  will additionally control for spatial location by including a separate
  Gaussian Process over locations for every coevolving variable in the
  model.

- dist_k:

  An integer of length one specifying the number of basis functions for
  computing Hilbert-space approximate Gaussian Processes over locations.
  If `NA` (the default), exact Gaussian Processes are computed.

- dist_cov:

  A string of length one specifying the covariance kernel used for
  Gaussian Processes over locations (strictly case-sensitive). Currently
  supported are `"exp_quad"` (exponentiated-quadratic kernel; default),
  `"exponential"` (exponential kernel), and `"matern32"` (Matern 3/2
  kernel).

- measurement_error:

  (optional) A named list of coevolving variables and their associated
  columns in the dataset containing standard errors. Only valid for
  normally-distributed variables. For example, if we declare
  `variables = list(x = "normal", y = "normal")`, then we could set
  `measurement_error = list(x = "x_std_err")` to tell the function to
  include measurement error on `x` using standard errors from the
  `x_std_err` column of the dataset.

- prior:

  (optional) A named list of priors for the model. If not specified, the
  model uses default priors (see `help(coev_fit)`). Alternatively, the
  user can specify a named list of priors. The list must contain
  non-duplicated entries for any of the following parameters: the
  autoregressive effects (`A_diag`), the cross effects (`A_offdiag`),
  the Cholesky factor for the drift matrix (`L_R`), the drift std. dev.
  parameters (`Q_sigma`), the continuous time intercepts (`b`), the
  ancestral states for the traits (`eta_anc`), the cutpoints for ordinal
  variables (`c`), the overdispersion parameters for negative binomial
  variables (`phi`), the shape parameters for gamma variables (`shape`),
  the sigma parameters for Gaussian Processes over locations
  (`sigma_dist`), the rho parameters for Gaussian Processes over
  locations (`rho_dist`), the residual standard deviations when there
  are repeated observations (`sigma_residual`), and the Cholesky factor
  for the residual correlations when there are repeated observations
  (`L_residual`). These must be entered with valid prior strings, e.g.
  `list(A_offdiag = "normal(0, 2)")`. Invalid prior strings will throw
  an error when the function internally checks the syntax of resulting
  Stan code.

- scale:

  Logical. If `TRUE` (default), variables following the `normal` and
  `gamma_log` response distributions are scaled before fitting the
  model. Continuous variables following the `normal` distribution are
  standardised (e.g., mean centered and divided by their standard
  deviation) and positive real variables following the `gamma_log`
  distribution are divided by the mean value without centering. This
  approach is recommended when using default priors to improve
  efficiency and ensure accurate inferences. If `FALSE`, variables are
  left unscaled for model fitting. In this case, users should take care
  to set sensible priors on variables.

- estimate_correlated_drift:

  Logical. If `TRUE` (default), the model estimates the off-diagonals
  for the \\Q\\ drift matrix (i.e., correlated drift). If `FALSE`, the
  off-diagonals for the \\Q\\ drift matrix are set to zero.

- estimate_residual:

  Logical. If `TRUE` (default), the model estimates residual standard
  deviations and residual correlations when there are repeated
  observations for taxa. If `FALSE`, residual standard deviations and
  residual correlations are not estimated. The latter may be preferable
  in cases where repeated observations are sparse (e.g., only some taxa
  have only few repeated observations). This argument only applies when
  repeated observations are present in the data.

- log_lik:

  Logical. Set to `FALSE` by default. If `TRUE`, the model returns the
  pointwise log likelihood, which can be used to calculate WAIC and LOO.

- prior_only:

  Logical. If `FALSE` (default), the model is fitted to the data and
  returns a posterior distribution. If `TRUE`, the model samples from
  the prior only, ignoring the likelihood.

- adapt_delta:

  Target acceptance probability for the NUTS sampler (default `0.95`).
  Passed to cmdstanr::sample() as `adapt_delta`, or to nutpie as
  `target_accept`.

- nuts_sampler:

  Which NUTS implementation to use. `"stan"` (default) fits the Stan
  model with CmdStan via cmdstanr. `"nutpie"` fits a pure JAX model
  using nutpie's Rust NUTS sampler with JAX gradients via
  `jax.value_and_grad`.

- backend:

  Deprecated. Use the `nuts_sampler` argument instead.

- dist_mat:

  Deprecated. Use the `lon_lat` argument instead.

- ...:

  Additional arguments. For `nuts_sampler = "stan"`: passed to
  cmdstanr::sample() (and compilation via `extra_stanc_args`,
  `extra_compile_args`). For `nuts_sampler = "nutpie"`: sampling
  arguments such as `chains`, `iter_sampling`, `iter_warmup`, `seed`.

## Value

Fitted model of class `coevfit`

## Details

The `coev_fit` function generates the Stan code for the model, generates
the data list, and then compiles and fits the model using the cmdstanr
package.

Fit a Bayesian dynamic coevolutionary model in Stan. A general overview
is provided in the vignette
[`vignette("coevolve")`](https://docs.ropensci.org/coevolve/articles/coevolve.md)

**Available response distributions**

Variables that should coevolve in the model are declared in the
`variables` argument, using a named list with associated response
distributions. For example:
`list(x = "bernoulli_logit", y = "ordered_logistic")`. Currently, the
only supported response distributions are `bernoulli_logit`,
`ordered_logistic`, `poisson_softplus`, `negative_binomial_softplus`,
`normal`, and `gamma_log`.

**Default prior distributions**

If priors are not explicitly declared by the user, default priors are
used. Default priors were chosen to be weakly regularising, which
improves model fitting and conservatism in parameter estimates. Default
priors for Stan parameters are as follows:

- `A_diag` (autoregressive effects) = `std_normal()`

- `A_offdiag` (cross effects) = `std_normal()`

- `L_R` (Cholesky factor for drift matrix) = `lkj_corr_cholesky(4)`

- `Q_sigma` (drift std. dev. parameters) = `std_normal()`

- `b` (continuous time intercepts) = `std_normal()`

- `eta_anc` (trait ancestral states) = `std_normal()`

- `c` (ordinal cutpoints) = `normal(0, 2)`

- `shape` (shape parameters for gamma distributions) =
  `gamma(0.01, 0.01)`

- `sigma_dist` (sigma for Gaussian process over locations) =
  `exponential(1)`

- `rho_dist` (rho for Gaussian process over locations) =
  `exponential(5)`

- `sigma_residual` (residual standard deviations) = `exponential(1)`

- `L_residual` (Cholesky factor for residual correlations) =
  `lkj_corr_cholesky(4)`

The default prior for `phi` (the overdispersion parameter for the
negative-binomial distribution) is scaled automatically based on the
variance of the data.

We recommend that users assess the suitability of these default priors
by fitting the model with `prior_only = TRUE` and then plotting prior
predictive checks for all variables using the
[`coev_plot_predictive_check()`](https://docs.ropensci.org/coevolve/reference/coev_plot_predictive_check.md)
function.

**Handling missing data**

In order to retain the most information, the `coev_fit()` function
automatically imputes all missing values. To turn off this behaviour and
exclude taxa with missing data, set `complete_cases = FALSE`.

**Dealing with repeated observations**

If taxa appear in the dataset multiple times (i.e., there are repeated
observations), the model will automatically estimate residual standard
deviations and residual correlations that capture the within-taxa
variation that is not due to the coevolutionary process. To turn off
this behaviour, set `estimate_residual = FALSE`.

We do not recommend modelling residual standard deviations in cases
where repeated observations are present for some traits but not others,
as there will be no within-species variance for traits without repeated
observations.

Note that models with repeated observations for mixtures of continuous
and non-continuous traits are currently considered experimental and
should be treated with caution.

**Incorporating measurement error**

If any normally-distributed coevolving variables are measured with
error, the user can pass standard errors for one or more of these
variables into the model with the `measurement_error` argument.

**Controlling for spatial location**

If users declare longitude and latitude values with the `lon_lat`
argument, the model will include several Gaussian processes (one per
coevolving variable) over spatial locations.

## References

Ringen, E., Martin, J. S., & Jaeggi, A. (2021). Novel phylogenetic
methods reveal that resource-use intensification drives the evolution of
"complex" societies. *EcoEvoRXiv*. `doi:10.32942/osf.io/wfp95`

Sheehan, O., Watts, J., Gray, R. D., Bulbulia, J., Claessens, S.,
Ringen, E. J., & Atkinson, Q. D. (2023). Coevolution of religious and
political authority in Austronesian societies. *Nature Human Behaviour*,
*7*(1), 38-45. `10.1038/s41562-022-01471-y`

## See also

[`coev_make_stancode`](https://docs.ropensci.org/coevolve/reference/coev_make_stancode.md),
[`coev_make_standata`](https://docs.ropensci.org/coevolve/reference/coev_make_standata.md)

## Author

Scott Claessens <scott.claessens@gmail.com>, Erik Ringen
<erikjacob.ringen@uzh.ch>

## Examples

``` r
if (FALSE) { # \dontrun{
# fit dynamic coevolutionary model
fit <- coev_fit(
  data = authority$data,
  variables = list(
    political_authority = "ordered_logistic",
    religious_authority = "ordered_logistic"
  ),
  id = "language",
  tree = authority$phylogeny,
  # additional arguments for cmdstanr::sample()
  chains = 4,
  parallel_chains = 4,
  seed = 1
  )

# print model summary
summary(fit)
} # }
```
