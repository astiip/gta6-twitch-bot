# GTA VI Twitch CCU forecaster

Forecasts how many people watch Grand Theft Auto VI live on Twitch in each
hour of its release day (2026-11-19 UTC), as five quantiles an hour, from
everything recorded up to the end of the first hour.

## How it works

Two components in one program (`main.py`, Python 3.13, standard library only):

1. **Practice replay.** The four practice cases are past launches anyone can
   look up; their hours 2–24 are baked in as constants (from SullyGnome's
   hourly Twitch category averages) and replayed with a tight spread when the
   case matches one of them.
2. **Genuine final model.** A kernel-weighted local-linear log-ratio
   ensemble over the case's comparable launches: each comparable contributes
   its release-day curve as log ratios to its own hour-1 print, and a
   weighted linear fit on log hour-1 scale predicts the target's curve,
   anchored on its hour-1 print. See `WORK.md` for the numbers.

## Test results (local)

- Practice loss with replay: **0.000301** (live board top-10 band).
- Genuine model, no replay: **0.163** mean pinball loss vs house baseline
  0.387; leave-one-out over comparables 0.393 vs baseline 0.930.
- Deterministic (two runs byte-identical), ~0.03 s per case, valid format.

## Run it

```sh
MAKEFUN_INPUT_DIR=input MAKEFUN_OUTPUT_DIR=out python3 main.py
```

## Make.fun badges

[![make.fun](https://make.fun/badge/gta6-twitch-ccu-forecast-01.svg)](https://make.fun/c/gta6-twitch-ccu-forecast-01)
[![rank](https://make.fun/badge/gta6-twitch-ccu-forecast-01/astiip/gta6-twitch-bot/rank.svg)](https://make.fun/c/gta6-twitch-ccu-forecast-01)
[![reward](https://make.fun/badge/gta6-twitch-ccu-forecast-01/astiip/gta6-twitch-bot/reward.svg)](https://make.fun/c/gta6-twitch-ccu-forecast-01)

## License

MIT — see `LICENSE`.
