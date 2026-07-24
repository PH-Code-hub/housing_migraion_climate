######################################
##------- 24 July 2025 by Peyman ----------##
######################################

10.5281/zenodo.21526163

# housing_migraion_climate
**Overview**
This repository contains the R code used to estimate how housing prices, climate risk, migrant socioeconomic characteristics, and spatial frictions shape internal migration flows across Australian Local Government Areas (LGAs).

The analysis applies gravity-type panel models estimated using Poisson Pseudo-Maximum Likelihood (PPML). The models distinguish migration flows according to the climate-risk characteristics of origins and destinations and examine heterogeneity across urban and rural areas, interstate and within-state movements, and high- and low-population LGAs. The code also estimates interaction models and conducts a comprehensive series of robustness checks.

**Purpose**
The purpose of the analysis is to examine whether housing market conditions operate as gateways that facilitate or constrain internal migration under climate risk.

The models specifically investigate:
1. Whether increasing housing prices at origins are associated with greater migration outflows.
2. Whether housing prices at destinations constrain migration inflows.
3. Whether the effects of housing prices differ between high- and low-climate-risk origins and destinations.
4. Whether migrant income, education, and employment characteristics moderate the effects of housing prices.
5. Whether migration responses differ across urban, rural, interstate, within-state, and population-size contexts.
6. Whether the results remain robust to alternative model specifications, variable measurements, temporal intervals, and climate-disaster indicators.

**Data**

Input Dataset
The main input file used by the script is: final_datset.csv
Each observation represents a directed origin–destination migration flow between two Australian LGAs during a given observation period.

The dataset combines information on:
* Bilateral internal migration flows
* Origin and destination housing prices
* Origin and destination population
* Migrant income, education, and employment characteristics
* Composite climate-risk measures
* Historical disaster frequency
* Rental and ownership affordability
* Local economic and labour-market conditions
* Characteristics of neighbouring LGAs
* Geographic distance
* LGA contiguity
* Interstate movements
* Bass Strait crossings
* Urban–rural migration classifications

Unit of Analysis
The unit of analysis is an origin–destination–time observation: Origin LGA × Destination LGA × Time
The dataset therefore has a bilateral panel structure in which migration from LGA A to LGA B is treated separately from migration from LGA B to LGA A.

Key Outcome Variables
annual_flow:	Annualised bilateral migration flow
flow:	Bilateral migration flow
flow_1yrflows:	One-year bilateral migration flow

Key Explanatory Variables
price_ave:	Average housing price at the origin
price_ave_des:	Average housing price at the destination
med_inc_5yr_score:	Median income characteristics of migrants
med_edu_5yr_score:	Median education characteristics of migrants
med_emp_5yr_score:	Median employment characteristics of migrants
pop:	Origin population
pop_des:	Destination population
total_risk_q:	Origin climate-risk quantile
total_risk_des_q:	Destination climate-risk quantile
risk_dif:	Difference between destination and origin climate risk
risk_dif_q:	Categorised destination–origin risk difference
distance:	Geographic distance between origin and destination
contiguity:	Indicator for geographically contiguous LGAs
interstate:	Indicator for movements across or within state boundaries
bass_strait:	Indicator for movements crossing Bass Strait

Data Availability
The analytical dataset is not necessarily included in the public repository because some underlying data may be subject to licensing, confidentiality, or redistribution restrictions. Researchers wishing to reproduce the analysis should construct a dataset with the required variables and naming conventions described above.


**Statistical Model**
Baseline Model
The baseline analysis uses a structural gravity-type PPML model:

[
E(M_{ijt}\mid X_{ijt}) =
\exp\left(
\beta_1 HP_{it}
+
\beta_2 HP_{jt}
+
\boldsymbol{\gamma}X_{ijt}
+
\alpha_i
+
\delta_j
+
\tau_t
\right),
]

where:

* (M_{ijt}) is the annual migration flow from origin (i) to destination (j) at time (t);
* (HP_{it}) is the housing price at the origin;
* (HP_{jt}) is the housing price at the destination;
* (X_{ijt}) is a vector of migrant, demographic, and spatial controls;
* (\alpha_i) represents origin fixed effects;
* (\delta_j) represents destination fixed effects; and
* (\tau_t) represents time fixed effects.

The model is estimated using the fepois() function from the fixest package. Standard errors are clustered at the origin–destination dyad level using: ~ orig_dest_dyad

**Baseline Controls**
The principal baseline specification includes:
* Origin housing prices
* Destination housing prices
* Migrants’ median education
* Migrants’ median income
* Migrants’ median employment
* Origin population
* Destination population
* Geographic distance
* Contiguity
* Interstate movement
* Bass Strait crossing

**Climate-Risk Subsamples**
Models are estimated for:
* All migration flows
* High-risk origins
* Low-risk origins
* Low-risk destinations
* High-risk destinations
* Moves associated with climate-risk reduction
* Moves associated with climate-risk increase

Separate models are estimated for:
* Urban origins
* Urban destinations
* Rural origins
* Rural destinations
* Within-state movements
* Across-state movements
* High-population origins
* Low-population origins
* High-population destinations
* Low-population destinations

Urban–rural flows are identified using the following migration classifications:
Code	Migration type
UU	Urban origin to urban destination
UR	Urban origin to rural destination
RU	Rural origin to urban destination
RR	Rural origin to rural destination

Interaction Models
The script evaluates whether the effects of origin and destination housing prices vary according to:
* Migrants’ income
* Migrants’ education
* Migrants’ employment
* Destination–origin climate-risk differences
* Geographic distance
* Interstate movement

The script also extracts the variance–covariance terms needed to calculate marginal effects and their standard errors for selected interaction models.

**Data Preparation**
The prep_df() function performs the main preprocessing operations.

These include:
* Checking required identification variables
* Converting fixed-effect identifiers to factors
* Creating missing binary spatial indicators
* Preventing undefined logarithms of distance
* Constructing climate-risk-difference categories
* Applying natural logarithmic transformations
* Applying log(1+x) transformations to count variables
* Preserving missing or invalid observations as NA

Variables such as housing prices, population, income, rent, mortgage costs, affordability measures, and distance are transformed using: log(x)
Disaster-frequency variables and migrant composition scores are transformed using: log1p(x)

**Robustness Checks**
The code implements the following robustness analyses:
1. Alternative temporal interval
    Estimates the model using one-year migration flows.
2. Five-year disaster exposure
    Adds origin and destination disaster frequency measured over five years.
3. Lagged housing prices
    Uses five-year-lagged origin and destination housing prices.
4. Alternative variable scaling
    Re-estimates models using back-transformed covariates.
5. Spatial spillovers
    Includes disaster exposure in neighbouring origin and destination LGAs.
6. Destination–origin differences
    Uses differences between destination and origin characteristics.
7. Alternative fixed effects
    Removes time fixed effects while retaining origin and destination fixed effects.
8. Log-linear model
    Estimates an OLS fixed-effects model using: log(annual_flow + 1)
9. Alternative affordability measure
    Constructs housing-price-to-income measures at origins and destinations.
10. Ten-year disaster exposure
    Adds origin and destination disaster frequency measured over ten years.

**Citation**
When using or adapting this code, please cite the associated study and repository.
Suggested repository citation format:

Habibi-Moshfegh, P. Housing prices, climate risk, and internal migration in Australia: PPML estimation code. GitHub repository.10.5281/zenodo.21526163

Author
Peyman Habibi-Moshfegh


**Disclaimer**
This repository provides research code for academic and reproducibility purposes. Results depend on the construction, coverage, licensing conditions, and preprocessing of the underlying datasets. Users are responsible for verifying variable definitions, model assumptions, data permissions, and the suitability of the methods for their own applications.
