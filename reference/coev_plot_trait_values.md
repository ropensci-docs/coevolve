# Plot estimated trait values from a fitted `coevfit` object

Produce a pairs plot of the estimated trait values for taxa from a
fitted `coevfit` object. The plot includes scatterplot(s) of median
estimated trait values, heatmap(s) of median estimated trait values, and
density plots of the marginal distributions for each variable with
associated posterior uncertainty.

## Usage

``` r
coev_plot_trait_values(
  object,
  variables = NULL,
  ndraws = 50,
  tree_id = NULL,
  xlim = NULL,
  ylim = NULL
)
```

## Arguments

- object:

  An object of class `coevfit`

- variables:

  If NULL (default), the function returns a pairs plot including all
  coevolving variables from the model. Otherwise, a character vector
  declaring the variables to be included.

- ndraws:

  An integer indicating the number of draws to return in the density
  plots on the diagonal. The default and maximum number of draws is the
  size of the posterior sample.

- tree_id:

  An integer indicating the tree ID to use when making posterior
  predictions. Set to `NULL` by default, which will use draws from every
  tree, integrating phylogenetic uncertainty.

- xlim:

  Limits for the x-axis. If `NULL` (default), limits are set to the
  minimum and maximum estimated trait values.

- ylim:

  Limits for the y-axis. If `NULL` (default), limits are set to the
  minimum and maximum estimated trait values.

## Value

A patchwork of `ggplot` objects

## References

Ringen, E., Martin, J. S., & Jaeggi, A. (2021). Novel phylogenetic
methods reveal that resource-use intensification drives the evolution of
"complex" societies. *EcoEvoRXiv*. `doi:10.32942/osf.io/wfp95`

Sheehan, O., Watts, J., Gray, R. D., Bulbulia, J., Claessens, S.,
Ringen, E. J., & Atkinson, Q. D. (2023). Coevolution of religious and
political authority in Austronesian societies. *Nature Human Behaviour*,
*7*(1), 38-45. `10.1038/s41562-022-01471-y`

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

# pairs plot of trait values
coev_plot_trait_values(fit)
} # }
```
