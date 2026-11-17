# Brain weight, body weight, diet, and sociality in primates

This dataset and associated phylogeny describes brain weights, body
weights, dietary categories, social systems, mating systems, and group
sizes for 143 primate species. These data were originally compiled in
DeCasien et al. (2017). The phylogeny is a 50% majority rule consensus
tree from the 10k Trees website.

## Usage

``` r
primates
```

## Format

A list containing a dataset and an associated phylogeny. The phylogeny
is a pruned consensus tree for 143 species of primate (excluding
humans). The dataset is a data frame with 143 observations and 11
variables:

- species:

  The primate species

- clade:

  The clade of the primate species

- brain_weight:

  Average brain weight, in grams

- body_weight:

  Average body weight, in grams

- diet:

  A factor with four levels indicating whether the species' dietary
  category is folivore, frugivore, frugivore/folivore, or omnivore

- percent_fruit:

  Percentage of fruit in diet

- social_system:

  A factor with four levels indicating whether the species' social
  system is solitary, pair-living, harem polygyny, or polygynandry

- mating_system:

  A factor with five levels indicating whether the species' mating
  system is spatial polygyny, monogamy, polyandry, harem polygyny, or
  polygynandry

- group_size:

  Average group size

## Source

DeCasien, A. R., Williams, S. A., & Higham, J. P. (2017). Primate brain
size is predicted by diet but not sociality. *Nature Ecology &
Evolution*, *1*(5), 0112.

Arnold, C., Matthews, L. J., & Nunn, C. L. (2010). The 10kTrees website:
a new online resource for primate phylogeny. *Evolutionary
Anthropology*, *19*(3), 114-118.

https://10ktrees.nunn-lab.org/Primates/index.html

## Examples

``` r
if (FALSE) { # \dontrun{
# fit model to primates data
m <-
  coev_fit(
    data = primates$data,
    variables = list(
      brain_weight = "lognormal",
      body_weight = "lognormal"
    ),
    id = "species",
    tree = primates$phylogeny,
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
