# Changelog

## 2026-11-26

- Drop `0001-mid-conversation-system-without-client-beta.patch`: upstream PR #18341 replaced the gate with the per-connection `keepMidConversationSystem` setting, and the patch no longer applied, so every daily build since 2026-11-05 failed.
- An empty `patches/` builds again; the tag is then `sha-<commit>` and the version label has no `+p` suffix.

## 2026-10-29

- `workflow_dispatch` input `ref` builds a given upstream commit instead of the default branch head.
- Apply the patches in `patches/` before building; image tags become `sha-<commit>-p<patch set>`.
- Add `0001-mid-conversation-system-without-client-beta.patch`: `ANTHROPIC_COMPATIBLE_MID_CONVERSATION_SYSTEM` no longer needs the client to send the `mid-conversation-system-2026-04-07` beta, which current Claude Code never sends.

## 2026-10-19

- Daily build of upstream's default branch head, smoke test, push to `ghcr.io/freefrank/omniroute:edge` and `:sha-<commit>`.
