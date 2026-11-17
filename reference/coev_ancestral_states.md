# Extract ancestral state estimates from a fitted coevolutionary model

Extracts posterior estimates of trait values at internal nodes of the
phylogenetic tree from a fitted `coevfit` object. By default, estimates
are returned on the latent (eta) scale; use `scale = "response"` to
apply the inverse link for each variable's response distribution. The
latent `eta` parameters in the model correspond directly to nodes in the
phylogeny, making ancestral state reconstruction a natural byproduct of
model fitting.

## Usage

``` r
coev_ancestral_states(
  object,
  variables = NULL,
  nodes = "internal",
  tree_id = NULL,
  scale = "latent",
  summary = TRUE,
  prob = 0.95
)
```

## Arguments

- object:

  An object of class `coevfit`

- variables:

  (optional) A character vector of variable names to include. If `NULL`
  (default), all variables from the fitted model are included.

- nodes:

  Which nodes to include. Either `"internal"` (default) for internal
  nodes only, `"all"` for both tips and internal nodes, or an integer
  vector of specific node IDs (using ape's node numbering: tips =
  1:N_tips, internal = (N_tips+1):total).

- tree_id:

  (optional) An integer indicating which tree to use when the model was
  fit with a `multiPhylo` object. If `NULL` (default), uses tree 1.
  (Integration over trees with topological uncertainty is not yet
  implemented.)

- scale:

  Character string, either `"latent"` (default) to return values on the
  latent eta scale, or `"response"` to apply the inverse link function
  for each variable's response distribution.

- summary:

  Logical. If `TRUE` (default), returns a long-format data frame with
  posterior point estimates and credible intervals. If `FALSE`, returns
  the raw posterior draws.

- prob:

  A single numeric value between 0 and 1 indicating the desired
  probability mass for the credible interval. Default is 0.95.

## Value

If `summary = TRUE`, a long-format data frame (tibble) with columns:

- node:

  Integer node ID in the reference tree

- variable:

  Character variable name

- category:

  Only present when `scale = "response"`. For ordinal variables, the
  category label (`"cat_1"`, `"cat_2"`, ...). `NA` for non-ordinal
  variables.

- estimate:

  Posterior median

- lower:

  Lower bound of the credible interval

- upper:

  Upper bound of the credible interval

The data frame has attributes `ref_tree` (the phylo object used),
`prob`, and `scale`.

If `summary = FALSE`, a list with elements:

- draws:

  A 3D array (draws x nodes x variables)

- ref_tree:

  The reference phylo object

- node_ids:

  Integer vector of node IDs (matching dim 2 of draws)

- variable_names:

  Character vector of variable names

## See also

[`coev_fit`](https://docs.ropensci.org/coevolve/reference/coev_fit.md),
[`coev_plot_trait_values`](https://docs.ropensci.org/coevolve/reference/coev_plot_trait_values.md)

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
  chains = 4,
  parallel_chains = 4,
  seed = 1
)

# extract ancestral states (latent scale, internal nodes, summary)
asr <- coev_ancestral_states(fit)

# extract all nodes including tips
asr_all <- coev_ancestral_states(fit, nodes = "all")

# extract on response scale
asr_resp <- coev_ancestral_states(fit, scale = "response")

# get raw posterior draws
asr_draws <- coev_ancestral_states(fit, summary = FALSE)
} # }
```
