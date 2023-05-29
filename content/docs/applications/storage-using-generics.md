# 使用泛型存储

**合约逻辑**：

* 使用泛型实现存储合约。
* 函数的类型参数位于函数名称之后和（值）参数列表之前。  下面的代码定义了一个通用的身份函数，它接受任何类型的值并返回不变的值。
* 一旦定义，类型参数 `T` 就可以在参数类型、返回类型和函数体内使用。

<!-- # Storage using Generics

**Contract logic:**

* Implementation of `Storage Contract` using Generics.
* Type parameters for functions are placed after the function name and before the (value) parameter list. The following code defines a generic identity function that takes a value of any type and returns that value unchanged.
* Once defined, the type parameter `T` can be used in parameter types, return types, and inside the function body. -->

```rust
module store_addrx::Storage{
    use std::signer;

    const ERROR: u64 = 101;

    struct Storage<T: store>has key{
        val: T,
    }

    fun store<T:store>(account: &signer,val: T){
        let addr = signer::address_of(account);
        assert!(!exists<Storage<T>>(addr),ERROR);
        let to_store = Storage{
            val,
        };
        move_to(account,to_store);
    }

    public fun get<T: store>(account: &signer):T acquires Storage{
        let addr = signer::address_of(account);
        assert!(exists<Storage<T>>(addr),ERROR);
        let Storage{val} = move_from<Storage<T>>(addr);
        val
    }

    #[test(account=@0x123)]
    fun test_store_u128(account: signer) acquires Storage{
        let value: u128 = 100;
        store(&account,value);
        assert!(value == get<u128>(&account),ERROR)
    }
}
```
