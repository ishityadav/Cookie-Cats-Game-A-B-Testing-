[README.md](https://github.com/user-attachments/files/30333822/README.md)
# Mobile Game A/B Testing - Gate Placement & Retention

**Tools:** Python, Pandas, NumPy, SciPy, Statsmodels, Matplotlib

## Business problem

Cookie Cats is a mobile puzzle game with a "gate" at level 30 that pauses players (wait or pay to skip). The product team ran an A/B test to see what happens if the gate is pushed back to level 40 instead - does giving players more free levels before the paywall help retention, or hurt it?

## Data

90,189 users from a real A/B test (dataset originally released by the game's developers, via DataCamp):
- `gate_30` - control group, gate stays at level 30 (44,700 users)
- `gate_40` - treatment group, gate moved to level 40 (45,489 users)
- `sum_gamerounds` - rounds played in first 14 days
- `retention_1` / `retention_7` - whether the user came back 1 day / 7 days after install

## What I did

- Sanity checks on the data (nulls, duplicate users, group balance via chi-square) and removed one extreme outlier (a user with ~50K rounds played in 14 days, clearly not a real player)
- Two-proportion z-test on retention_1 and retention_7 between the two groups
- Bootstrapped the retention_7 difference 5,000 times to double check the z-test result isn't fragile
- Cohort analysis - split users into 4 engagement cohorts based on rounds played, then compared retention across cohorts and versions
- Two-way ANOVA to test if the version effect interacts with engagement level
- Logistic regression predicting retention_7 from version + engagement, to see how much of the effect holds up once you control for how much someone already plays

## Key results

- Retention_1 difference is not significant (p = 0.074)
- Retention_7 difference IS significant (p = 0.0016), and gate_30 (current setup) retains better than gate_40
- Bootstrap confirms this - 99.98% of 5,000 resamples show gate_30 ahead, 95% CI doesn't cross zero
- ANOVA shows the effect isn't the same for everyone - more engaged players show a bigger retention gap between versions
- Logistic regression confirms version still matters for retention even after controlling for engagement level

## Recommendation

Don't move the gate to level 40. Retention is flat or slightly worse under gate_40, and the drop is concentrated among more engaged players - the ones you'd least want to lose. If the team wants to revisit the gate position later, it's probably worth testing changes targeted at highly engaged users specifically rather than rolling it out to everyone.

## Files

- `mobile_game_ab_testing.ipynb` - full analysis notebook
- `cookie_cats.csv` - dataset used
