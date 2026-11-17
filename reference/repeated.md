# Example dataset with repeated observations

This example dataset and associated phylogeny are used as an example of
repeated observations in phylogenetic modelling. The data are adapted
from de Villemeruil & Nakagawa (2014) to include only the first twenty
species.

## Usage

``` r
repeated
```

## Format

A list containing a dataset and an associated phylogeny. The dataset is
a data frame with 100 observations and 3 variables:

- species:

  The name of the species

- x:

  An example continuous variable

- y:

  An example continuous variable

## Source

de Villemeruil P. & Nakagawa, S. (2014). General quantitative genetic
methods for comparative biology. In L. Garamszegi (Ed.), *Modern
phylogenetic comparative methods and their application in evolutionary
biology: concepts and practice* (pp. 287-303). Springer, New York.

## Examples

``` r
if (FALSE) { # \dontrun{
# fit model to repeated data
m <-
  coev_fit(
    data = repeated$data,
    variables = list(
      x = "normal",
      y = "normal"
    ),
    id = "species",
    tree = repeated$phylogeny,
    # arguments to cmdstanr::sample()
    parallel_chains = 4,
    seed = 1
  )
# print model summary
summary(m)
# plot delta theta
coev_plot_delta_theta(m)
} # }
```
