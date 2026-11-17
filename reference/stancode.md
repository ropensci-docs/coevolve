# Expose the Stan code of a fitted model represented by a `coevfit` object

Expose the Stan code of a fitted model represented by a `coevfit` object

## Usage

``` r
stancode(object)
```

## Arguments

- object:

  An object of class `coevfit`.

## Value

Printed Stan code

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

# expose stan code
stancode(fit)
} # }
```
