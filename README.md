# World Cup 2026 predictions

Daily, timestamped forecasts for the 2026 FIFA World Cup from [uanalyse](https://uanalyse.co.uk).
Every file here is published before the matches it covers, so anyone can check how our model did
once the results are in.

## Why this repo is public

We wanted a clean way to show our forecasts hold up, with no hindsight. Each day we commit a fresh
snapshot. The snapshots under `data/history/` are append-only and never edited, and every commit is
signed and timestamped by GitHub. So you can see exactly what we predicted, and when, before a ball
was kicked.

## How to verify

- Browse `data/history/<date>/` for the forecast we published on that day.
- Check the commit log and signatures:

  ```bash
  git log --show-signature -- data/history/
  ```

  Each daily commit carries a verified signature and the public timestamp GitHub recorded. Files
  under `data/history/` are never changed after they land, so the git history is the full,
  tamper-evident record.

## What's inside

`data/latest/` always holds the most recent snapshot. `data/history/<date>/` keeps every past one.

### `tournament_probabilities.csv`

One row per team: the chance of reaching each stage.

| column | meaning |
| --- | --- |
| `snapshot_date` | the model snapshot the row comes from |
| `team` | team name |
| `group` | group |
| `prob_win_group` | win the group |
| `prob_runner_up` | finish second in the group |
| `prob_reach_round_of_32` | reach the round of 32 |
| `prob_reach_quarterfinals` | reach the quarterfinals |
| `prob_reach_semifinals` | reach the semifinals |
| `prob_reach_final` | reach the final |
| `prob_champion` | win the tournament |

### `match_predictions.csv`

Every scheduled fixture, with our result probabilities and expected goals. Before the
tournament that's the 72 group games. Knockout fixtures are added here as the bracket fills in
and each tie becomes real.

| column | meaning |
| --- | --- |
| `snapshot_date` | the forecast snapshot the row comes from |
| `kickoff_date` | fixture date |
| `stage` | round |
| `fixture_type` | `group`, and `knockout` once those ties are set |
| `home_team`, `away_team` | the two sides |
| `prob_meeting` | chance this exact tie happens (`1.0` for scheduled fixtures) |
| `prob_home_win`, `prob_draw`, `prob_away_win` | 1X2 result probabilities |
| `exp_home_goals`, `exp_away_goals` | expected goals |

#### Why a team can have more expected goals yet a lower chance of winning

The 1X2 probabilities and the expected goals come from separate parts of the model rather than one
shared goal distribution. Expected goals is the average a side scores across all the simulated
matches. The result probabilities depend on the full spread of those outcomes, not the average
alone. So now and then a side can carry the higher expected goals while still being the underdog to
win. That usually happens when its goals are lumpy: it gets blown out in some simulations, which
lifts its average, but it loses the typical close game. Small gaps like this are normal and aren't a
contradiction.

## How it's made

The figures come from a pre-tournament Monte-Carlo forecast of 10,000 simulated tournaments, run
through our match model. We simulate the group stage, then solve the knockout bracket exactly with
dynamic programming, so the knockout probabilities are computed rather than sampled. Match numbers
are the model's 1X2 estimate for each fixture. The live, round-by-round version is on our
[World Cup 2026 portal](https://uanalyse.co.uk/world-cup-2026). The model itself is proprietary, so
this repo publishes the outputs only.

## Cadence

Updated about once a day through the tournament.

## Disclaimer

This is for information and entertainment. It isn't betting advice.

## License

Data is released under [CC BY 4.0](LICENSE). Please credit uanalyse and link back to
https://uanalyse.co.uk.
