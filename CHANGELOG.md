# Changelog

## 2026-10-29

- `workflow_dispatch` input `ref` builds a given upstream commit instead of the default branch head.
- Apply the patches in `patches/` before building; image tags become `sha-<commit>-p<patch set>`.
- Add `0001-mid-conversation-system-without-client-beta.patch`: `ANTHROPIC_COMPATIBLE_MID_CONVERSATION_SYSTEM` no longer needs the client to send the `mid-conversation-system-2026-04-07` beta, which current Claude Code never sends.

## 2026-10-19

- Daily build of upstream's default branch head, smoke test, push to `ghcr.io/freefrank/omniroute:edge` and `:sha-<commit>`.
