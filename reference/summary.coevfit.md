# Create a summary of a fitted model represented by a `coevfit` object

Create a summary of a fitted model represented by a `coevfit` object

## Usage

``` r
# S3 method for class 'coevfit'
summary(object, prob = 0.95, robust = FALSE, ...)
```

## Arguments

- object:

  An object of class `coevfit`.

- prob:

  A value between 0 and 1 indicating the desired probability to be
  covered by the uncertainty intervals. The default is 0.95.

- robust:

  If `FALSE` (the default) the mean is used as the measure of central
  tendency and the standard deviation as the measure of variability. If
  `TRUE`, the median and the median absolute deviation are applied
  instead.

- ...:

  Other potential arguments

## Value

List of class coevsummary

## Details

The convergence diagnostics `rhat`, `ess_bulk`, and `ess_tail` are
described in detail in Vehtari et al. (2020).

## References

Aki Vehtari, Andrew Gelman, Daniel Simpson, Bob Carpenter, and
Paul-Christian Bürkner (2020). Rank-normalization, folding, and
localization: An improved R-hat for assessing convergence of MCMC.
*Bayesian Analysis*. 1–28. doi:10.1214/20-BA1221

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

# print summary of model
summary(fit)
} # }
```
