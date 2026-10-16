# fgs-contest-image

Contest agent image for [fgs-v4](https://github.com/whickerpoplawski-arch/fgs-v4).

## Download

Get `agent.tar.gz` from the latest [Release](https://github.com/whickerpoplawski-arch/fgs-contest-image/releases).

**Current:** [v3958259-close-resilience](https://github.com/whickerpoplawski-arch/fgs-contest-image/releases/tag/v3958259-close-resilience) — **PR#11**: Challenges close/list/start timeout no longer kills the driver loop (keeps SCORE pipeline alive). Includes **PR#10** `verifyMode: command` + `FGS_UNIQUE_CODE`.

## Integrity

| Field | Value |
|-------|-------|
| File | `agent.tar.gz` |
| Size | ~380M |
| SHA256 | `cb6e4049df8ca187318b7f802116e16d4b22c50a187f180fa68c5bfcb86a7052` |
| Source tip | [fgs-v4 @ 3958259](https://github.com/whickerpoplawski-arch/fgs-v4/commit/39582590249a4798034342b3544822af7190ebbc) (PR#11 close resilience + PR#10 submit) |
| verifyMode | **command** (`node ./tools/submit_flag.mjs` → POST `/openapi/v1/challenges/submit`) |
| Close resilience | TimeoutError/OSError on close/list/start → log + continue; driver main loop stays up |
| LLM provider | `deepseek-contest` (`openai-completions`) — **not** stock `deepseek-official` (Messages) |
| Sandbox | `DSH_PERMISSION_MODE=danger-full-access` + cordis `sandbox-policy` / `approval: never` |
| bwrap | Debian `bubblewrap` 0.8.0 on PATH (`/usr/bin/bwrap`) |

Verify after download:

```bash
sha256sum agent.tar.gz
# expect: cb6e4049df8ca187318b7f802116e16d4b22c50a187f180fa68c5bfcb86a7052
```

## What changed in v3958259-close-resilience

After ~1e4 SCORE, the hosted agent went flat because `perform_close` → `urlopen` raised bare `TimeoutError` and crashed the driver main loop. No more launch/submit after that.

Fixes in this image (fgs-v4 PR#11 @ 3958259):
1. **Close/list/start timeouts are non-fatal** — caught, logged, driver keeps ticking.
2. Keeps **PR#10** `verifyMode: command` + `submit_flag.mjs` + per-challenge `FGS_UNIQUE_CODE`.
3. Keeps bubblewrap + `danger-full-access` + `deepseek-contest` from prior releases.

Prior: [vf50c2a0-submit](https://github.com/whickerpoplawski-arch/fgs-contest-image/releases/tag/vf50c2a0-submit) (SCORE gap), [v5624fa3-bwrap](https://github.com/whickerpoplawski-arch/fgs-contest-image/releases/tag/v5624fa3-bwrap) (sandbox), [v5624fa3-llmfix](https://github.com/whickerpoplawski-arch/fgs-contest-image/releases/tag/v5624fa3-llmfix) (Messages 404).

## Upload env (platform Agent page)

Fill **only** these two:

| KEY | VALUE |
|-----|-------|
| `DEEPSEEK_API_KEY` | your DeepSeek API key |
| `SOLVER_GATEWAY` | `1` |

Do **not** set `BENCHMARK_*` vars. Do **not** set a global `FGS_UNIQUE_CODE` on the upload page (driver injects per challenge). Image already includes `CONTEST_DSH_LAUNCH=1`, `DSH_PERMISSION_MODE=danger-full-access`, and related contest defaults.
