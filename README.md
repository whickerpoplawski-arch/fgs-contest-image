# fgs-contest-image

Contest agent image for [fgs-v4](https://github.com/whickerpoplawski-arch/fgs-v4).

## Download

Get `agent.tar.gz` from the latest [Release](https://github.com/whickerpoplawski-arch/fgs-contest-image/releases).

**Current:** [v5624fa3-bwrap](https://github.com/whickerpoplawski-arch/fgs-contest-image/releases/tag/v5624fa3-bwrap) — bubblewrap + `danger-full-access` default (fixes hosted `SANDBOX_UNAVAILABLE` / approval gate).

## Integrity

| Field | Value |
|-------|-------|
| File | `agent.tar.gz` |
| Size | ~379 MB |
| SHA256 | `251f3bdccefe135114c0e4bd7788c204b488e3f79a1c74790967fbd85e5ff239` |
| Source tip | [fgs-v4 @ 5624fa3](https://github.com/whickerpoplawski-arch/fgs-v4/commit/5624fa335b6253780b53dd84a6a8794f7a9044f5) (PR#8 `start?unique_code=`) |
| LLM provider | `deepseek-contest` (`openai-completions`) — **not** stock `deepseek-official` (Messages) |
| Sandbox | `DSH_PERMISSION_MODE=danger-full-access` + cordis `sandbox-policy` / `approval: never` |
| bwrap | Debian `bubblewrap` 0.8.0 on PATH (`/usr/bin/bwrap`) |

Verify after download:

```bash
sha256sum agent.tar.gz
# expect: 251f3bdccefe135114c0e4bd7788c204b488e3f79a1c74790967fbd85e5ff239
```

## What changed in v5624fa3-bwrap

Hosted logs: bash → `SANDBOX_UNAVAILABLE` (kernel 5.4, no Landlock, no `bwrap`); escalating to `danger-full-access` needed an approval channel the hosted runner does not have → "未达成" reports → SCORE 0 thrash.

Fixes in this image:
1. **Install `bubblewrap`** so `workspace-write` sandbox can use `bwrap` if needed.
2. **Default `DSH_PERMISSION_MODE=danger-full-access`** (Dockerfile ENV) so bash runs without sandbox fences and without approval prompts. Contest `cordis.patch.yml` also pins `sandbox-policy.mode: danger-full-access` and `approval.policy: never` if the platform clears undeclared env.

Prior: [v5624fa3-llmfix](https://github.com/whickerpoplawski-arch/fgs-contest-image/releases/tag/v5624fa3-llmfix) fixed Messages 404 via provider rename.

## Upload env (platform Agent page)

Fill **only** these two:

| KEY | VALUE |
|-----|-------|
| `DEEPSEEK_API_KEY` | your DeepSeek API key |
| `SOLVER_GATEWAY` | `1` |

Do **not** set `BENCHMARK_*` vars. Image already includes `CONTEST_DSH_LAUNCH=1`, `DSH_PERMISSION_MODE=danger-full-access`, and related contest defaults.
