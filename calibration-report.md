# Calibration report 2026-11-24 11:03 CST

- Games simulated against: 258 (this season's real schedule, injuries off)
- Decision: gap to full sim fit now (-0.0038, 0.009) vs (-0.0065, 0.005) at fit -> no drift: keep model
- Runtime: full_sim_fit_s 19.0, formula_apply_s 0.5, total_s 27.3

All rows are BBGM GameSim re-runs of the season so far (8 x 4 reps) with the given ratings, scored against the real
2026-27 per-36 lines (players with 200+ min) and team point differential.

| stat | blend only | full sim fit | formula (current model) |
|---|---|---|---|
| pts36 r / RMSE | 0.893 / 2.758 | 0.947 / 1.973 | 0.947 / 2.005 |
| orb36 r / RMSE | 0.814 / 0.761 | 0.844 / 0.714 | 0.836 / 0.727 |
| drb36 r / RMSE | 0.843 / 1.126 | 0.881 / 0.977 | 0.887 / 0.959 |
| ast36 r / RMSE | 0.844 / 1.265 | 0.906 / 0.97 | 0.929 / 0.865 |
| stl36 r / RMSE | 0.644 / 0.418 | 0.733 / 0.359 | 0.739 / 0.361 |
| blk36 r / RMSE | 0.825 / 0.402 | 0.827 / 0.397 | 0.828 / 0.398 |
| tov36 r / RMSE | 0.713 / 0.764 | 0.856 / 0.57 | 0.88 / 0.57 |
| fga36 r / RMSE | 0.849 / 1.969 | 0.906 / 1.586 | 0.914 / 1.527 |
| fgp r / RMSE | 0.585 / 0.063 | 0.635 / 0.059 | 0.617 / 0.061 |
| tpar r / RMSE | 0.798 / 0.165 | 0.823 / 0.157 | 0.827 / 0.157 |
| tpp r / RMSE | 0.171 / 0.12 | 0.169 / 0.119 | 0.164 / 0.119 |
| ftr r / RMSE | 0.633 / 0.108 | 0.74 / 0.094 | 0.757 / 0.091 |
| ftp r / RMSE | 0.674 / 0.086 | 0.834 / 0.067 | 0.822 / 0.068 |
| mpg r / RMSE | 0.824 / 4.44 | 0.824 / 4.399 | 0.822 / 4.413 |
| team net r / RMSE | 0.926 / 2.59 | 0.927 / 2.55 | 0.918 / 2.71 |
| team win% r | 0.9 | 0.899 | 0.898 |
| score (mean r excl. 3P%, team r) | (0.7645, 0.926) | (0.8274, 0.927) | (0.8312, 0.918) |

## Model in use (fitted 2026-11-23 18:55 CST, 27250 training rows, offsets on)
Forward-model R^2 vs simulated stats: pts36 0.891, orb36 0.901, drb36 0.919, ast36 0.863, stl36 0.751, blk36 0.818, tov36 0.802, fga36 0.885, fgp 0.514, tpar 0.891, tpp 0.324, ftr 0.769, ftp 0.721, mpg 0.92
