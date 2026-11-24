# Calibration report 2026-11-24 06:54 CST

- Games simulated against: 258 (this season's real schedule, injuries off)
- Decision: gap to full sim fit now (-0.0087, -0.002) vs (-0.0065, 0.005) at fit -> no drift: keep model
- Runtime: full_sim_fit_s 15.4, formula_apply_s 0.42, total_s 22.1

All rows are BBGM GameSim re-runs of the season so far (8 x 4 reps) with the given ratings, scored against the real
2026-27 per-36 lines (players with 200+ min) and team point differential.

| stat | blend only | full sim fit | formula (current model) |
|---|---|---|---|
| pts36 r / RMSE | 0.89 / 2.765 | 0.948 / 1.92 | 0.949 / 1.987 |
| orb36 r / RMSE | 0.818 / 0.755 | 0.837 / 0.727 | 0.835 / 0.729 |
| drb36 r / RMSE | 0.84 / 1.132 | 0.887 / 0.954 | 0.888 / 0.953 |
| ast36 r / RMSE | 0.845 / 1.264 | 0.911 / 0.94 | 0.931 / 0.853 |
| stl36 r / RMSE | 0.637 / 0.418 | 0.737 / 0.355 | 0.735 / 0.365 |
| blk36 r / RMSE | 0.824 / 0.404 | 0.832 / 0.388 | 0.834 / 0.393 |
| tov36 r / RMSE | 0.721 / 0.753 | 0.851 / 0.573 | 0.881 / 0.573 |
| fga36 r / RMSE | 0.843 / 2.002 | 0.903 / 1.609 | 0.912 / 1.549 |
| fgp r / RMSE | 0.594 / 0.063 | 0.611 / 0.06 | 0.644 / 0.059 |
| tpar r / RMSE | 0.797 / 0.165 | 0.826 / 0.156 | 0.824 / 0.158 |
| tpp r / RMSE | 0.157 / 0.12 | 0.167 / 0.119 | 0.175 / 0.119 |
| ftr r / RMSE | 0.618 / 0.109 | 0.75 / 0.092 | 0.756 / 0.091 |
| ftp r / RMSE | 0.678 / 0.086 | 0.815 / 0.07 | 0.835 / 0.067 |
| mpg r / RMSE | 0.822 / 4.453 | 0.824 / 4.397 | 0.821 / 4.415 |
| team net r / RMSE | 0.931 / 2.5 | 0.938 / 2.38 | 0.94 / 2.33 |
| team win% r | 0.909 | 0.919 | 0.909 |
| score (mean r excl. 3P%, team r) | (0.7636, 0.931) | (0.8255, 0.938) | (0.8342, 0.94) |

## Model in use (fitted 2026-11-23 18:55 CST, 27250 training rows, offsets on)
Forward-model R^2 vs simulated stats: pts36 0.891, orb36 0.901, drb36 0.919, ast36 0.863, stl36 0.751, blk36 0.818, tov36 0.802, fga36 0.885, fgp 0.514, tpar 0.891, tpp 0.324, ftr 0.769, ftp 0.721, mpg 0.92
