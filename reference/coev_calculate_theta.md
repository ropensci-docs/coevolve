# Calculate equilibrium trait values (theta) for a fitted `coevfit` object

Calculate equilibrium trait values \\\theta\\ for one or more traits
given a set of intervention values for other traits from a fitted
`coevfit` object.

## Usage

``` r
coev_calculate_theta(object, intervention_values = NULL)
```

## Arguments

- object:

  An object of class `coevfit`

- intervention_values:

  Either `NULL` (the default) or a named list of variables and
  associated intervention values for calculating equilibrium trait
  values. If `NULL`, calculates the equilibrium states when all
  parameters are free to vary. Otherwise, all coevolving variables must
  be declared separately in the named list without repetition. If the
  intervention value for a particular variable is set to NA, this
  variable is treated as a free variable. If the intervention value for
  a particular variable is specified, the variable is held constant at
  this trait value in the calculation. At least one variable must be
  declared as a free variable and at least one variable must be held
  constant (e.g., `list(var1 = NA, var2 = 0)`).

## Value

Posterior samples in matrix format

## Details

The equilibrium trait values for freely evolving traits
\\\mathbf{\eta}\\ are calculated using the following formula:

\$\$\mathbf{\theta} = -\mathbf{A}^{-1}\mathbf{b}\$\$

If we hold some variables constant at some value(s) (denoted
\\\eta\_{h}\\) and let others evolve freely (denoted \\\eta\_{f}\\), we
can partition the parameters as follows:

- \\\mathbf{A}\_{ff}\\: selection coefficients to/from the free
  variables

- \\\mathbf{A}\_{fh}\\: selection coefficients to the free variables
  from the held variables

- \\\mathbf{b}\_{f}\\: continuous time intercepts for the free variables

We can then calculate the equilibrium values for the free variables:

\$\$\pmb{\theta_f} = -\mathbf{A}\_{ff}^{-1} \left( \mathbf{A}\_{fh}
\mathbf{\eta}\_h + \mathbf{b}\_f \right)\$\$

With the overall equilibrium vector being a mix of the free equilibria
and the held values:

\$\$\pmb{\theta \| \mathbf{\eta}\_h} = \begin{bmatrix} \pmb{\theta}\_f
\\\mathbf{\eta}\_h\end{bmatrix}\$\$

## References

Ringen, E., Martin, J. S., & Jaeggi, A. (2021). Novel phylogenetic
methods reveal that resource-use intensification drives the evolution of
"complex" societies. *EcoEvoRXiv*. `doi:10.32942/osf.io/wfp95`

Sheehan, O., Watts, J., Gray, R. D., Bulbulia, J., Claessens, S.,
Ringen, E. J., & Atkinson, Q. D. (2023). Coevolution of religious and
political authority in Austronesian societies. *Nature Human Behaviour*,
*7*(1), 38-45. `10.1038/s41562-022-01471-y`

## See also

[`coev_calculate_delta_theta`](https://docs.ropensci.org/coevolve/reference/coev_calculate_delta_theta.md),
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

# calculate theta with no interventions
coev_calculate_theta(
  object = fit
 )

# calculate theta given intervention values
coev_calculate_theta(
  object = fit,
  intervention_values = list(
    political_authority = NA,
    religious_authority = 0
    )
  )
} # }
```
