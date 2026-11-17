# Summary statistics for jax_fit object

Internal S3 method. Computes summary statistics from a jax_fit object,
compatible with cmdstanr's summary() method. This method mimics
cmdstanr's interface where summary() is called with named arguments
where names are column names and values are function names or formulas.

## Usage

``` r
# S3 method for class 'jax_fit'
summary(object, variables = NULL, ...)
```

## Arguments

- object:

  A jax_fit object.

- variables:

  Character vector of variable names to summarize.

- ...:

  Named summary spec arguments (cmdstanr style).

## Value

A data.frame with summary statistics.

## Examples

``` r
TRUE
#> [1] TRUE
```
