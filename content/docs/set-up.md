---
title: 设置
weight: 2
---

# 设置

> 安装 IDE 和环境设置。

**安装 Move IDE**

要安装它，你需要：

1. VSCode（版本 1.43.0 及以上）——你可以[从这里获取](https://code.visualstudio.com/download)；如果您已经安装 - 继续下一步；
2. Move-Analyzer IDE - 安装 VSCode 后，按照[这个链接](https://marketplace.visualstudio.com/items?itemName=move.move-analyzer)安装最新版本的 IDE。

**安装 Aptos 命令行界面（CLI）**

你可以[从这里获取](https://aptos.dev/tools/install-cli/)。

**使用 Aptos CLI 运行代码**

首先你的文件夹结构应该是这样的：

```sh
move_project
    |-sources
        |-first.move
    |Move.toml
```

**first.move**：

```rust
module my_addrx::Sample
{
    use std::debug;

    fun sample_function()
    {
        debug::print(&12345);
    }

    #[test]
    fun testing()
    {
        sample_function();
    }
}
```

**Move.toml**：

```toml
[package]
name = "move_project"
version = "0.0.0"

[dependencies]
AptosFramework = { git = "https://github.com/aptos-labs/aptos-core.git", subdir = "aptos-move/framework/aptos-framework", rev = "mainnet" }

[addresses]
my_addrx = "0x42"
std = "0x1"
```

**编译模块**：

在 VSCode 终端运行 `aptos move compile` 来编译模块。

**测试模块**：

在 VSCode 终端运行 `aptos move test` 来运行单元测试。

> 查看[这里获取](https://aptos.dev/cli-tools/aptos-cli-tool/use-aptos-cli/)有关 Aptos CLI 的更多信息。
