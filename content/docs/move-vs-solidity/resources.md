# 资源

Move 被设计为一种面向对象的语言，用于编写具有安全资源管理的智能合约或程序。资产被定义为“资源（resource）”，可以在账户之间移动，但不能被双花或重复。

 这使得编写无错误代码变得非常容易，与 Solidity 相比，在 Solidity 中，资产的转移必须手动指定，从而增加了编写错误代码的可能性。

**Resource** 本来就是一个完美的存储数字资产的类型，要实现它必须是不可复制和不可丢弃的。同时，它必须可以在账户之间存储和转移。

**定义资源**：

```rust
struct ResourceName has key, store {
        FIELD: TYPE
}
```

在模块名称之后调用模块的主要资源有一个约定。

```rust
module my_addrx::Basket
{
    use std::string::String;
    struct Basket has key,store   //Basket is the resource containing list of fruits in the basket
    {
        list_of_fruits:vector<Fruit>
    }

    struct Fruit has store,drop,copy
    {
        name:String,
        calories:u8
    }
}
```


<!-- # Resources

Move is designed as an object-oriented language to write smart contracts or programs with safe resource management. Assets are defined as a “resource”, which can be moved between accounts, but which cannot be double-spent or duplicated.

This makes it very easy to write error-free code, in contrast to Solidity, where transfers of assets must be specified manually, increasing the likelihood of writing faulty code.

**Resource** is meant to be a perfect type for storing _digital assets_, to achieve that it must to be non-copyable and non-droppable. At the same time it must be storable and transferable between accounts.

**Defining a Resource:**

```rust
struct ResourceName has key, store {
        FIELD: TYPE
}
```

{% hint style="info" %}
There's a convention to call main resource of the module after module name.
{% endhint %}

```rust
module my_addrx::Basket
{
    use std::string::String;
    struct Basket has key,store   //Basket is the resource containing list of fruits in the basket
    {
        list_of_fruits:vector<Fruit>
    }

    struct Fruit has store,drop,copy
    {
        name:String,
        calories:u8
    }
}
``` -->
