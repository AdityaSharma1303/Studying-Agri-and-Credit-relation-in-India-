# Agricultural Credit and Crop Insurance in India: A Panel Data Analysis

A state-year panel analysis (2017–2022) examining whether growth in agricultural
credit predicts growth in crop insurance uptake under India's Pradhan Mantri
Fasal Bima Yojana (PMFBY), and whether that relationship shifted after PMFBY
enrollment moved from compulsory (loanee-linked) to voluntary in Kharif 2020.

## Motivation

Until 2020, farmers taking a seasonal crop loan were automatically enrolled in
PMFBY, with the premium deducted at source. From Kharif 2020 onward, enrollment
became voluntary for all farmers, including loanees. This project asks whether
that administrative "unbundling" of credit and insurance shows up as a
detectable change in the state-level relationship between the two, and whether
the relationship differs by a state's climate vulnerability — the states where
crop insurance arguably matters most.

## Data

| Source | Content | Coverage |
|---|---|---|
| RBI Table 157 | State-wise outstanding agricultural credit | 2016–2022 |
| PMFBY administrative data (Rajya Sabha unstarred questions) | Gross premium and claims paid, by state and insurer | 2017–2022 |
| CEEW Climate Vulnerability Index | Composite state-level climate vulnerability score | Single cross-section |

All three sources are public. Credit and PMFBY data are merged into a
state-year panel (inner join); CVI is merged on state only, since it has no
time dimension.

## Method

Two-way fixed-effects panel regression (`linearmodels.PanelOLS`) with
state-clustered standard errors:

```
log(Premium) ~ log(Credit) + EntityEffects + TimeEffects              (Model 1)
log(Premium) ~ log(Credit) + post_2020 + log(Credit)×post_2020 + FE   (Model 2)
log(Premium) ~ log(Credit) + CVI + log(Credit)×CVI + post_2020 + FE   (Model 3)
log(Claims)  ~ log(Credit) + CVI + log(Credit)×CVI + post_2020 + FE   (Model 3b)
```

State fixed effects absorb time-invariant state characteristics; year fixed
effects absorb shocks common to all states in a given year. Standard errors
are clustered by state to account for within-state serial correlation.

## Key findings

- Within-state credit growth does **not** significantly predict within-state
  PMFBY premium growth in any specification (all p > 0.4).
- The credit–insurance relationship does not detectably change between the
  compulsory and voluntary enrollment eras.
- No significant moderation by climate vulnerability is found, though this
  should be read alongside the panel's limited statistical power (26 states,
  unbalanced, 134 observations) rather than as evidence the relationship is
  genuinely absent.
- The panel's imbalance is itself informative: states with no PMFBY
  observations in later years (e.g. Punjab, Gujarat, Andhra Pradesh,
  Telangana, Jharkhand) either never joined or exited the scheme, consistent
  with reported adverse-selection and fiscal-burden pressures documented in
  PMFBY policy literature.

Full derivation of these results, confidence intervals, and a discussion of
statistical power, the Mundlak (correlated random effects) alternative to a
standard Hausman test, and a proposed state-exit robustness check are in the
notebook.

## Repository structure

```
.
├── Agri_Insurance.ipynb       # Full analysis: cleaning, merging, regressions, charts
├── data/                      # Raw input files (see Data section above)
├── outputs/                   # Saved charts and regression_results.xlsx
├── requirements.txt
└── README.md
```

## Reproducing the analysis

```bash
git clone https://github.com/<your-username>/agri-credit-insurance-india.git
cd agri-credit-insurance-india
pip install -r requirements.txt
jupyter notebook Agri_Insurance.ipynb
```

## Limitations

- Small, unbalanced panel (26 states, 134 state-year observations) limits
  statistical power; the analysis can rule out large elasticities but not
  small-to-moderate ones.
- State-level premium totals are an imperfect proxy for farmer-level
  enrollment, since they also reflect crop-risk pricing and sum insured.
- Panel attrition (states exiting PMFBY) is not random and is discussed
  descriptively rather than corrected for formally, given the sample size.

## Author

Aditya — MA Environmental Economics, Madras School of Economics.
