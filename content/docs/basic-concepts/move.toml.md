# Move.toml

`Move.toml` 文件是一个清单文件，其中包含包的名称、版本和依赖项等元数据。

清单本身包含许多部分，其中主要是：

- `[package]` - 包括包元数据，例如名称和版本
- `[dependencies]` - 指定项目的依赖项
- `[addresses]` - 地址别名

最小包源目录结构如下所示：

```
package_name 
     |-Move.toml 
     |-sources 
          |- example.move
```

**Move.toml**

```toml
[package]
name = "aptos_by_examples"
version = "0.0.0"

[dependencies]
AptosFramework = { git = "https://github.com/aptos-labs/aptos-core.git", subdir = "aptos-move/framework/aptos-framework", rev = "mainnet" }

[addresses]
my_addrx = "0x42"
std = "0x1"
```

<!-- # Move.toml

A `Move.toml` file is a manifest file that contains metadata such as name, version, and dependencies for the package.

The manifest itself contains a number of sections, primary of which are:

* `[package]`- includes package metadata such as name and version
* `[dependencies]` - specifies dependencies of the project
* `[addresses]` - address aliases

The minimal package source directory structure looks as follows:

```
package_name 
     |-Move.toml 
     |-sources 
          |- example.move
```

**Move.toml**

```toml
[package]
name = "aptos_by_examples"
version = "0.0.0"

[dependencies]
AptosFramework = { git = "https://github.com/aptos-labs/aptos-core.git", subdir = "aptos-move/framework/aptos-framework", rev = "mainnet" }

[addresses]
my_addrx = "0x42"
std = "0x1"
``` -->
