# Does Farm Credit in India Actually Come With Insurance?

**Key findings: The relation is not significant, and that gap matters. **

India's premier crop insurance scheme, Pradhan Mantri Fasal Beema Yojna, used to be automatic: if you took
out a farm loan, you were signed up for insurance with the premium deducted
from your crop loan, no extra step required. In 2020, this insurance aspect
became optional, even for people taking loans. This project asks a simple
question: **once insurance stopped being automatic, did farmers still end up
insured, or is the correlative mom?**

I built a state-by-state dataset by merging two different sources covering 2017 to 2022, tracking how much
agricultural credit each state disbursed and how much crop insurance was
actually taken up in that particular state, and tested whether the two still moved together after the
rule change.

**What I found:** they don't move together, either before the rule change or
after. A state's farm credit growing faster or slower in a given year tells
you almost nothing about whether its insurance uptake grew too. The two are
more disconnected than you'd expect from a scheme that had the two bundled together, that is, until recently.

## Why this matters

The decoupling we observe between credit and insurance can be treated as a policy gap.
Farmers can be taking on more debt without a matching safety net when crops
fail. States with the highest climate risk are exactly where you'd want that
link to be strongest, so I also checked whether climate-vulnerable states
behave differently. They don't, detectably, though with only 26 states in the
dataset, that's more "we can't see a big effect" than "there's definitely no
effect at all" (more on that below).

## What's in this repo

| File | What it is |
|---|---|
| `Agri_Insurance.ipynb` | The full analysis: cleaning the raw data, merging it into one dataset, running the statistics, and building the charts |
| `RBI_Credit.xlsx` | State-wise farm credit data (Reserve Bank of India) |
| `RS_Session_260_AU_194_A_to_B_*.csv` | Official crop insurance data by state, one file per year (2017 to 2022), from government records tabled in Parliament |
| `cvi_scores - Sheet1.csv` | A climate-risk score for each Indian state (CEEW Climate Vulnerability Index) |

## How I tested it (for the technically curious)

I used a panel regression, a statistical method built for exactly this kind
of "same states, tracked over several years" data. It's designed to strip out
two things that could otherwise fake a relationship: whatever makes a state
permanently different (better banking access, richer soil, etc.) and whatever
hit every state in a given year (a nationwide policy change, a bad monsoon
year). What's left is the cleanest possible test of whether a state's own
credit growth predicted its own insurance growth, year to year, and the
answer, consistently, was no.

I also checked this a second way, using a companion technique (a Mundlak-style
correlated random effects model) that can additionally show whether states
with historically higher credit also have historically higher insurance, a
related but different question. That comparison is discussed in the
notebook.

## The honest limitations

- Only 26 states and 134 data points total. This is a small dataset, so a
  "no relationship found" result really means "no large relationship is
  visible," not "definitely zero effect." A smaller, real effect could still
  be hiding in there undetected.
- A few states (Punjab, Gujarat, Andhra Pradesh, Telangana, Jharkhand) either
  never joined the insurance scheme or dropped out partway through. This
  isn't random, and it's discussed in the notebook as a finding in its own
  right rather than just a data gap.
- Insurance "premium collected" is a proxy for uptake, not a perfect
  headcount of insured farmers.

## Running it yourself

```bash
git clone https://github.com/AdityaSharma1303/Studying-Agri-and-Credit-relation-in-India-.git
cd Studying-Agri-and-Credit-relation-in-India-
pip install pandas numpy matplotlib seaborn linearmodels openpyxl jupyter
jupyter notebook Agri_Insurance.ipynb
```

## Author

Aditya, MA Environmental Economics, Madras School of Economics.
