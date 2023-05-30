---
bookCollapseSection: true
---

# 为什么 Move 安全

> Move 语言的属性使其成为编写智能合约的更安全的语言。

确保智能合约的安全在区块链领域至关重要，因为即使是一个小漏洞也可能由于所涉资产的巨大价值而导致灾难性后果。  因此，选择合适的编程语言对于确保智能合约的可靠性和安全性至关重要。

智能合约语言安全的主要目标是了解合约的内部工作原理，并防止由错误或安全漏洞引起的潜在问题，例如重入问题，这是智能合约中执行多次自调用的常见问题，导致流失的资金。

Move 语言为开发人员提供了有价值的工具，例如 Move 证明器，它有助于验证他们的合约是否正确运行。

Move Prover是形式化验证工具，形式化验证是使用数学和逻辑方法证明程序符合其规范的过程。

Move 的其他安全功能： 

**线性类型系统**：Move 具有强大的线性类型系统，可确保 Move 程序中数据结构的特定访问和修改模式。  这有助于避免不可预见的行为。

**可见性系统**：Move 结合了可靠的可见性系统，使开发人员能够精确地管理区块链中的数据访问、修改和存储。  这有助于防止未经授权访问敏感信息或代码。

**模块化**：Move 是高度模块化的，允许模块拥有资源，这意味着只有负责定义资源的模块才能修改它。  这建立了一个安全的数据结构，因为只有授权的模块才能访问和修改其中的信息。

 有关详细信息，请在此处查看：[https://pontem.network/posts/what-makes-move-safe](https://pontem.network/posts/what-makes-move-safe)


<!-- Securing smart contracts is vital in the blockchain sector, as even a small vulnerability can result in disastrous consequences due to the significant value of the assets at stake. As such, selecting an appropriate programming language is essential in ensuring the reliability and security of smart contracts. -->

<!-- The primary goal of safety in smart contract languages is to understand the contract's inner workings and prevent potential problems arising from bugs or security flaws, such as the reentrancy issue, a common problem in smart contracts where multiple self-calls are executed, causing a drain of funds.

The Move language equips developers with valuable tools like the **Move prover**, which aids in verifying the correct operation of their contracts.

Move Prover is formal verification tool, formal verification is the process of using mathematical and logical methods to prove that a program meets its specification.

Other security features of Move :&#x20;

* **Linear Type System**: Move features a robust linear type system that guarantees specific access and modification patterns for data structures within a Move program. This aids in avoiding unforeseen behaviours.
* **Visibility System**: Move incorporates a solid visibility system, enabling developers to precisely manage data access, modification, and storage within a blockchain. This assists in preventing unauthorised access to sensitive information or code.
* **Modularity**: Move is highly modular, enabling resource ownership by modules, which means that only the module responsible for defining a resource can modify it. This establishes a secure data structure, as only authorized modules can access and modify the information within.

For more details check here : [https://pontem.network/posts/what-makes-move-safe](https://pontem.network/posts/what-makes-move-safe)
-->
