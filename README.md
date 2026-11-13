```
  ____          _      _____
 / ___|___   __| | ___|__  /___ _ __ ___
| |   / _ \ / _` |/ _ \ / // _ \ '__/ _ \
| |__| (_) | (_| |  __// /|  __/ | | (_) |
 \____\___/ \__,_|\___/____\___|_|  \___/
```

# GeoTrend Detective

**NASA Space Apps Challenge: Be an Earth System Trend Detective**

A browser-based "case file" that helps students and the public find out *what* is changing in the Earth system, *where*, *how much*, and whether the change is statistically significant.

**Live demo:** https://krishnamahajan050308-stack.github.io/nasa/

## What it does

- Guided wizard and one-click case studies recommend variables, with plain-language definitions and the real NASA dataset to use.
- Region map, time-window sliders, atmospheric pressure-level selector, and a two-region split-screen to show opposite trends.
- Raw and deseasonalized time series with a linear trend and 95% confidence band; hover any chart to read the exact month and value.
- Upload or paste a CSV exported from NASA Giovanni or AppEEARS (header lines are ignored), or press **Fetch live NASA data** to pull monthly T2M, TS, PRECTOTCORR, GWETROOT or ALLSKY_SFC_SW_DWN from the NASA POWER API for any location.
- Pick any place: search about 70 built-in countries, cities and regions, enter latitude/longitude, click the map, or drag a box on it.
- An "Explain it simply" panel answers: what changed, why it might have, whether it is significant, what the confidence interval means, and what to investigate next.
- Plain-English verdict answering: what is changing, where and when, how much, and is it significant.

## Statistical methods

| Step | Method |
|---|---|
| Seasonal removal | Anomaly = value minus the long-term mean of that calendar month |
| Rate of change | Ordinary least squares slope, reported per decade, with net change |
| Significance (parametric) | Two-tailed t-test on the slope, n-2 degrees of freedom, 95% CI = b ± t(0.975) x SE |
| Significance (non-parametric) | Mann-Kendall S, tie-corrected variance, Z and tau |
| Robust rate | Theil-Sen slope (median of all pairwise slopes) |
| Autocorrelation | Lag-1 residual autocorrelation r1 and effective sample size n_eff = n(1-r1)/(1+r1). The t-test, Mann-Kendall test and confidence interval are all adjusted (variance inflated by n/n_eff) and the verdict uses the adjusted p-values |
| Data validation | Duplicate months and daily rows are averaged to monthly values, fill values (-999, -9999) are removed, gaps are reported (never interpolated), and short or thin records are flagged |
| Importance vs significance | Net change is reported as a share of the mean and relative to the scatter around the trend, separately from the p-value |
| Verdict | Uses the more cautious of the two autocorrelation-adjusted p-values (p < 0.05 significant, p < 0.01 highly significant) |

Only linear trends are tested.

## Data notes

The built-in case studies are **synthetic demonstration series** shaped like typical observations, so the tool works offline. Use *Fetch live NASA data* or paste a CSV for real analyses. Live fetching needs a host that allows requests to `power.larc.nasa.gov` (GitHub Pages does).

## Run or deploy

It is a single static file, `index.html`.

1. Put `index.html` in a GitHub repository.
2. Settings > Pages > deploy from the main branch, root folder.
3. Open the Pages URL.

## Resources

See the "Evidence locker" section inside the app for NASA, NOAA, KNMI and Canadian Space Agency links.
