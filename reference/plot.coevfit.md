# Trace and density plots for `coevfit` objects

Trace and density plots for `coevfit` objects

## Usage

``` r
# S3 method for class 'coevfit'
plot(
  x,
  parameters = NULL,
  combo = c("dens", "trace"),
  npars = 5,
  plot = TRUE,
  ...
)
```

## Arguments

- x:

  An object of class `coevfit`.

- parameters:

  If NULL (default), the function returns plots for the main parameters
  from the model (the selection matrix "A", the drift matrix "Q", and
  the continuous time intercepts "b"). Otherwise, a character vector
  declaring the parameters to plot (strictly case-sensitive).

- combo:

  A character vector with at least two elements. Each element of `combo`
  corresponds to a column in the resulting plot and should be the name
  of one of the available
  [`MCMC`](https://mc-stan.org/bayesplot/reference/MCMC-overview.html)
  functions (omitting the `mcmc_` prefix).

- npars:

  Number of parameters displayed per page (default = 5).

- plot:

  Logical. If TRUE (default), plots are plotted in the active graphic
  device. If FALSE, plots are not plotted.

- ...:

  Not used.

## Value

A list of `ggplot` objects

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

# print trace and density plots
plot(fit)
} # }
```
