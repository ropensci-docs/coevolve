# Plot predictive check from a fitted `coevfit` object

Plot posterior (or prior) predictive checks for variables from a fitted
`coevfit` object, with help from the bayesplot package.

## Usage

``` r
coev_plot_predictive_check(
  object,
  variables = NULL,
  ndraws = NULL,
  tree_id = NULL
)
```

## Arguments

- object:

  An object of class `coevfit`

- variables:

  If NULL (default), the function returns a list of plots for all
  coevolving variables from the model. Otherwise, a character vector
  declaring the variables to be included in the list of plots.

- ndraws:

  An integer indicating the number of draws to return. The default and
  maximum number of draws is the size of the posterior sample.

- tree_id:

  An integer indicating the tree ID to use when making posterior
  predictions. Set to `NULL` by default, which will use draws from every
  tree, integrating phylogenetic uncertainty.

## Value

A list of `ggplot` objects

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

# plot predictive checks for all variables
coev_plot_predictive_check(fit)
} # }
```
