# Plot flowfield of expected evolutionary change from a fitted `coevfit` object

Plot a flowfield of the expected evolutionary dynamics from a fitted
`coevfit` object, with help from the phaseR package.

## Usage

``` r
coev_plot_flowfield(
  object,
  var1,
  var2,
  nullclines = FALSE,
  limits = c(-2.5, 2.5)
)
```

## Arguments

- object:

  An object of class `coevfit`

- var1:

  A character string of length one equal to one of the coevolving
  variables in the model (strictly case-sensitive)

- var2:

  A character string of length one equal to one of the coevolving
  variables in the model (strictly case-sensitive)

- nullclines:

  Logical (defaults to FALSE); whether to show coloured nullclines to
  indicate where each variable is at equilibrium, depending on the state
  of the other

- limits:

  A numeric vector of length 2 (defaults to `c(-2.5, 2.5)`); specifying
  the lower limit and the upper limit of the x and y axes.

## Value

A flowfield plot drawn directly to the device

## Details

The flowfield visualises the change in two coevolving traits depending
on their current states, with the direction and strength of change
depicting with the direction and size of arrows. If nullclines are
included, they represent the parameter combinations where each trait is
at equilibrium, depending on the state of the other trait.

If three or more traits were included in the model, other traits are
held at their median values during the computations. Note that flowfield
plots can potentially produce misleading pictures of coevolutionary
dynamics when other traits are held constant in models with three or
more traits.

If the plot does not look right, the user might try zooming out from the
default parameter space by setting wider limits. For some variables
(e.g., continuous and count variables), the default limits may not be
suitable.

## References

Ringen, E., Martin, J. S., & Jaeggi, A. (2021). Novel phylogenetic
methods reveal that resource-use intensification drives the evolution of
"complex" societies. *EcoEvoRXiv*. `doi:10.32942/osf.io/wfp95`

Sheehan, O., Watts, J., Gray, R. D., Bulbulia, J., Claessens, S.,
Ringen, E. J., & Atkinson, Q. D. (2023). Coevolution of religious and
political authority in Austronesian societies. *Nature Human Behaviour*,
*7*(1), 38-45. `10.1038/s41562-022-01471-y`

## See also

[`coev_plot_selection_gradient`](https://docs.ropensci.org/coevolve/reference/coev_plot_selection_gradient.md)

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

# plot flow field
coev_plot_flowfield(
  object = fit,
  var1 = "political_authority",
  var2 = "religious_authority"
  )
} # }
```
