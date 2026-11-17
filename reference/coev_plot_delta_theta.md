# Plot delta theta values from a fitted `coevfit` object

Plot delta theta values for all trait pairs from a fitted `coevfit`
object. This plot can be used to visually assess contingencies and
directionality between different variables in the coevolutionary
process.

## Usage

``` r
coev_plot_delta_theta(
  object,
  variables = NULL,
  prob = 0.66,
  prob_outer = 0.95,
  limits = NULL
)
```

## Arguments

- object:

  An object of class `coevfit`

- variables:

  If NULL (default), the plot includes all coevolving variables from the
  model. Otherwise, a character vector of length \>= 2 declaring the
  variables to be included in the plot.

- prob:

  Probability mass to include in the inner interval. Default is 0.66
  (66% interval).

- prob_outer:

  Probability mass to include in the outer interval. Default is 0.95
  (95% interval).

- limits:

  If NULL (default), limits are scaled automatically to include all
  posterior samples. Otherwise, a numeric vector of length 2 specifying
  the lower and upper limits for the x-axis.

## Value

A `ggplot` object

## Details

This function repeatedly uses the
[`coev_calculate_delta_theta`](https://docs.ropensci.org/coevolve/reference/coev_calculate_delta_theta.md)
function under the hood to generate a pairs plot of \\\Delta\theta\\ for
all variables in the model. For more details on the definition and
calculation of \\\Delta\theta\\, see
[`help(coev_calculate_delta_theta)`](https://docs.ropensci.org/coevolve/reference/coev_calculate_delta_theta.md).
Note that often the posterior distribution for \\\Delta\theta\\ has long
tails, meaning that the distribution for different traits can be
difficult to visualise in a single pairs plot. If this plot does not
produce satisfactory visualisations, the user should either specify
narrower limits for the x-axis or use the
[`coev_calculate_delta_theta`](https://docs.ropensci.org/coevolve/reference/coev_calculate_delta_theta.md)
function to create plots manually.

## References

Ringen, E., Martin, J. S., & Jaeggi, A. (2021). Novel phylogenetic
methods reveal that resource-use intensification drives the evolution of
"complex" societies. *EcoEvoRXiv*. `doi:10.32942/osf.io/wfp95`

Sheehan, O., Watts, J., Gray, R. D., Bulbulia, J., Claessens, S.,
Ringen, E. J., & Atkinson, Q. D. (2023). Coevolution of religious and
political authority in Austronesian societies. *Nature Human Behaviour*,
*7*(1), 38-45. `10.1038/s41562-022-01471-y`

## See also

[`coev_calculate_delta_theta`](https://docs.ropensci.org/coevolve/reference/coev_calculate_delta_theta.md),
[`coev_calculate_theta`](https://docs.ropensci.org/coevolve/reference/coev_calculate_theta.md)

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

# plot delta theta values for all effects
coev_plot_delta_theta(fit)
} # }
```
