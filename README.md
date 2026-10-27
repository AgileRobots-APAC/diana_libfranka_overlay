# Diana libfranka overlay

[中文文档](README.cn.md)

This repository carries machine-specific patches on top of the upstream
`libfranka` `0.18.2` source. The `libfranka` submodule is pinned to the upstream
tag recorded in the `VERSION` file (`0.18.2`).

## Branches

| Branch | Patch | Target arm |
|---|---|---|
| `diana3` | `patches/diana3.patch` | Diana3 |
| `diana7` | `patches/diana7.patch` | Diana7 |
| `diana7g2` | `patches/diana7g2.patch` | Diana7G2 |

Each branch contains exactly one target-arm patch. Applying it selects the
matching rate-limiting header, configures the matching C++ and Python examples,
and retains the upstream library implementation unchanged.

## Use

```bash
git clone --recurse-submodules <overlay-repository-url>
cd diana_libfranka_overlay
git switch diana3
git submodule update --init --recursive
git -C libfranka apply --check ../patches/diana3.patch
git -C libfranka apply ../patches/diana3.patch
```

Then open the `libfranka` directory in VS Code and choose **Reopen in
Container**. The Dev Container mounts that directory at `/workspaces` and
already contains the build dependencies. In its terminal, build from the
container workspace root:

```bash
cmake -S . -B build -DBUILD_EXAMPLES=ON
cmake --build build
```

If building on the host instead, remain at the overlay repository root and use
`cmake -S libfranka -B build/libfranka -DBUILD_EXAMPLES=ON`, followed by
`cmake --build build/libfranka`.

The patch is valid only when `libfranka` is clean and at the pinned commit.
After applying it, a dirty submodule is expected. To change machine type,
discard the submodule's applied changes, check out the other overlay branch,
update the submodule, and apply that branch's patch.

## Version

This is release **1.0.0**. It belongs to software release 3.3.0 (SDK 3.3.0) and applies to the upstream `libfranka` `0.18.2` source pinned above.

| Tag | Points to |
|---|---|
| `v1.0.0-libfranka0.18.2-diana3` | `diana3` |
| `v1.0.0-libfranka0.18.2-diana7` | `diana7` |
| `v1.0.0-libfranka0.18.2-diana7g2` | `diana7g2` |

The upstream `libfranka` version used for packaging is kept in the `VERSION` file at the repository root.

Check out a tag instead of a branch when you need a reproducible baseline, for example `git switch --detach v1.0.0-libfranka0.18.2-diana3`.

## License

The patches and example sources in this repository are licensed under the Apache License, Version 2.0 (see [`LICENSE`](LICENSE)), the same license as upstream `libfranka`. The `libfranka` submodule is not part of this repository and keeps its own license.
