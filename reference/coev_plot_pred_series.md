# Plot a predicted coevolutionary time series from a fitted `coevfit` object

This function plots a predicted coevolutionary time series using the
estimated parameters from a fitted `coevfit` model. By default, the plot
uses the posterior ancestral states estimated by the model as the
starting values, but users can also set their own starting values for
traits. Plots can be generated with or without stochastic drift. For
more details on the underlying predictive function, see
[`help(coev_pred_series)`](https://docs.ropensci.org/coevolve/reference/coev_pred_series.md).

## Usage

``` r
coev_plot_pred_series(object, prob = 0.95, ...)
```

## Arguments

- object:

  An object of class `coevfit`

- prob:

  A value between 0 and 1 indicating the desired probability to be
  covered by the uncertainty intervals. The default is 0.95.

- ...:

  Additional arguments passed to
  [`coev_pred_series`](https://docs.ropensci.org/coevolve/reference/coev_pred_series.md)

## Value

A ggplot object

## See also

[`coev_pred_series`](https://docs.ropensci.org/coevolve/reference/coev_pred_series.md)

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

# simulated trait co-evolution
coev_plot_pred_series(
  object = fit,
  stochastic = TRUE
  )

# expected trait co-evolution, no drift
coev_plot_pred_series(
  object = fit,
  stochastic = FALSE,
  eta_anc = list(political_authority = -2, religious_authority = 1.5)
  )
} # }
```
