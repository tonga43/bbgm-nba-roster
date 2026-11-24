# Calibration report 2026-11-23 18:55 CST

- Games simulated against: 248 (this season's real schedule, injuries off)
- Decision: first fit; new model saved (score (0.8289, 0.862))
- Runtime: full_sim_fit_s 17.1, refit_s 21.5, total_s 43.2

All rows are BBGM GameSim re-runs of the season so far (8 x 4 reps) with the given ratings, scored against the real
2026-27 per-36 lines (players with 200+ min) and team point differential.

| stat | blend only | full sim fit | formula (new model, with player offsets) | formula (new model, no offsets) |
|---|---|---|---|---|
| pts36 r / RMSE | 0.879 / 2.905 | 0.942 / 2.057 | 0.94 / 2.125 | 0.938 / 2.11 |
| orb36 r / RMSE | 0.826 / 0.747 | 0.833 / 0.738 | 0.844 / 0.715 | 0.836 / 0.727 |
| drb36 r / RMSE | 0.839 / 1.15 | 0.881 / 0.984 | 0.887 / 0.963 | 0.882 / 0.985 |
| ast36 r / RMSE | 0.844 / 1.273 | 0.906 / 0.969 | 0.934 / 0.833 | 0.928 / 0.901 |
| stl36 r / RMSE | 0.606 / 0.441 | 0.694 / 0.387 | 0.717 / 0.378 | 0.706 / 0.385 |
| blk36 r / RMSE | 0.838 / 0.391 | 0.839 / 0.386 | 0.84 / 0.382 | 0.841 / 0.381 |
| tov36 r / RMSE | 0.696 / 0.774 | 0.854 / 0.566 | 0.869 / 0.584 | 0.868 / 0.583 |
| fga36 r / RMSE | 0.839 / 2.047 | 0.905 / 1.609 | 0.912 / 1.549 | 0.909 / 1.579 |
| fgp r / RMSE | 0.561 / 0.064 | 0.612 / 0.06 | 0.613 / 0.061 | 0.597 / 0.061 |
| tpar r / RMSE | 0.793 / 0.166 | 0.818 / 0.158 | 0.816 / 0.16 | 0.817 / 0.159 |
| tpp r / RMSE | 0.165 / 0.123 | 0.081 / 0.125 | 0.192 / 0.121 | 0.172 / 0.121 |
| ftr r / RMSE | 0.605 / 0.113 | 0.75 / 0.094 | 0.755 / 0.094 | 0.742 / 0.095 |
| ftp r / RMSE | 0.675 / 0.086 | 0.848 / 0.066 | 0.841 / 0.066 | 0.833 / 0.067 |
| mpg r / RMSE | 0.81 / 4.77 | 0.809 / 4.713 | 0.808 / 4.74 | 0.808 / 4.733 |
| team net r / RMSE | 0.858 / 3.5 | 0.867 / 3.4 | 0.862 / 3.49 | 0.845 / 3.69 |
| team win% r | 0.867 | 0.884 | 0.875 | 0.867 |
| score (mean r excl. 3P%, team r) | (0.7547, 0.858) | (0.8224, 0.867) | (0.8289, 0.862) | (0.8235, 0.845) |

## Model in use (fitted 2026-11-23 18:55 CST, 27250 training rows, offsets on)
Forward-model R^2 vs simulated stats: pts36 0.891, orb36 0.901, drb36 0.919, ast36 0.863, stl36 0.751, blk36 0.818, tov36 0.802, fga36 0.885, fgp 0.514, tpar 0.891, tpp 0.324, ftr 0.769, ftp 0.721, mpg 0.92
