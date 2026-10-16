# the-siraaj fork

Upstream: [AlexandrErohin/home-assistant-flightradar24](https://github.com/AlexandrErohin/home-assistant-flightradar24).
This fork exists only to carry a rate-limit fix until upstream ships one. Branch `siraaj` = upstream
release tag + the commits below; releases are tagged `vX.Y.Z-siraaj.N` so HACS offers them.

| Change | Why | Upstream |
|---|---|---|
| Per-endpoint circuit breaker after a 429, 429 logged at debug | Without it every scan burns 3 rejected requests and FR24 never lifts the throttle | PR [#333](https://github.com/AlexandrErohin/home-assistant-flightradar24/pull/333) (cherry-picked as-is) |
| `FlightRadarAPI.api.FEED_EMPTY_RETRIES = 0` (`api/client.py`) | An empty small-radius box cost 5 feed requests per scan | Issue [#330](https://github.com/AlexandrErohin/home-assistant-flightradar24/issues/330) |

## Retire the fork

When an upstream release contains both fixes: in HACS remove this custom repository, re-add the
official one, download its latest release, restart Home Assistant, then archive this repo. The weekly
Home Assistant health digest (the-siraaj/homeassistant) reports when upstream has a newer release.

## Rebase on a new upstream release

```bash
git fetch upstream --tags
git rebase --onto vNEW v2.3.0 siraaj        # replay the fork commits
# bump custom_components/flightradar24/manifest.json version to vNEW-siraaj.1, run pytest
git push --force-with-lease origin siraaj
gh release create vNEW-siraaj.1 --target siraaj --title vNEW-siraaj.1 --notes "Upstream vNEW + fork fixes (FORK.md)"
```
