# Manifest 说明

本目录用 [Google repo](https://gerrit.googlesource.com/git-repo) 锁定各上游组件的仓库地址与版本。

## 文件

| 文件 | 说明 |
|---|---|
| `default.xml` | 默认基线：QEMU virt + Linux 6.9 + TF-A v2.14 + OP-TEE 4.5 + U-Boot v2025.01 |
| `qemu-linux61.xml` | 变体：仅 Kernel 切换为 Linux 6.1，其余与默认一致 |

## 常用操作

```bash
# 首次/默认同步
scripts/sync.sh

# 切换 manifest（如 Kernel 6.1）
scripts/sync.sh -m qemu-linux61.xml

# 发布时固化快照（在源码同步后执行）
repo manifest -r -o manifest/frozen-<release>.xml
```

## 约定

- 开发期 `revision` 一律写 **tag**（如 `v6.9.12`），不写分支名，保证任何时刻 `repo sync` 结果一致。
- 升级组件（如 TF-A v2.14 → v2.15）只改一行 revision，然后执行：
  `patch.sh unapply … && sync.sh && patch.sh apply`（见 docs/sdk-directory-spec.md §3.2）。
- 首次同步默认 shallow（`repo sync --depth=1 --no-tags`），调试需要完整历史时加 `FULL_SYNC=1`。
