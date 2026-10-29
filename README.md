# omniroute-build

[简体中文](README.zh-CN.md)

Daily Docker image of [OmniRoute](https://github.com/diegosouzapw/OmniRoute) built from the head of upstream's default branch.

Upstream's `:next` image rarely publishes because each push cancels the previous build. This repository builds the current head once a day (07:17 UTC) with the same Dockerfile target (`runner-base`) and build args as upstream's `:next`, applies the patches in [`patches/`](patches), starts it offline with an empty data directory, checks `/api/health`, `/api/health/ping` and `/login`, and only then pushes:

- `ghcr.io/freefrank/omniroute:edge`: the latest build that passed the smoke test
- `ghcr.io/freefrank/omniroute:sha-<12 chars>-p<8 chars>`: one tag per upstream commit and patch set, for pinning or rollback

A commit that already has an image with the current patch set is skipped. Run the workflow by hand with `force` to rebuild, and with `ref` to build a specific upstream commit (for example the last one that built when upstream's head is broken).

The image carries `org.opencontainers.image.revision` with the upstream commit; `org.opencontainers.image.version` ends in `+p<patch set>`.

## Patches

Applied in order with `git apply`. If one stops applying, the build fails and `:edge` stays on the last good image; drop or refresh the patch once upstream changes the code.

- `0001-mid-conversation-system-without-client-beta.patch`: with the feature flag `ANTHROPIC_COMPATIBLE_MID_CONVERSATION_SYSTEM` on, `anthropic-compatible-*` relays keep mid-conversation `role: "system"` turns in place even when the client did not send the `mid-conversation-system-2026-04-07` beta. Current Claude Code no longer sends that beta (Anthropic made the feature generally available), so upstream's flag had no effect and the `<total_tokens>` turn Claude Code adds each turn was hoisted into `system`, breaking the prompt cache after the system prompt on every request.
