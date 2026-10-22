# Create a matrix of output plots from a `coevfit` object

A [`pairs`](https://rdrr.io/r/graphics/pairs.html) method that is
customized for MCMC output.

## Usage

``` r
# S3 method for class 'coevfit'
pairs(x, parameters = NULL, ...)
```

## Arguments

- x:

  An object of class `coevfit`

- parameters:

  If NULL (default), the function returns plots for the main parameters
  from the model (the selection matrix "A", the drift matrix "Q", and
  the continuous time intercepts "b"). Otherwise, a character vector
  declaring the parameters to plot (strictly case-sensitive).

- ...:

  Further arguments to be passed to
  [`mcmc_pairs`](https://mc-stan.org/bayesplot/reference/MCMC-scatterplots.html).

## Value

A matrix of output plots

## Details

For a detailed description see
[`mcmc_pairs`](https://mc-stan.org/bayesplot/reference/MCMC-scatterplots.html).

## See also

[`plot.coevfit`](https://docs.ropensci.org/coevolve/reference/plot.coevfit.md)

## Author

Scott Claessens <scott.claessens@gmail.com>

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

# print pairs plot
pairs(fit)
} # }
```
