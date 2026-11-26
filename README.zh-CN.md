# omniroute-build

[English](README.md)

每天从 [OmniRoute](https://github.com/diegosouzapw/OmniRoute) 上游默认分支的最新提交构建一份 Docker 镜像。

上游的 `:next` 镜像很少能发布出来：每推一次代码就会取消上一次构建。这个仓库每天 07:17 UTC 构建一次当前最新提交，构建目标（`runner-base`）和参数与上游 `:next` 相同，并打上 [`patches/`](patches) 里的补丁；先在断网、空数据目录的容器里检查 `/api/health`、`/api/health/ping` 和 `/login`，通过后才推送：

- `ghcr.io/freefrank/omniroute:edge`：最近一次通过检查的构建
- `ghcr.io/freefrank/omniroute:sha-<12 位>-p<8 位>`：每个上游提交加补丁组合一个标签，用来固定版本或回滚（没有补丁时是 `sha-<12 位>`）

同一个上游提交、同一组补丁已经构建过的会跳过。需要重建时手动运行工作流并勾选 `force`；填 `ref` 可以构建指定的上游提交（比如上游最新提交构建失败时，退回上一个能构建的提交）。

镜像的 `org.opencontainers.image.revision` 标签记录了上游提交，`org.opencontainers.image.version` 末尾的 `+p<补丁组>` 记录了补丁（没打补丁时没有这一段）。

## 补丁

用 `git apply` 按顺序打上。某个补丁打不上时构建会失败，`:edge` 保持在上一个好的镜像；上游改了相关代码后，删掉或更新这个补丁。

目前没有。

`0001-mid-conversation-system-without-client-beta.patch`（2026-10-29 至 2026-11-25）让 `anthropic-compatible-*` 中转在客户端不带 `mid-conversation-system-2026-04-07` beta 时，也把对话中间的 `role: "system"` 消息留在原位。上游 PR #18341 改成了按连接设置的 `keepMidConversationSystem`；能接受这种格式的中转（CLIProxyAPI 可以）把它打开即可。
