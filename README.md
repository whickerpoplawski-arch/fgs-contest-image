# fgs-contest-image

Contest agent image for [fgs-v4](https://github.com/whickerpoplawski-arch/fgs-v4).

## Download

Get `agent.tar.gz` from the latest [Release](https://github.com/whickerpoplawski-arch/fgs-contest-image/releases).

**Current:** [vf50c2a0-submit](https://github.com/whickerpoplawski-arch/fgs-contest-image/releases/tag/vf50c2a0-submit) — **PR#10**: `verifyMode: command` + `submit_flag.mjs` + `FGS_UNIQUE_CODE` (fixes SCORE not counting after local regex "goal_final 已满足").

## Integrity

| Field | Value |
|-------|-------|
| File | `agent.tar.gz` |
| Size | ~380M |
| SHA256 | `1727cc1fc50af767a2aebbb9283c7cddf2f85e81d6326955c6dd882e10fbdd8b` |
| Source tip | [fgs-v4 @ f50c2a0](https://github.com/whickerpoplawski-arch/fgs-v4/commit/f50c2a0f22f9a3b2a0ee8ec9285228c20d64d9da) (PR#10 submit fix) |
| verifyMode | **command** (`node ./tools/submit_flag.mjs` → POST `/openapi/v1/challenges/submit`) |
| LLM provider | `deepseek-contest` (`openai-completions`) — **not** stock `deepseek-official` (Messages) |
| Sandbox | `DSH_PERMISSION_MODE=danger-full-access` + cordis `sandbox-policy` / `approval: never` |
| bwrap | Debian `bubblewrap` 0.8.0 on PATH (`/usr/bin/bwrap`) |

Verify after download:

```bash
sha256sum agent.tar.gz
# expect: 1727cc1fc50af767a2aebbb9283c7cddf2f85e81d6326955c6dd882e10fbdd8b
```

## What changed in vf50c2a0-submit

Platform SCORE stayed at 250 with only 1 "得分成功" while the harness logged many `goal_final 已满足`. Root cause: image defaulted to `verifyMode: regex` — local flag-shape match only, **never** POST to Challenges submit.

Fixes in this image (fgs-v4 PR#10 @ f50c2a0):
1. **Default `verifyMode: command`** + `verifyCommand: node ./tools/submit_flag.mjs` in plugin `cordis.patch.yml`.
2. **Worker injects `FGS_UNIQUE_CODE`** (same id as start/close) so submit binds the right challenge under concurrency.
3. Keeps bubblewrap + `danger-full-access` + `deepseek-contest` from prior releases.

Prior: [v5624fa3-bwrap](https://github.com/whickerpoplawski-arch/fgs-contest-image/releases/tag/v5624fa3-bwrap) (sandbox), [v5624fa3-llmfix](https://github.com/whickerpoplawski-arch/fgs-contest-image/releases/tag/v5624fa3-llmfix) (Messages 404).

## Upload env (platform Agent page)

Fill **only** these two:

| KEY | VALUE |
|-----|-------|
| `DEEPSEEK_API_KEY` | your DeepSeek API key |
| `SOLVER_GATEWAY` | `1` |

Do **not** set `BENCHMARK_*` vars. Do **not** set a global `FGS_UNIQUE_CODE` on the upload page (driver injects per challenge). Image already includes `CONTEST_DSH_LAUNCH=1`, `DSH_PERMISSION_MODE=danger-full-access`, and related contest defaults.
