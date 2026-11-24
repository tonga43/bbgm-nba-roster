# Rating calibration against BBGM's game engine

## What runs when
| job | command | what it does | runtime (Nov 23, 248 games played) |
|---|---|---|---|
| daily (publish.sh) | `update.py` (`RATING_METHOD=formula`, default) | blend sets ovr; formula reshapes skill profiles; J5 table applied (+ update for new games); BBGM roster sort | J5 apply 0.5 s; whole build about 18 s offline (Nov 23) |
| weekly (Sunday) | `calibrate.py` (`--force` to refit regardless) | surrogate drift check (as before), then the J5 lineup-matched replay loop, the pass check and the starters match | about 75 s now (surrogate 25 s + J5 46 s); scales with games played |

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

## J5 player-by-player adjustment (live 2026-11-23; points/PER terms and ceiling pass marks 2026-11-24, `j5.py`)
- **Replay harness** (`replay/`): every game played is re-simulated with BBGM's GameSim using the real lineups: the
  real starters, then the rest of that night's players by real minutes; anyone who did not play sits out. 8 node
  processes, reps per game set by the step (8 in the loop, 48 in the final check). `ptModifier` is 1 for everyone, as
  BBGM does for AI teams.
- **Formula with J5 settings**: profile caps skills 14, tp 5, athletic 3 (total 12/3); engine offsets re-measured in a
  replay at the blend ratings (blended 50/50 with the previous offsets in the stat-shape round).
- **One scalar per player**, Gauss-Newton on his own evidence: `a += damp * (Wm*slope*gm + Wi*kon*gi - a/tau^2) /
  (Wm*slope^2 + 5*Wi*kon^2 + 1/tau^2)`, where `gm` = real - sim MPG, `gi` = real on-court +/- per 48 (+ a shift toward
  the team's win%, `0.75*(28.7*(win%-0.5) - real PD)`) - sim on-court +/- per 48; `Wm = min(1, gp/5)/2.5^2`,
  `Wi = min/6912`, `tau = 6`, `damp = 0.8`, 8 rounds, cap +/-15. `slope` (~0.75 MPG per ovr) and `kon` (~0.38 per 48 per
  ovr) are measured each week by perturbation replays. No team step, no team cap.
- **League constant**: one constant for all players keeps the mean team MOV (BBGM formula: -102.98, 0.3334,
  -0.1609, top 10) at the blend's level. The ovr change is realized by a uniform shift (mostly oIQ/dIQ); hgt and stre
  never move.
- **Points and PER terms (live 2026-11-24)**: the same scalar also matches the player's own box score. Points:
  `+ Wb*sppg*(real PPG - sim PPG)`, `Wb = min(1, gp/5)/1.5^2`; when the PPG miss is over 1 and the +/- evidence points
  the other way, the +/- term is dropped for that player. PER: `+ 2*min(1, min/400)/3^2 * sper*(real PER - sim PER)`.
  `sppg` / `sper` (~0.58 PER per ovr) are measured by the same perturbation replay. PER is Hollinger's formula as in
  BBGM's `advStats.basketball.ts`, league-normalized to 15, computed identically on real and replayed totals (team
  totals = sums of the team's players, no pace adjustment). Tested against 30+ variants (on-off, ridge plus-minus,
  team-relative, no plus-minus, higher PER blend weight, endurance/defense channels, top-2 exemptions); see
  `experiments/docs-merge/phase2/`.
- **Daily**: the table's adjustments plus one Gauss-Newton step for changes in real MPG, on-court +/- and PPG since
  the weekly snapshot (no sims; the PER term is refreshed weekly only). A 4-rep replay pass check goes in the build log.
- **Scorecard** (weekly, 48-rep replay): rotation players (200+ real min) pass the core check when MPG, PPG, RPG, APG
  (strict / relative tolerances in `weekly-status.md`) and PER (+/-2) all match; teams count when within +/-2 wins.
- **Noise ceiling** (weekly): 20 runs of ONE simulated season of the games played with the final ratings, each scored
  against the 48-rep mean of the same ratings, i.e. what perfect ratings would score given BBGM's randomness and the
  sample sizes so far (Nov 24, 258 games: players ~48.7% relative, teams ~21.4/30).
- **Pass (ceiling-based, Tony 2026-11-24)**: player relative pass rate >= ceiling - 2 points AND teams within +/-2
  wins >= round(ceiling teams) - 1. The fixed tolerances cover rerun noise (~1.5 points). The absolute marks
  (27/30 teams, 90% players) are still reported as `pass_absolute`.
- **Weekly output**: `j5-table.json` (adjustments, offsets, snapshot, responses, history), `weekly-status.json|md`.
  The routine keys on `weekly-status.json` -> `alert`: true when the ceiling-based pass fails, or the relative player
  pass rate fell more than 3 points vs the previous week (compared only when the scorecard definition, `core`, is the
  same).
- **Minutes are not decoupled**: BBGM's per-player playing-time modifier (`ptModifier`) would be the clean lever, but
  `loadTeams.ts` resets it to 1 for every AI-controlled team before each game (and league import resets it too), so it
  cannot carry real minutes in a shared league file.
- **Roster order**: `bbgm_roster_order` ports BBGM's `rosterAutoSort` (valueNoPot = 0.8 ovr' + 0.2 PER-based current
  value, ovr' standardized over all players; then `findStarters`), and every team has `keepRosterSorted`.

## Surrogate calibration (weekly, `calibrate.py`)
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
