# 脚本

*脚本*是类似于传统语言中的主函数的可执行入口点。脚本通常会调用已发布模块的函数，这些函数对全局存储执行更新。脚本是不会在全局存储中发布的临时代码片段。

一个 Move 源文件（或**编译单元**）可能包含多个模块和脚本。但是，发布模块或执行脚本是单独的 VM 操作。

**语法**

脚本具有以下结构：

<!-- # Scripts

_Scripts_ are executable entrypoints similar to a `main` function in a conventional language. A script typically calls functions of a published module that perform updates to global storage. Scripts are ephemeral code snippets that are not published in global storage.

A Move source file (or **compilation unit**) may contain multiple modules and scripts. However, publishing a module or executing a script are separate VM operations.

**Syntax**

A script has the following structure: -->

```rust
script {
    <use>*
    <constants>*
    fun <identifier><[type parameters: constraint]*>([identifier: type]*) <function_body>
}
```

`script` 块必须以其所有 `use` 声明开始，然后是任何 `constants` 和（最后）main `function` 声明。主函数可以有任何名称（即，它不需要称为 `main`），是脚本块中的唯一函数，可以有任意数量的参数，并且不得返回值。以下是每个组件的示例：

<!-- A `script` block must start with all of its `use` declarations, followed by any `constants` and (finally) the main `function` declaration. The main function can have any name (i.e., it need not be called `main`), is the only function in a script block, can have any number of arguments, and must not return a value. Here is an example with each of these components: -->

```rust
script {
    // Import the debug module published at the named account address std.
    use std::debug;

    const ONE: u64 = 1;

    fun main(x: u64) {
        let sum = x + ONE;
        debug::print(&sum)
    }
}
```

脚本的功能非常有限——它们不能声明友元、结构类型或访问全局存储。它们的主要目的是调用模块函数。

<!-- Scripts have very limited power—they cannot declare friends, struct types or access global storage. Their primary purpose is to invoke module functions. -->
