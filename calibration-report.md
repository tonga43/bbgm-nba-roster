# Calibration report 2026-11-24 09:53 CST

- Games simulated against: 258 (this season's real schedule, injuries off)
- Decision: gap to full sim fit now (-0.0065, 0.007) vs (-0.0065, 0.005) at fit -> no drift: keep model
- Runtime: full_sim_fit_s 17.1, formula_apply_s 0.44, total_s 24.5

All rows are BBGM GameSim re-runs of the season so far (8 x 4 reps) with the given ratings, scored against the real
2026-27 per-36 lines (players with 200+ min) and team point differential.

| stat | blend only | full sim fit | formula (current model) |
|---|---|---|---|
| pts36 r / RMSE | 0.89 / 2.782 | 0.948 / 1.951 | 0.948 / 2.011 |
| orb36 r / RMSE | 0.82 / 0.751 | 0.836 / 0.728 | 0.84 / 0.722 |
| drb36 r / RMSE | 0.837 / 1.141 | 0.887 / 0.953 | 0.886 / 0.963 |
| ast36 r / RMSE | 0.846 / 1.26 | 0.901 / 0.981 | 0.928 / 0.867 |
| stl36 r / RMSE | 0.647 / 0.419 | 0.745 / 0.35 | 0.752 / 0.352 |
| blk36 r / RMSE | 0.824 / 0.405 | 0.835 / 0.384 | 0.832 / 0.392 |
| tov36 r / RMSE | 0.721 / 0.759 | 0.856 / 0.561 | 0.879 / 0.572 |
| fga36 r / RMSE | 0.85 / 1.966 | 0.903 / 1.617 | 0.913 / 1.537 |
| fgp r / RMSE | 0.581 / 0.063 | 0.618 / 0.06 | 0.632 / 0.06 |
| tpar r / RMSE | 0.803 / 0.164 | 0.828 / 0.155 | 0.823 / 0.158 |
| tpp r / RMSE | 0.136 / 0.121 | 0.149 / 0.12 | 0.097 / 0.122 |
| ftr r / RMSE | 0.623 / 0.109 | 0.748 / 0.093 | 0.755 / 0.091 |
| ftp r / RMSE | 0.696 / 0.084 | 0.825 / 0.068 | 0.83 / 0.068 |
| mpg r / RMSE | 0.822 / 4.436 | 0.822 / 4.421 | 0.819 / 4.442 |
| team net r / RMSE | 0.927 / 2.57 | 0.937 / 2.38 | 0.93 / 2.5 |
| team win% r | 0.919 | 0.932 | 0.902 |
| score (mean r excl. 3P%, team r) | (0.7662, 0.927) | (0.8271, 0.937) | (0.8336, 0.93) |

## Model in use (fitted 2026-11-23 18:55 CST, 27250 training rows, offsets on)
Forward-model R^2 vs simulated stats: pts36 0.891, orb36 0.901, drb36 0.919, ast36 0.863, stl36 0.751, blk36 0.818, tov36 0.802, fga36 0.885, fgp 0.514, tpar 0.891, tpp 0.324, ftr 0.769, ftp 0.721, mpg 0.92
