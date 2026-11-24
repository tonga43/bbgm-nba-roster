# Calibration report 2026-11-23 21:21 CST

- Games simulated against: 248 (this season's real schedule, injuries off)
- Decision: gap to full sim fit now (-0.0063, 0.0) vs (-0.0065, 0.005) at fit -> no drift: keep model
- Runtime: full_sim_fit_s 17.3, formula_apply_s 0.47, total_s 24.6

All rows are BBGM GameSim re-runs of the season so far (8 x 4 reps) with the given ratings, scored against the real
2026-27 per-36 lines (players with 200+ min) and team point differential.

| stat | blend only | full sim fit | formula (current model) |
|---|---|---|---|
| pts36 r / RMSE | 0.892 / 2.774 | 0.948 / 1.964 | 0.948 / 1.994 |
| orb36 r / RMSE | 0.828 / 0.742 | 0.838 / 0.728 | 0.841 / 0.724 |
| drb36 r / RMSE | 0.841 / 1.136 | 0.881 / 0.984 | 0.888 / 0.956 |
| ast36 r / RMSE | 0.848 / 1.255 | 0.909 / 0.957 | 0.929 / 0.861 |
| stl36 r / RMSE | 0.647 / 0.422 | 0.744 / 0.357 | 0.744 / 0.362 |
| blk36 r / RMSE | 0.826 / 0.403 | 0.842 / 0.384 | 0.836 / 0.388 |
| tov36 r / RMSE | 0.712 / 0.758 | 0.861 / 0.547 | 0.885 / 0.551 |
| fga36 r / RMSE | 0.854 / 1.962 | 0.905 / 1.608 | 0.914 / 1.543 |
| fgp r / RMSE | 0.575 / 0.063 | 0.606 / 0.06 | 0.634 / 0.059 |
| tpar r / RMSE | 0.801 / 0.164 | 0.833 / 0.155 | 0.835 / 0.156 |
| tpp r / RMSE | 0.143 / 0.123 | 0.174 / 0.122 | 0.138 / 0.122 |
| ftr r / RMSE | 0.612 / 0.112 | 0.754 / 0.094 | 0.747 / 0.094 |
| ftp r / RMSE | 0.679 / 0.086 | 0.828 / 0.068 | 0.83 / 0.068 |
| mpg r / RMSE | 0.819 / 4.428 | 0.816 / 4.396 | 0.816 / 4.394 |
| team net r / RMSE | 0.927 / 2.57 | 0.925 / 2.6 | 0.925 / 2.61 |
| team win% r | 0.907 | 0.932 | 0.918 |
| score (mean r excl. 3P%, team r) | (0.7642, 0.927) | (0.8281, 0.925) | (0.8344, 0.925) |

## Model in use (fitted 2026-11-23 18:55 CST, 27250 training rows, offsets on)
Forward-model R^2 vs simulated stats: pts36 0.891, orb36 0.901, drb36 0.919, ast36 0.863, stl36 0.751, blk36 0.818, tov36 0.802, fga36 0.885, fgp 0.514, tpar 0.891, tpp 0.324, ftr 0.769, ftp 0.721, mpg 0.92
