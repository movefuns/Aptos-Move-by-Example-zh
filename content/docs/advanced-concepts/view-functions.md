# 查看函数

查看函数是一种从区块链中检索数据而不对其进行任何更改的函数。它用于读取和显示存储在区块链上的数据。

视图函数在 Aptos 区块链中很重要，因为它们允许外部应用程序访问和显示存储在区块链上的数据，而无需直接访问区块链本身。这可以提高区块链网络的效率和安全性。

视图功能是最近添加到 Aptos 区块链的功能，它允许开发人员创建 GET API 结构来显示智能合约的复杂状态。以往，开发者需要在客户端拉取所有模块的资源数据并进行计算，耗时且效率低下。但是，借助视图函数，开发人员现在可以通过更简单、更流线型的过程检索复杂状态，从而节省时间和资源。这一发展显着提高了 Aptos 区块链的可用性，并使开发人员更容易访问它。

<!-- # View functions

View function is a function that retrieves data from the blockchain without making any changes to it. It is used to read and display data stored on the blockchain.

View functions are important in the Aptos blockchain because they allow external applications to access and display data stored on the blockchain without the need for direct access to the blockchain itself. This can improve the efficiency and security of the blockchain network.

View functions are a recent addition to the Aptos blockchain that allows developers to create a GET API structure to display complex states of smart contracts. In the past, developers had to fetch all the module’s resource data and perform calculations on the client-side, which was time-consuming and inefficient. However, with view functions, developers can now retrieve complex states through a simpler and more streamlined process that saves time and resources. This development has significantly improved the usability of the Aptos blockchain and made it more accessible to developers. -->

```rust
#[view]
public fun get_todos(todo_address: address): vector<String> acquires TodoStore {
    borrow_global<TodoStore>(todo_address).todos
}
```

随着视图功能的引入，开发人员现在可以以更加简化和高效的方式从智能合约中检索复杂状态。视图函数允许开发人员定义可以从智能合约返回特定数据的函数，提供一个简单的 API，外部调用者可以使用该 API 从区块链中检索数据。这一进步显着提高了 Aptos 区块链的可用性和可访问性，使其对开发人员更加友好且更易于使用。

您现在可以使用新的视图函数以所需的格式接收所需的特定数据，而不是每次查询时都接收整个数据库。

<!-- With the introduction of view functions, developers can now retrieve complex states from a smart contract in a more streamlined and efficient manner. View functions allow developers to define functions that can return specific data from a smart contract, providing a simple API that can be used by external invokers to retrieve data from the blockchain. This advancement has significantly improved the usability and accessibility of the Aptos blockchain, making it more developer-friendly and easier to use.

Instead of receiving the entire database every time you query it, you can now receive the specific data you need in the desired format using the new view functions. -->
