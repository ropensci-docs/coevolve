# Make Stan data for dynamic coevolutionary model

Make the data list for the Stan model. The function takes a dataset,
phylogeny, and a set of variables and performs the necessary
computations (e.g., cutting up the tree into segments, computing branch
lengths, determining parent and child nodes) for the Stan model.

## Usage

``` r
coev_make_standata(
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
  dist_mat = deprecated()
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
  Gaussian Processes over locations (case-sensitive). Currently
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
  model uses default priors (see
  [`help(coev_fit)`](https://docs.ropensci.org/coevolve/reference/coev_fit.md)).
  Alternatively, the user can specify a named list of priors. The list
  must contain non-duplicated entries for any of the following
  parameters: the autoregressive effects (`A_diag`), the cross effects
  (`A_offdiag`), the Cholesky factor for the drift matrix (`L_R`), the
  drift std. dev. parameters (`Q_sigma`), the continuous time intercepts
  (`b`), the ancestral states for the traits (`eta_anc`), the cutpoints
  for ordinal variables (`c`), the overdispersion parameters for
  negative binomial variables (`phi`), the shape parameters for gamma
  variables (`shape`), the sigma parameters for Gaussian Processes over
  locations (`sigma_dist`), the rho parameters for Gaussian Processes
  over locations (`rho_dist`), the residual standard deviations when
  there are repeated observations (`sigma_residual`), and the Cholesky
  factor for the residual correlations when there are repeated
  observations (`L_residual`). These must be entered with valid prior
  strings, e.g. `list(A_offdiag = "normal(0, 2)")`.

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

- dist_mat:

  **\[deprecated\]** Use the `lon_lat` argument instead.

## Value

A list containing the data for fitting the dynamic coevolutionary model
in Stan

## Details

For further details, see
[`help(coev_fit)`](https://docs.ropensci.org/coevolve/reference/coev_fit.md)

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
[`coev_fit`](https://docs.ropensci.org/coevolve/reference/coev_fit.md)

## Author

Scott Claessens <scott.claessens@gmail.com>, Erik Ringen
<erikjacob.ringen@uzh.ch>

## Examples

``` r
# make stan data
stan_data <- coev_make_standata(
  data = authority$data,
  variables = list(
    political_authority = "ordered_logistic",
    religious_authority = "ordered_logistic"
  ),
  id = "language",
  tree = authority$phylogeny
)

# include effects matrix
effects_mat <-
  matrix(
    c(TRUE, TRUE,
      FALSE, TRUE),
    nrow = 2,
    dimnames = list(
      c("political_authority", "religious_authority"),
      c("political_authority", "religious_authority")
    )
  )
stan_data <- coev_make_standata(
  data = authority$data,
  variables = list(
    political_authority = "ordered_logistic",
    religious_authority = "ordered_logistic"
  ),
  id = "language",
  tree = authority$phylogeny,
  effects_mat = effects_mat
)

# include longitude/latitude values
stan_data <- coev_make_standata(
  data = authority$data,
  variables = list(
    political_authority = "ordered_logistic",
    religious_authority = "ordered_logistic"
  ),
  id = "language",
  tree = authority$phylogeny,
  lon_lat = authority$coordinates
)

# include measurement error
d <- authority$data
d$x <- rnorm(nrow(d))
d$y <- rnorm(nrow(d))
d$x_std_err <- rexp(nrow(d))
d$y_std_err <- rexp(nrow(d))
stan_data <- coev_make_standata(
  data = d,
  variables = list(
    x = "normal",
    y = "normal"
  ),
  id = "language",
  tree = authority$phylogeny,
  measurement_error = list(
    x = "x_std_err",
    y = "y_std_err"
  )
)

# set manual priors
stan_data <- coev_make_standata(
  data = authority$data,
  variables = list(
    political_authority = "ordered_logistic",
    religious_authority = "ordered_logistic"
  ),
  id = "language",
  tree = authority$phylogeny,
  prior = list(A_offdiag = "normal(0, 2)")
)
```
