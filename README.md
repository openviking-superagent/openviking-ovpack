# OpenViking main 分支 ovpack（OpenViking add-resource 直接入库）

这个包是用 OpenViking 自己的 `add-resource` 直接抓取 GitHub 仓库生成的，不是外部脚本拼出来的：解析、L2 文件摘要、目录 L0/L1、索引记录都由 OpenViking 0.5.0 的 SemanticProcessor 完成，最后用 `ov export` 导出。

## 下载

- 直接下载：[OpenViking.ovpack](https://github.com/openviking-superagent/openviking-ovpack/raw/refs/heads/main/OpenViking.ovpack)
- 或者克隆仓库：

```bash
git clone --depth 1 https://github.com/openviking-superagent/openviking-ovpack.git
```

## 文件

- `OpenViking.ovpack` — 23,005,234 字节（21.94 MiB）
- `SHA256SUMS` — 校验和

SHA-256：

```text
e71143da3580c86ae7392ce9bb545326651810b3bca55c52b555051a58b257db  OpenViking.ovpack
```

## 入库方式

```bash
ov add-resource https://github.com/zrwangems/OpenViking \
  --to viking://resources/OpenViking \
  --processing-mode semantic_and_vectors
```

服务端按仓库导入处理：GitHub ZIP 获取、仓库过滤、文件解析、文件级摘要、目录 L0/L1 聚合、向量索引。本次任务在 OpenViking 0.5.0 上完成，耗时约 10 分钟，SemanticProcessor 处理 5,908 个语义节点，Embedding 处理 6,739 条记录，0 错误。

## 内容

- 源仓库：<https://github.com/zrwangems/OpenViking>
- 分支：`main`
- 提交：`52612416a9946c2b6ba38a869b01f446fbe2746e`
- 内容文件：5,077 个
- 目录：831 个（含仓库根目录）
- L0/L1：每个目录一对 `.abstract.md` / `.overview.md`，共 1,662 个 sidecar
- OVPack 文件总数：6,739 个
- 索引记录：6,739 条（含文件级 L2 摘要）
- 原始仓库：5,718 个文件 / 113.1 MiB
- 过滤：按 `.gitignore`、忽略目录、二进制扩展名、10 MB 单文件上限等规则，过滤掉 641 个文件
- 向量：包内没有 dense vector 快照；导入时由目标端重算

L0/L1 是 OpenViking 的 SemanticProcessor 生成的，不是占位符；摘要语言由 OV 按仓库内容自动判断，这次为英文。导入后可以直接用 `ov abstract` / `ov overview` 读取，`ov tree` 也会显示目录摘要。

## 导入

```bash
ov import OpenViking.ovpack viking://resources --vector-mode recompute
```

导入后的根路径：

```text
viking://resources/OpenViking
```

## 不导入 OV 时查看

OVPack 是 ZIP 容器，可以直接解压：

```bash
unzip OpenViking.ovpack -d OpenViking-unpacked
```

资源文件在 `OpenViking-unpacked/OpenViking/files/`，清单在 `OpenViking-unpacked/OpenViking/_ovpack/manifest.json`，索引记录在 `OpenViking-unpacked/OpenViking/_ovpack/index_records.jsonl`。目录摘要位于各目录下的 `.abstract.md` 和 `.overview.md`。

## 来源与许可

代码和文档来自上述公开仓库，遵循该仓库的许可（AGPL-3.0）。本仓库只存放导出的资源包，不修改原始内容。
