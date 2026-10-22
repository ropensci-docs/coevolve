# Print a summary of a fitted model represented by a `coevfit` object

Print a summary of a fitted model represented by a `coevfit` object

## Usage

``` r
# S3 method for class 'coevfit'
print(x, digits = 2, ...)
```

## Arguments

- x:

  An object of class `coevfit`

- digits:

  The number of significant digits for printing out the summary;
  defaults to 2

- ...:

  Additional arguments that would be passed to method `summary` of
  `coevfit`.

## Value

A printed summary of a `coevfit` model object

## See also

[`summary.coevfit`](https://docs.ropensci.org/coevolve/reference/summary.coevfit.md)

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
print(fit)
} # }
```
