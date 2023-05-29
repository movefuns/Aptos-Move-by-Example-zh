# 签名者

`signer`（签名者）是一种代表对区块链上资源或资产的授权和控制的类型。签名者类型用于指示哪个帐户或实体负责在区块链上执行特定交易或操作。

您可以将本机实现视为：

<!-- # Signer

A `signer` is a type that represents the authorization and control of a resource or asset on the blockchain. The signer type is used to indicate which account or entity is responsible for executing a particular transaction or operation on the blockchain.

You can think of the native implementation as being: -->

```rust
struct signer has drop { a: address }
```

签名者值是特殊的，因为它们不能通过文字或指令创建——只能由 Move VM 创建。在 VM 运行带有 `signer` 类型参数的脚本之前，它会自动创建 `signer` 值并将它们传递到脚本中：

<!-- Signer values are special because they cannot be created via literals or instructions--only by the Move VM. Before the VM runs a script with parameters of type `signer`, it will automatically create `signer` values and pass them into the script: -->

```rust
script {
    use std::signer;
    fun main(s: signer) {
        assert!(signer::address_of(&s) == @my_addrx, 0);
    }
}
```

**`signer` 操作**：

`std::signer` 标准库模块在 `signer` 值上提供了两个实用函数：

* `signer::address_of(&signer): address` - 返回此 `&signer` 包装的地址。
* `signer::borrow_address(&signer): &address` - 返回对此 `&signer` 包装的地址的引用。

<!-- **`signer` operations:**

The `std::signer` standard library module provides two utility functions over `signer` values:

* `signer::address_of(&signer): address` - Return the `address` wrapped by this `&signer`.
* `signer::borrow_address(&signer): &address` - Return a reference to the `address` wrapped by this `&signer`. -->

```rust
module my_addrx::MyResource
{
    use std::signer; 
    
    struct MyResource has key
    {
        value:u64
    }

    public entry fun increase_value_by_one(account: &signer) acquires MyResource {
        
        let signer_address = signer::address_of(account); 
        
        let myresource = borrow_global_mut<MyResource>(signer_address);
        myresource.value=myresource.value+1;
    }
}

```

**所有权**

与简单的标量值不同，`signer` 值不可复制，这意味着它们不能被复制（从任何操作，无论是通过显式 `copy`（复制）指令还是通过 `dereference *`（解引用）。

<!-- Unlike simple scalar values, `signer` values are not copyable, meaning they cannot be copied(from any operation whether it be through an explicit `copy` instruction or through a `dereference *`. -->
