# fgs-contest-image

Contest agent image for [fgs-v4](https://github.com/whickerpoplawski-arch/fgs-v4).

## Download

Get `agent.tar.gz` from the latest [Release](https://github.com/whickerpoplawski-arch/fgs-contest-image/releases).

**Current:** [v5624fa3-llmfix](https://github.com/whickerpoplawski-arch/fgs-contest-image/releases/tag/v5624fa3-llmfix) — provider rename fix (`deepseek-contest` + `openai-completions`).

## Integrity

| Field | Value |
|-------|-------|
| File | `agent.tar.gz` |
| Size | ~346 MB |
| SHA256 | `b24b2508abc5a2870cac7747851d90dd67d14a241e86c90a9ebb3963748ca54f` |
| Source tip | [fgs-v4 @ 5624fa3](https://github.com/whickerpoplawski-arch/fgs-v4/commit/5624fa335b6253780b53dd84a6a8794f7a9044f5) (PR#8 `start?unique_code=`) |
| LLM provider | `deepseek-contest` (`openai-completions`) — **not** stock `deepseek-official` (Messages) |

Verify after download:

```bash
sha256sum agent.tar.gz
# expect: b24b2508abc5a2870cac7747851d90dd67d14a241e86c90a9ebb3963748ca54f
```

## What changed in v5624fa3-llmfix

Hosted logs showed: `configurable provider "deepseek-official" is already declared` → our `openai-completions` patch never activated → stock Messages API → HTTP 404 → worker death / SCORE thrash / DeepSeek usage 0.

Fix: unique provider id `deepseek-contest` (same pattern as practice `scienceedu-deepseek`). Upload env unchanged.

## Upload env (platform Agent page)

Fill **only** these two:

| KEY | VALUE |
|-----|-------|
| `DEEPSEEK_API_KEY` | your DeepSeek API key |
| `SOLVER_GATEWAY` | `1` |

Do **not** set `BENCHMARK_*` vars. Image already includes `CONTEST_DSH_LAUNCH=1` and related contest defaults.
