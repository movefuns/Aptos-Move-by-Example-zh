---
title: 重入攻击
weight: 8
---

# 重入攻击

重入攻击是智能合约中可能发生的一种漏洞，恶意用户可以利用合约代码中的缺陷在前一个函数执行完成之前重复执行一个函数。  这可能会导致意外行为，并可能允许攻击者窃取资金或操纵合约状态。

Solidity 和 Move 都有防止重入攻击的机制，但它们有不同的方法。

在 `Solidity` 中，推荐的防止重入攻击的方法是使用“检查-效果-交互”模式，这涉及在执行任何外部交互之前对用户输入和合约状态执行检查。此模式可确保在任何外部交互发生之前更新合约状态，从而防止任何潜在的重入攻击。

在 `Move` 中，资源模型阻止了重入攻击，这确保资源一次只能由单个执行上下文访问。这意味着如果函数执行未完成，则在执行完成之前其他函数无法访问同一资源。这可以防止重入攻击，因为恶意用户无法在前一次执行完成之前重复执行一个函数。

总的来说，Solidity 和 Move 都有防止重入攻击的机制。然而，Solidity 依靠编程模式来防止这些攻击，而 Move 使用资源模型来防止它们。两者之间的选择最终取决于应用程序的具体要求和所使用的平台。

<!-- # Reentrancy attacks

Reentrancy attacks are a type of vulnerability that can occur in smart contracts, where a malicious user can exploit a flaw in the contract code to repeatedly execute a function before the previous function execution has completed. This can lead to unexpected behavior and can allow the attacker to steal funds or manipulate the contract state.

Both Solidity and Move have mechanisms in place to prevent reentrancy attacks, but they have different approaches.

In `Solidity`, the recommended approach to prevent reentrancy attacks is to use the "checks-effects-interactions" pattern, which involves performing checks on user inputs and contract state before executing any external interactions. This pattern ensures that the contract state is updated before any external interactions occur, which prevents any potential reentrancy attacks.

In `Move`, reentrancy attacks are prevented by the resource model, which ensures that a resource can only be accessed by a single execution context at a time. This means that if a function execution is not complete, other functions cannot access the same resource until the execution is complete. This prevents reentrancy attacks because a malicious user cannot repeatedly execute a function before the previous execution has completed.

Overall, both Solidity and Move have mechanisms in place to prevent reentrancy attacks. However, Solidity relies on a programming pattern to prevent these attacks, while Move uses the resource model to prevent them. The choice between the two ultimately depends on the specific requirements of the application and the platform being used. -->
