# Diana libfranka overlay

[English](README.md)

此仓库在上游 `libfranka` `0.18.2` 源码的基础上维护面向特定机械臂的补丁。`libfranka` 子模块固定在 `VERSION` 文件所记录的上游 tag（`0.18.2`）。

## 分支

| 分支 | 补丁 | 目标机械臂 |
|---|---|---|
| `diana3` | `patches/diana3.patch` | Diana3 |
| `diana7` | `patches/diana7.patch` | Diana7 |
| `diana7g2` | `patches/diana7g2.patch` | Diana7G2 |

每个分支只包含一个面向目标机械臂的补丁。应用补丁后，会选用相应的速率限制头文件、配置匹配的 C++ 和 Python 示例，而上游库的实现保持不变。

## 使用方法

```bash
git clone --recurse-submodules <overlay-repository-url>
cd diana_libfranka_overlay
git switch diana3
git submodule update --init --recursive
git -C libfranka apply --check ../patches/diana3.patch
git -C libfranka apply ../patches/diana3.patch
```

然后在 VS Code 中打开 `libfranka` 目录，选择 **Reopen in Container**。Dev Container 会将该目录挂载到 `/workspaces`，并已包含构建所需的依赖。在其终端中，从容器工作区根目录构建：

```bash
cmake -S . -B build -DBUILD_EXAMPLES=ON
cmake --build build
```

如果改为在主机上构建，请保持在 overlay 仓库根目录，使用 `cmake -S libfranka -B build/libfranka -DBUILD_EXAMPLES=ON`，然后执行 `cmake --build build/libfranka`。

补丁仅在 `libfranka` 子模块处于干净状态且位于固定提交时有效。应用后，子模块处于脏状态是预期行为。如需切换机械臂型号，请先丢弃子模块中已应用的修改，再切换到另一个 overlay 分支，更新子模块，然后应用该分支的补丁。

## 版本

当前为 **1.0.0** 版本，属于软件版本 3.3.0（SDK 3.3.0），适配上文固定的上游 `libfranka` `0.18.2` 源码。

| Tag | 指向 |
|---|---|
| `v1.0.0-libfranka0.18.2-diana3` | `diana3` |
| `v1.0.0-libfranka0.18.2-diana7` | `diana7` |
| `v1.0.0-libfranka0.18.2-diana7g2` | `diana7g2` |

打包使用的上游 `libfranka` 版本记录在仓库根目录的 `VERSION` 文件中。

需要可复现的基线时，请检出 tag 而不是分支，例如 `git switch --detach v1.0.0-libfranka0.18.2-diana3`。

## 许可证

本仓库中的补丁和示例源码采用 Apache License 2.0（见 [`LICENSE`](LICENSE)），与上游 `libfranka` 相同。`libfranka` 子模块不属于本仓库，沿用其自身的许可证。
