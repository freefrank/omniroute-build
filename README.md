# omniroute-build

[简体中文](README.zh-CN.md)

Daily Docker image of [OmniRoute](https://github.com/diegosouzapw/OmniRoute) built from the head of upstream's default branch.

Upstream's `:next` image rarely publishes because each push cancels the previous build. This repository builds the current head once a day (07:17 UTC) with the same Dockerfile target (`runner-base`) and build args as upstream's `:next`, starts it offline with an empty data directory, checks `/api/health`, `/api/health/ping` and `/login`, and only then pushes:

- `ghcr.io/freefrank/omniroute:edge`: the latest build that passed the smoke test
- `ghcr.io/freefrank/omniroute:sha-<12 chars>`: one tag per upstream commit, for pinning or rollback

A commit that already has an image is skipped. Run the workflow by hand with `force` to rebuild.

The image carries `org.opencontainers.image.revision` with the upstream commit. Nothing in this repository changes OmniRoute's source.
