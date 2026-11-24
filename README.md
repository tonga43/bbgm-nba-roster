# Real NBA for Basketball GM, updated daily

A [Basketball GM](https://play.basketball-gm.com) league file of the **current real NBA season (2026-27)**. It is built on
BBGM's own *Real Players* league, which brings full rating and stat history, awards, contracts with extensions, real
draft-pick ownership and real draft classes. Ratings blend BBGM's base with 2026-27 production, weighted by minutes. Then each player's skill profile is tuned, at the same overall, so BBGM's own game engine reproduces his real 2026-27 per-36 stats. This uses a formula calibrated weekly against re-simulations of the season so far (see calibration-report.md). Yearly cap inflation is on (7% average), with 4- and 3-year rookie deals. Each morning, after the night's games, it is updated with real life:
current rosters (trades, signings, releases), injuries with return timelines, standings, **real box scores of every
game played so far**, and the remaining real schedule.

**One click:** [start a new league with today's NBA](https://play.basketball-gm.com/new_league#url=https%3A%2F%2Fraw.githubusercontent.com%2Ftonga43%2Fbbgm-nba-roster%2Fmain%2Fnba-roster-latest.json.gz)

Or in BBGM go to New League, then Customize, then *Enter league file URL*, and paste `https://raw.githubusercontent.com/tonga43/bbgm-nba-roster/main/nba-roster-latest.json.gz`

- `latest-changes.md`: what changed in the latest update
- `build-report.md`: the overlay applied to BBGM's data (moves, contracts, injuries and their sources, rating blend with biggest risers and fallers, draft-board changes)
- `changelog.md`: full history

Sources: BBGM Real Players data, the ESPN public API, Basketball-Reference, and the Tankathon 2027 big board. Each
update is a fresh league; BBGM can't merge it into an existing save.
