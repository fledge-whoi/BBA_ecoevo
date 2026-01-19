# Eco-Evolutionary Forecasting of Albatross Populations

**MATLAB code repository**

This repository contains the MATLAB code used to generate all eco-evolutionary simulations and figures for the manuscript:

> **Climate change outpaces evolutionary adaptation in a long-lived seabird population**  
> Jenouvrier *et al.* (submitted)

The code implements a **stage-structured, evolutionarily explicit population model** that propagates uncertainty across the full eco-evolutionary pathway:

**climate forcing → breeding values → phenotypes → vital rates → population dynamics**

The framework is used to evaluate the **limits of evolutionary rescue** under historical and future climate scenarios.

---

## 1. Model overview

The population is structured by:
- **Breeding value** (genetic component)
- **Phenotype** (expressed trait)
- **Age and breeding stage**

Evolution follows standard quantitative-genetic theory. **Heritability (h²)** controls the partitioning of phenotypic variance into additive genetic and environmental components. Phenotypes affect survival and reproduction through empirically estimated vital-rate functions.

Each simulation is defined by:
- one focal phenotype,
- one heritability value,
- one climate scenario,
- and (for selected figures) an alternative selective-pressure scenario.

---

## 2. Phenotypes encoded in the simulations

Phenotypes are selected using the variable `PHENOTYPE`:

| PHENOTYPE | Trait                                | Category              |
|----------:|--------------------------------------|-----------------------|
| 1         | Activity (landings / take-offs)       | Foraging behavior     |
| 2         | Time on water                         | Foraging behavior     |
| 3         | Return date                           | Phenological trait    |
| 4         | Wing length                           | Morphological trait   |

Each simulation runs **one phenotype at a time**.

---

## 3. Heritability values

Each phenotype is simulated across a range of heritability values: h2 ∈ [0.1, 0.9]

---

## 4. Climate scenarios

Climate forcing is controlled by the variable `caseID`:

| caseID | Scenario        | Description                     |
|-------:|-----------------|----------------------------------|
| 1      | High emissions  | SSP3–7.0                         |
| 3      | Low emissions   | Paris-compatible 2 °C pathway    |

Historical simulations use **observed environmental conditions** and do not include temporal climate change.

---

## 5. Repository structure

The repository is organized **by manuscript figure**.  
Each figure has its own folder containing all scripts and data needed to reproduce that figure.


├── Figure2/
├── Figure3/
├── Figure4/


Each figure folder typically includes:
1. A main plotting script (`MainFigX.m`)
2. A model-building script (`main_Pheno_ENV_*.m`)
3. Parameter functions defining phenotype–environment–vital-rate relationships
4. Core model functions shared across figures
5. Empirical data and/or precomputed simulation outputs

---

## Figure 2 — Historical environment (stationary climate)

Figure2/
├── MainFig2.m
├── main_Pheno_ENV_HIST.m
├── parameterPHENOTYPE_HIST.m
├── figure2.pdf
└── resultsMATfiles.zip


- `MainFig2.m`  
  Loads simulation outputs and generates Figure 2.

- `main_Pheno_ENV_HIST.m`  
  Builds and runs the eco-evolutionary model under historical (stationary) environmental conditions.

- `parameterPHENOTYPE_HIST.m`  
  Defines phenotype–vital-rate relationships for the historical environment.

---

## Figure 3 — Future climate projections with uncertainty

Figure3/
├── MainFig3.m
├── main_Pheno_ENV_ClimateChange_UNC.m
├── parameterPHENOTYPE_ENV.m
├── parameterPHENOTYPE_ENVSTO.m
├── ClimateScenario_Case1.mat
├── ClimateScenario_Case2.mat
├── ClimateScenario_Case3.mat
└── ClimateScenario_Case4.mat


This figure propagates uncertainty in:
- climate projections,
- demographic processes,
- phenotype–environment–vital-rate relationships.

- `parameterPHENOTYPE_ENV.m`  
  Mean phenotype–environment–vital-rate relationships.

- `parameterPHENOTYPE_ENVSTO.m`  
  Stochastic version used to propagate uncertainty.

---

## Figure 4 — Modified selective pressure on juvenile traits

Figure4/
├── MainFig4.m
├── main_Pheno_ENV_ClimateChange_meanEns.m
├── parameterPHENOTYPE_ENV_SP.m
├── figure4.pdf
└── ecoEvoResults_*.mat

This figure explores a **counterfactual selective-pressure scenario**, modifying selection on juvenile wing length and juvenile survival.

---

## 6. Core model functions (shared across figures)

The following functions define the core eco-evolutionary model and appear in all figure folders:

- `Umat2.m` — survival and state-transition matrices  
- `Fmat2.m` — fertility / offspring-production matrices  
- `invlogit.m`, `invlogitG.m` — logit transforms for probabilities  

---

## 7. Empirical data files

- `FOR.mat` — foraging behavior data  
- `wing_corrected.mat` — wing-length measurements  
- `Nobs_FRA.mat` — observed population size data  

These data are used for model parameterization, initialization, and validation.

---

## 8. How figures are generated (summary)

For each figure:
1. `MainFigX.m`  
   Sets phenotype, heritability, climate scenario, and plotting options.
2. Calls `main_Pheno_ENV_*.m`  
   Builds the model, runs simulations, saves outputs.
3. Computes derived quantities  
   Population size, trait means, variances, adaptation rates.
4. Produces the final manuscript figure.

---

## 9. Program architecture (main scripts)

All main scripts follow the same three-block structure:

**Block 1 — Initialization**
- Load empirical trait and climate data
- Set phenotype, heritability, and climate scenario

**Block 2 — Model construction and simulation**
- Discretize breeding values and phenotypes
- Compute genetic and environmental variances
- Build survival (`U`) and reproduction (`F`) matrices
- Construct inheritance and phenotype-mapping operators
- Run burn-in and forward eco-evolutionary projections
- Save outputs for each scenario

**Block 3 — Outputs and plotting**
- Population size through time
- Mean phenotype and breeding value
- Phenotypic and genetic variances
- Adaptation rates
- Phenotype-weighted vital rates

---

## Contact

**Stéphanie Jenouvrier**  
Woods Hole Oceanographic Institution  
📧 sjenouvrier@whoi.edu


