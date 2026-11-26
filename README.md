# omniroute-build

[简体中文](README.zh-CN.md)

Daily Docker image of [OmniRoute](https://github.com/diegosouzapw/OmniRoute) built from the head of upstream's default branch.

Upstream's `:next` image rarely publishes because each push cancels the previous build. This repository builds the current head once a day (07:17 UTC) with the same Dockerfile target (`runner-base`) and build args as upstream's `:next`, applies the patches in [`patches/`](patches), starts it offline with an empty data directory, checks `/api/health`, `/api/health/ping` and `/login`, and only then pushes:

- `ghcr.io/freefrank/omniroute:edge`: the latest build that passed the smoke test
- `ghcr.io/freefrank/omniroute:sha-<12 chars>-p<8 chars>`: one tag per upstream commit and patch set, for pinning or rollback (`sha-<12 chars>` when there are no patches)

A commit that already has an image with the current patch set is skipped. Run the workflow by hand with `force` to rebuild, and with `ref` to build a specific upstream commit (for example the last one that built when upstream's head is broken).

The image carries `org.opencontainers.image.revision` with the upstream commit; `org.opencontainers.image.version` ends in `+p<patch set>` when patches were applied.

## Patches

Applied in order with `git apply`. If one stops applying, the build fails and `:edge` stays on the last good image; drop or refresh the patch once upstream changes the code.

None at the moment.

`0001-mid-conversation-system-without-client-beta.patch` (2026-10-29 to 2026-11-25) let `anthropic-compatible-*` relays keep mid-conversation `role: "system"` turns in place without the client's `mid-conversation-system-2026-04-07` beta. Upstream PR #18341 replaced it with the per-connection setting `keepMidConversationSystem`; turn it on for relays that accept that shape (CLIProxyAPI does).
