# fgs-contest-image

Contest agent image for [fgs-v4](https://github.com/whickerpoplawski-arch/fgs-v4).

## Download

Get `agent.tar.gz` from the latest [Release](https://github.com/whickerpoplawski-arch/fgs-contest-image/releases).

## Integrity

| Field | Value |
|-------|-------|
| File | `agent.tar.gz` |
| Size | ~380 MB |
| SHA256 | `6a5c99c872ef911349e2b152f5af1edfb60ad42a325128c611bdcfb60bd8ebb1` |
| Source tip | [fgs-v4 @ 5624fa3](https://github.com/whickerpoplawski-arch/fgs-v4/commit/5624fa335b6253780b53dd84a6a8794f7a9044f5) (PR#8 `start?unique_code=`) |

Verify after download:

```bash
sha256sum agent.tar.gz
# expect: 6a5c99c872ef911349e2b152f5af1edfb60ad42a325128c611bdcfb60bd8ebb1
```

## Upload env (platform Agent page)

Fill **only** these two:

| KEY | VALUE |
|-----|-------|
| `DEEPSEEK_API_KEY` | your DeepSeek API key |
| `SOLVER_GATEWAY` | `1` |

Do **not** set `BENCHMARK_*` vars. Image already includes `CONTEST_DSH_LAUNCH=1` and related contest defaults.
