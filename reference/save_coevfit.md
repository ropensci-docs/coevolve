# Save a fitted `coevfit` object to a file

A wrapper around [`saveRDS`](https://rdrr.io/r/base/readRDS.html) that
saves a fitted `coevfit` model object while ensuring that all posterior
draws and diagnostics are correctly saved by cmdstanr.

## Usage

``` r
save_coevfit(object, file, ...)
```

## Arguments

- object:

  An object of class `coevfit`

- file:

  A string declaring the path where the file should be saved

- ...:

  Other arguments to pass to
  [`saveRDS`](https://rdrr.io/r/base/readRDS.html) besides `object` and
  `file`

## Value

An .RDS file containing the fitted `coevfit` model object

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

# save model to .rds file
save_coevfit(fit, file = "model.rds")
} # }
```
