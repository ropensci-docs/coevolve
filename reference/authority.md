# Political and religious authority in Austronesian societies

This dataset and associated phylogeny describes the states of political
and religious authority in 97 Austronesian societies. These data were
originally compiled in Sheehan et al. (2023). Political authority is
defined as a right to manage interactions between living human beings,
whereas religious authority is defined as a right to manage interactions
between living human beings and supernatural agents or powers. Authority
is coded as being absent, sublocal (smaller than the local community),
local (coextensive with the local community), or supralocal (consisting
of more than one local community).

## Usage

``` r
authority
```

## Format

A list containing a dataset, an associated language phylogeny, and
associated geographic coordinates. The phylogeny is a pruned maximum
clade credibility tree for 97 Austronesian languages. The coordinates
are in a data frame, with longitude and latitude values measured in
decimal degrees. The dataset is a data frame with 97 observations and
three variables:

- language:

  The name of the Austronesian language linked to each society

- political_authority:

  An ordered factor indicating whether political authority is absent,
  sublocal, local, or supralocal in each society

- religious_authority:

  An ordered factor indicating whether religious authority is absent,
  sublocal, local, or supralocal in each society

## Source

Sheehan, O., Watts, J., Gray, R. D., Bulbulia, J., Claessens, S.,
Ringen, E. J., & Atkinson, Q. D. (2023). Coevolution of religious and
political authority in Austronesian societies. *Nature Human Behaviour*,
*7*(1), 38-45.

## Examples

``` r
if (FALSE) { # \dontrun{
# fit model to authority data
m <-
  coev_fit(
    data = authority$data,
    variables = list(
      political_authority = "ordered_logistic",
      religious_authority = "ordered_logistic"
    ),
    id = "language",
    tree = authority$phylogeny,
    # arguments for cmdstanr::sample()
    parallel_chains = 4,
    seed = 1,
    # set prior manually
    prior = list(A_offdiag = "normal(0, 2)")
  )
# print model summary
summary(m)
# plot delta theta
coev_plot_delta_theta(m)
} # }
```
