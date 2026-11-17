# Extract draws from jax_fit object

Internal S3 method. Extracts draws from a jax_fit object, compatible
with cmdstanr's draws() method.

## Usage

``` r
# S3 method for class 'jax_fit'
draws(x, variables = NULL, ...)
```

## Arguments

- x:

  A jax_fit object.

- variables:

  Character vector of variable names to extract.

- ...:

  Additional arguments (ignored).

## Value

A draws_array object.

## Examples

``` r
TRUE
#> [1] TRUE
```
