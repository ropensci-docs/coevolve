# Extract samples (draws) from a fitted `coevfit` object

Extract samples (draws) from a fitted `coevfit` object

## Usage

``` r
extract_samples(object)
```

## Arguments

- object:

  An object of class `coevfit`

## Value

Samples in 'rethinking' style list format

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

# extract samples from fitted model
extract_samples(fit)
} # }
```
