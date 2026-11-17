# Calculate delta theta from a fitted `coevfit` object

Calculate \\\Delta\theta\\ from a fitted `coevfit` object.
\\\Delta\theta\\ is defined as the difference in the equilibrium value
for a "response" trait that results from a standardised increase in the
value of another "predictor" trait. This value can be used to assess
contingencies and directionality between variables in the coevolutionary
process.

## Usage

``` r
coev_calculate_delta_theta(object, response, predictor)
```

## Arguments

- object:

  An object of class `coevfit`

- response:

  A character string of length one equal to one of the coevolving
  variables in the model (strictly case-sensitive)

- predictor:

  A character string of length one equal to one of the coevolving
  variables in the model (strictly case-sensitive)

## Value

Posterior samples in the draws_array format

## Details

This function calculates \\\Delta\theta\\, which is defined as the
difference in the equilibrium value for a "response" trait that results
from a standardised increase in the value of another "predictor" trait.
The function first calculates the equilibrium trait value for the
response trait when the predictor trait is held at its empirical median
value (the
[`coev_calculate_theta`](https://docs.ropensci.org/coevolve/reference/coev_calculate_theta.md)
function is used for this purpose, see
[`help(coev_calculate_theta)`](https://docs.ropensci.org/coevolve/reference/coev_calculate_theta.md)
for further details). The function then calculates the equilibrium trait
value for the response variable after increasing the predictor trait
from its median by one median absolute deviation. The function then
returns the posterior difference between these values. The resulting
\\\Delta\theta\\ samples can be used to infer whether increases in one
trait have a positive or negative selective effect on another trait in
the model.

## References

Ringen, E., Martin, J. S., & Jaeggi, A. (2021). Novel phylogenetic
methods reveal that resource-use intensification drives the evolution of
"complex" societies. *EcoEvoRXiv*. `doi:10.32942/osf.io/wfp95`

Sheehan, O., Watts, J., Gray, R. D., Bulbulia, J., Claessens, S.,
Ringen, E. J., & Atkinson, Q. D. (2023). Coevolution of religious and
political authority in Austronesian societies. *Nature Human Behaviour*,
*7*(1), 38-45. `10.1038/s41562-022-01471-y`

## See also

`coev_calculate_delta_theta`,
[`coev_plot_delta_theta`](https://docs.ropensci.org/coevolve/reference/coev_plot_delta_theta.md)

## Author

Scott Claessens <scott.claessens@gmail.com>, Erik Ringen
<erikjacob.ringen@uzh.ch>

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

# calculate delta theta
coev_calculate_delta_theta(
  object = fit,
  response = "political_authority",
  predictor = "religious_authority"
  )
} # }
```
