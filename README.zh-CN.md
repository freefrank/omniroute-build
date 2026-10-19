# omniroute-build

[English](README.md)

每天从 [OmniRoute](https://github.com/diegosouzapw/OmniRoute) 上游默认分支的最新提交构建一份 Docker 镜像。

上游的 `:next` 镜像很少能发布出来：每推一次代码就会取消上一次构建。这个仓库每天 07:17 UTC 构建一次当前最新提交，构建目标（`runner-base`）和参数与上游 `:next` 相同；先在断网、空数据目录的容器里检查 `/api/health`、`/api/health/ping` 和 `/login`，通过后才推送：

- `ghcr.io/freefrank/omniroute:edge`：最近一次通过检查的构建
- `ghcr.io/freefrank/omniroute:sha-<12 位>`：每个上游提交一个标签，用来固定版本或回滚

已经构建过的提交会跳过。需要重建时手动运行工作流并勾选 `force`。

镜像的 `org.opencontainers.image.revision` 标签记录了上游提交。这个仓库不改 OmniRoute 的源码。
