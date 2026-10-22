# Package index

## Datasets

- [`authority`](https://docs.ropensci.org/coevolve/reference/authority.md)
  : Political and religious authority in Austronesian societies
- [`primates`](https://docs.ropensci.org/coevolve/reference/primates.md)
  : Brain weight, body weight, diet, and sociality in primates
- [`repeated`](https://docs.ropensci.org/coevolve/reference/repeated.md)
  : Example dataset with repeated observations

## Fit

- [`coev_fit()`](https://docs.ropensci.org/coevolve/reference/coev_fit.md)
  : Fit Bayesian dynamic coevolutionary model in Stan
- [`coev_make_stancode()`](https://docs.ropensci.org/coevolve/reference/coev_make_stancode.md)
  : Make Stan code for dynamic coevolutionary model
- [`coev_make_standata()`](https://docs.ropensci.org/coevolve/reference/coev_make_standata.md)
  : Make Stan data for dynamic coevolutionary model

## Summarise

- [`print(`*`<coevfit>`*`)`](https://docs.ropensci.org/coevolve/reference/print.coevfit.md)
  :

  Print a summary of a fitted model represented by a `coevfit` object

- [`summary(`*`<coevfit>`*`)`](https://docs.ropensci.org/coevolve/reference/summary.coevfit.md)
  :

  Create a summary of a fitted model represented by a `coevfit` object

## Plot

- [`plot(`*`<coevfit>`*`)`](https://docs.ropensci.org/coevolve/reference/plot.coevfit.md)
  :

  Trace and density plots for `coevfit` objects

- [`pairs(`*`<coevfit>`*`)`](https://docs.ropensci.org/coevolve/reference/pairs.coevfit.md)
  :

  Create a matrix of output plots from a `coevfit` object

- [`coev_plot_delta_theta()`](https://docs.ropensci.org/coevolve/reference/coev_plot_delta_theta.md)
  :

  Plot delta theta values from a fitted `coevfit` object

- [`coev_plot_flowfield()`](https://docs.ropensci.org/coevolve/reference/coev_plot_flowfield.md)
  :

  Plot flowfield of expected evolutionary change from a fitted `coevfit`
  object

- [`coev_plot_pred_series()`](https://docs.ropensci.org/coevolve/reference/coev_plot_pred_series.md)
  :

  Plot a predicted coevolutionary time series from a fitted `coevfit`
  object

- [`coev_plot_predictive_check()`](https://docs.ropensci.org/coevolve/reference/coev_plot_predictive_check.md)
  :

  Plot predictive check from a fitted `coevfit` object

- [`coev_plot_selection_gradient()`](https://docs.ropensci.org/coevolve/reference/coev_plot_selection_gradient.md)
  :

  Plot selection gradient heatmap from a fitted `coevfit` object

- [`coev_plot_trait_values()`](https://docs.ropensci.org/coevolve/reference/coev_plot_trait_values.md)
  :

  Plot estimated trait values from a fitted `coevfit` object

## Extract

- [`extract_samples()`](https://docs.ropensci.org/coevolve/reference/extract_samples.md)
  :

  Extract samples (draws) from a fitted `coevfit` object

- [`coev_ancestral_states()`](https://docs.ropensci.org/coevolve/reference/coev_ancestral_states.md)
  : Extract ancestral state estimates from a fitted coevolutionary model

## Expose

- [`stancode()`](https://docs.ropensci.org/coevolve/reference/stancode.md)
  :

  Expose the Stan code of a fitted model represented by a `coevfit`
  object

- [`standata()`](https://docs.ropensci.org/coevolve/reference/standata.md)
  :

  Expose the Stan data list for a fitted model represented by a
  `coevfit` object

## Calculate

- [`coev_calculate_delta_theta()`](https://docs.ropensci.org/coevolve/reference/coev_calculate_delta_theta.md)
  :

  Calculate delta theta from a fitted `coevfit` object

- [`coev_calculate_theta()`](https://docs.ropensci.org/coevolve/reference/coev_calculate_theta.md)
  :

  Calculate equilibrium trait values (theta) for a fitted `coevfit`
  object

- [`coev_pred_series()`](https://docs.ropensci.org/coevolve/reference/coev_pred_series.md)
  :

  Predict a co-evolutionary time series from a fitted `coevfit` object

## Save

- [`save_coevfit()`](https://docs.ropensci.org/coevolve/reference/save_coevfit.md)
  :

  Save a fitted `coevfit` object to a file

## Simulate

- [`coev_simulate_coevolution()`](https://docs.ropensci.org/coevolve/reference/coev_simulate_coevolution.md)
  : Simulate the coevolution of multiple variables in discrete time
  steps

## Internal S3 methods

- [`draws(`*`<jax_fit>`*`)`](https://docs.ropensci.org/coevolve/reference/draws.jax_fit.md)
  : Extract draws from jax_fit object
- [`metadata(`*`<jax_fit>`*`)`](https://docs.ropensci.org/coevolve/reference/metadata.jax_fit.md)
  : Extract metadata from jax_fit object
- [`summary(`*`<jax_fit>`*`)`](https://docs.ropensci.org/coevolve/reference/summary.jax_fit.md)
  : Summary statistics for jax_fit object
