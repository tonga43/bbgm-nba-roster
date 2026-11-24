# Rating calibration against BBGM's game engine

## What runs when
| job | command | what it does | runtime (Nov 23, 248 games played) |
|---|---|---|---|
| daily (publish.sh) | `update.py` (`RATING_METHOD=formula`, default) | blend sets ovr; saved formula reshapes skill profiles; 2 quick check sims for the report | ratings step about 5 s (formula 0.4 s + checks), whole build about 20 s offline / 45 s with fetches |
| weekly (Sunday) | `calibrate.py` (`--force` to refit regardless) | re-sims the season with blend / full sim fit / formula, drift check, refit if worse | about 50 s now; scales with games played (a few minutes by March) |

## Pipeline per player (daily)
1. **Blend (`blend.py`)** sets ovr from BBGM's Real Players base plus 2026-27 production (BPM, PER, WS/48, usage x
   efficiency, stat model). Safeguards: `w = 1-exp(-(MIN/300)^1.6)`, +/-(1+6w) ovr (max 7), team scale lock, archetype
   offset, recomputed from base daily.
2. **Formula (`surrogate.py`)** reshapes the skill profile at that exact ovr so BBGM's engine produces his real
   per-36 line:
   - forward model `stat_k = f_k(ratings, role)` for pts, orb, drb, ast, stl, blk, tov, fga per 36, FG%, 3PA rate, 3P%,
     FT rate, FT%, mpg. Inputs: the 13 fitted ratings (10 skills, spd, jmp, endu) plus hgt and stre, their squares,
     and hgt x tp/ins/reb. Role: ovr gap to the team's top-8 mean and the roster slot. Weighted ridge regression.
   - a per-player engine residual (`offsets`) from the calibration week, shrunk by min/(min+400)
   - MAP solve: `delta = argmin sum_k W_k (y_k - f_k(r+delta) - off_k)^2 + sum_v (delta_v/(tau_v w))^2`, 3 Gauss-Newton
     steps. `W_k = 1/(real sampling variance + floor^2)`, `tau = 15` (skills) / `3` (athletic).
   - target `y` = real 2026-27 line minus up to 50% of the 2025-26 real-vs-predicted gap (archetype offset, scaled by
     2025-26 min/(min+1000))
   - caps on top of the blend: skills +/-8, tp +/-2 (tp drives both 3PA rate and 3P% in BBGM; letting it chase 3PA rate
     wrecked 3P%), spd/jmp/endu +/-2. Total vs base stays within +/-12 skills / +/-3 athletic. hgt and stre are fixed.
   - re-shift (bisection) so ovr equals the blend's ovr exactly; pot and skills are recomputed by BBGM on import.
3. Pot recompute for every changed player (`update.py`).

## Calibration (weekly, `calibrate.py`)
- Training data: 64 league sims x 3 reps of the season's real schedule (injuries off, real availability and
  rotation order), each with every player's 13 ratings randomly perturbed (+/-10 skills, +/-4 athletic) around the
  current blended ratings, giving about 27,000 player-season rows per fit.
- The formula is scored against the full simulation fit (`simfit.py`, profile mode: simulated Jacobian + 4
  Gauss-Newton re-sims, about 20 s). Both are scored by re-simming the season (8 x 4 reps) and comparing with the real
  lines (players with 200+ min) and team point differential.
- **Drift rule:** refit when the formula's gap to the full sim fit grows by more than 0.015 in mean per-stat r
  (3P% excluded as early-season noise) or 0.03 in team point-differential r, compared with the gap at fit time.
  A refit is kept only if it beats the current model on that week's data.
- Files: `model.json` (coefficients, standardization, offsets, metrics at fit time), `fit-report.md|json` (latest
  calibration), `calibrate-log.md` (history of decisions).

## Why the sim does not set ovr
A full simulation fit that set ovr too (`RATING_METHOD=sim`) was tested out of sample: fitted on odd games, scored on
even games, and the reverse. It predicted held-out per-36 lines better than base, but team point differential no
better than base (r 0.57-0.79 vs 0.57-0.78). The blend did clearly better (0.70-0.85), as did blend + sim profile
(0.69-0.87). Box-score per-36 lines under-measure defense and team impact, which BPM/WS capture. So production sets
the level, and the engine calibration sets the shape.
