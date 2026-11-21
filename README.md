# Cookie Cats A/B Test Analysis: does moving the first gate change retention?

A/B test analysis in Python: how moving a game gate from level 30 to 40 affects 7-day retention.

## ❓Business question

An analysis of an A/B test in the mobile puzzle game **Cookie Cats**, where the first "gate" (a forced wait before the player can continue or make an in-app purchase) was moved from level 30 to level 40. The goal is to find out whether the change affects **7-day player retention**, and to give a clear recommendation.



Moving the gate 10 levels later may let players enjoy the game longer before they hit a wait, or it may remove the pause that keeps them coming back. Which version retains more players one week after install?

## 🗂️Data

Public Cookie Cats dataset (`cookie_cats.csv`), about 90K players randomly assigned at install to one of two groups.

| Column | Description |
|---|---|
| `userid` | Unique player ID |
| `version` | `gate_30` (control, gate at level 30) or `gate_40` (test, gate at level 40) |
| `sum_gamerounds` | Game rounds played in the first week after install |
| `retention_1` | Player came back and played 1 day after install |
| `retention_7` | Player came back and played 7 days after install |


## 📌Approach

1. **Test planning.** Calculated the sample size needed to detect a 1 percentage point lift in retention (baseline 19%, expected 20%) at 5% significance and 80% power.
2. **Descriptive comparison.** Compared mean 7-day retention for the two versions.
3. **Hypothesis test.** Two-proportion z-test (two-sided) plus 95% confidence intervals for each group.
4. **Cross-check.** Chi-square test of independence between game version and 7-day retention.

Hypotheses:

- H0: 7-day retention is the same in both versions (p30 = p40)
- H1: 7-day retention differs between versions (p30 ≠ p40)

## ✅Results

| Step | Result |
|---|---|
| Required sample size | 24,641 players per group, 49,282 in total |
| 7-day retention, gate at level 30 | 19.0% |
| 7-day retention, gate at level 40 | 18.2% |
| Z-test | z = 3.16, p = 0.0016 |
| 95% CI, gate at level 30 | [0.187, 0.194] |
| 95% CI, gate at level 40 | [0.178, 0.186] |
| Chi-square test | χ² = 9.96 (critical value 3.84), 1 degree of freedom, p = 0.0016 |

The actual groups are well above the required sample size, and both tests give the same answer.

## 🔐Conclusion

The difference is statistically significant at the 5% level: reject H0. The confidence intervals of the two versions do not overlap, and the chi-square test confirms the dependence between gate position and 7-day retention.

**Recommendation:** keep the first gate at level 30. Moving it to level 40 most likely lowered 7-day retention by about 0.8 percentage points.

## 🎮Limitations and next steps

- The effect is statistically significant but small (about 0.8 pp). Its business value should be checked against revenue, not retention alone.
- Only 7-day retention is analysed here. The next steps are to check 1-day retention and the number of game rounds (`sum_gamerounds`) to see whether the effect appears earlier.
- Purchase data is not part of the dataset, so the impact on revenue cannot be measured directly.

## 🛠Tools

Python, pandas, NumPy, SciPy, statsmodels

