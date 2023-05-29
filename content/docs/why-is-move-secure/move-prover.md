# Move prover

Move prover 是一种工具，用于验证用 Move 编程语言编写的智能合约的正确性。Move prover 通过数学证明代码的正确性，帮助确保这些智能合约的行为正确。

Move prover 的工作原理是自动验证智能合约代码是否符合某些属性，例如不允许整数溢出或下溢。它还检查代码是否存在某些类型的错误，例如重入漏洞和被零除错误。如果 Move 证明者发现任何违规行为，它会将其报告给开发人员，允许他们在部署之前修复代码。
Move Prover 的存在是为了让合约更 *值得信赖*；它：

- 保护 Aptos 区块链管理的大量资产免受智能合约漏洞的影响
- 抵御资源充足的对手
- 预期合理的监管审查和合规要求
- 允许具有数学背景但不一定具有软件工程背景的领域专家了解智能合约的作用

> 目前，Move Prover 不支持 Windows。

```rust
module my_addrx::Example{
    struct Counter has key {
        value: u8,
    }

    public fun increment(a: address) acquires Counter {
        let c = borrow_global_mut<Counter>(a);
        c.value = c.value + 1;
    }

    spec increment {
        aborts_if !exists<Counter>(a);
        ensures global<Counter>(a).value == old(global<Counter>(a)).value + 1;
    }
}
```

Move Prover 通过 Aptos cli 调用。

从要测试的目录或通过将其路径提供给 `--package-dir` 参数来调用 Move Prover：

* 证明当前目录下包的来源：

    ```rust
    aptos move prove
    ```

* 在 `<path>` 证明包的来源：

    ```rust
    aptos move prove --package-dir <path>
    ```

现在，如果我们为我们的代码运行 `aptos move prove` 命令，它将导致以下错误。

```bash
error: abort not covered by any of the `aborts_if` clauses
   │  
 8 │           c.value = c.value + 1;
   │                             - abort happened here with execution failure
   ·  
11 │ ╭     spec increment {
12 │ │         aborts_if !exists<Counter>(a);
13 │ │         ensures global<Counter>(a).value == old(global<Counter>(a)).value + 1;
14 │ │     }
   │ ╰─────^
{
  "Error": "Move Prover failed: exiting with verification errors"
}
```

Move Prover 生成了一个示例计数器，当为 `u8` 将 255 的值加 1 时会导致溢出。如果函数规范要求中止行为，但规范未涵盖函数中止的条件，则会发生此溢出。事实上，通过 `aborts_if !exists<Counter>(a)`，我们只覆盖了资源不存在导致的中止，而没有覆盖算术溢出导致的中止。

让我们修复上面的代码并添加以下条件：

```rust
spec increment {
    aborts_if global<Counter>(a).value == 255;
    ...
}
```

这样，Move Prover 就会成功，不会出现任何错误。



<!-- A Move prover is a tool that verifies the correctness of smart contracts written in the Move programming language. The Move prover helps ensure that these smart contracts behave correctly by mathematically proving the correctness of the code.

The Move prover works by automatically verifying that the smart contract code adheres to certain properties, such as not allowing for integer overflow or underflow. It also checks that the code is free from certain classes of bugs, such as reentrancy vulnerabilities and division by zero errors. If the Move prover finds any violations, it reports them back to the developer, allowing them to fix the code before it is deployed.

The Move Prover exists to make contracts more _trustworthy_; it:

* Protects massive assets managed by the Aptos blockchain from smart contract bugs
* Protects against well-resourced adversaries
* Anticipates justified regulator scrutiny and compliance requirements
* Allows domain experts with a mathematical background, but not necessarily a software engineering background, to understand what smart contracts do

{% hint style="info" %}
Currently, Windows is not supported by the Move Prover.
{% endhint %}

```rust
module my_addrx::Example{
    struct Counter has key {
        value: u8,
    }

    public fun increment(a: address) acquires Counter {
        let c = borrow_global_mut<Counter>(a);
        c.value = c.value + 1;
    }

    spec increment {
        aborts_if !exists<Counter>(a);
        ensures global<Counter>(a).value == old(global<Counter>(a)).value + 1;
    }
}
```

The Move Prover is invoked via the Aptos cli.

Call the Move Prover from the directory to be tested or by supplying its path to the `--package-dir` argument:

*   Prove the sources of the package in the current directory:

    ```rust
    aptos move prove
    ```
*   Prove the sources of the package at \<path>:

    ```rust
    aptos move prove --package-dir <path>
    ```

Now if we run `aptos move prove` command for our code it will lead to following error.

```bash
error: abort not covered by any of the `aborts_if` clauses
   │  
 8 │           c.value = c.value + 1;
   │                             - abort happened here with execution failure
   ·  
11 │ ╭     spec increment {
12 │ │         aborts_if !exists<Counter>(a);
13 │ │         ensures global<Counter>(a).value == old(global<Counter>(a)).value + 1;
14 │ │     }
   │ ╰─────^
{
  "Error": "Move Prover failed: exiting with verification errors"
}
```

The Move Prover has generated an example counter that leads to an overflow when adding 1 to the value of 255 for an `u8`. This overflow occurs if the function specification calls for abort behavior, but the condition under which the function is aborting is not covered by the specification. And in fact, with `aborts_if !exists<Counter>(a)`, we only cover the abort caused by the absence of the resource, but not the abort caused by the arithmetic overflow.

Let's fix the above code and add the following condition:

```rust
spec increment {
    aborts_if global<Counter>(a).value == 255;
    ...
}
```

With this, the Move Prover will succeed without any errors. -->
