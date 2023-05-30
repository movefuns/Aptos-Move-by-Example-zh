---
title: 并行处理
weight: 7
---

# 并行处理

Solidity 和 Move 在处理和并发方面有不同的方法。

在 Solidity 中，默认的执行模型是顺序处理，这意味着代码按照它在代码中出现的顺序一次执行一步。这是因为 Solidity 是一种单线程语言，这意味着它一次只能执行一条指令。然而，Solidity 确实通过使用事件和回调支持异步处理，这允许开发人员编写可以在主线程执行其他代码时在后台执行的代码。

相比之下，Move 具有并行处理模型，允许多个线程同时执行指令。这是通过使用 Move 的资源模型实现的，该模型允许资源在不同线程之间移动。这意味着 Move 可以同时执行多条指令，从而缩短处理时间并提高可扩展性。

顺序处理和并行处理之间的选择最终取决于应用程序的具体要求。如果应用程序需要高速处理和可扩展性，那么像 Move 这样的并行处理模型可能更合适。但是，如果应用程序不需要并行处理，更注重安全性和可预测性，那么像 Solidity 这样的顺序处理模型可能更合适。

<!-- # Parallel Processing

Solidity and Move have different approaches to processing and concurrency.

In Solidity, the default execution model is sequential processing, meaning that the code is executed one step at a time, in the order that it appears in the code. This is because Solidity is a single-threaded language, which means that it can only execute one instruction at a time. However, Solidity does support asynchronous processing through the use of events and callbacks, which allow developers to write code that can execute in the background while the main thread is executing other code.

In contrast, Move has a parallel processing model, which allows for multiple threads to execute instructions simultaneously. This is achieved through the use of Move's resource model, which allows resources to be moved between different threads. This means that Move can execute multiple instructions at the same time, which can result in faster processing times and improved scalability.

The choice between sequential and parallel processing ultimately depends on the specific requirements of the application. If the application requires high-speed processing and scalability, a parallel processing model like Move may be more appropriate. However, if the application does not require parallel processing and is more focused on security and predictability, a sequential processing model like Solidity may be more appropriate. -->
