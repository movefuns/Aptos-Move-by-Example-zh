---
title: 全局存储操作
weight: 41
---

# 全球存储操作

Move 程序可以使用以下五个指令在全局存储中创建、删除和更新资源：

<!-- # Global Storage Operations

Move programs can `create`, `delete`, and `update` resources in global storage using the following five instructions: -->

| Operation                               | Description                                                     | Aborts?                                 |
| --------------------------------------- | --------------------------------------------------------------- | --------------------------------------- |
| `move_to<T>(&signer,T)`                 | Publish `T` under `signer.address`                              | If `signer.address` already holds a `T` |
| `move_from<T>(address): T`              | Remove `T` from `address` and return it                         | If `address` does not hold a `T`        |
| `borrow_global_mut<T>(address): &mut T` | Return a mutable reference to the `T` stored under `address`    | If `address` does not hold a `T`        |
| `borrow_global<T>(address): &T`         | Return an immutable reference to the `T` stored under `address` | If `address` does not hold a `T`        |
| `exists<T>(address): bool`              | Return `true` if a `T` is stored under `address`                | Never                                   |

<!-- Each of these instructions is parameterized by a type `T` with the `key` ability. However, each type `T` _must be declared in the current module_. This ensures that a resource can only be manipulated via the API exposed by its defining module. The instructions also take either an address `&signer` representing the account address where the resource of type `T` is stored. -->
这些指令中的每一条都由具有关键能力的类型 `T` 参数化。但是，每个类型 `T` 都必须在当前模块中声明。这确保资源只能通过其定义模块公开的 API 进行操作。这些指令还采用地址 `&signer` 表示存储类型 `T` 资源的帐户地址。

```rust
module my_addrx::counter {
    use std::signer;
    use std::account;

    /// Resource that wraps an integer counter
    struct Counter has key { i: u64 }

    /// Publish a `Counter` resource with value `i` under the given `account`
    public fun publish(account: &signer, i: u64) {
      // "Pack" (create) a Counter resource. This is a privileged operation that
      // can only be done inside the module that declares the `Counter` resource
      move_to(account, Counter { i })
    }

    /// Read the value in the `Counter` resource stored at `addr`
    public fun get_count(addr: address): u64 acquires Counter {
        borrow_global<Counter>(addr).i
    }

    /// Increment the value of `addr`'s `Counter` resource
    public fun increment(addr: address) acquires Counter {
        let c_ref = &mut borrow_global_mut<Counter>(addr).i;
        *c_ref = *c_ref + 1
    }

    /// Reset the value of `account`'s `Counter` to 0
    public fun reset(account: &signer) acquires Counter {
        let c_ref = &mut borrow_global_mut<Counter>(signer::address_of(account)).i;
        *c_ref = 0
    }

    /// Delete the `Counter` resource under `account` and return its value
    public fun delete(account: &signer): u64 acquires Counter {
        // remove the Counter resource
        let c = move_from<Counter>(signer::address_of(account));
        // "Unpack" the `Counter` resource into its fields. This is a
        // privileged operation that can only be done inside the module
        // that declares the `Counter` resource
        let Counter { i } = c;
        i
    }

    /// Return `true` if `addr` contains a `Counter` resource
    public fun resource_exists(addr: address): bool {
        exists<Counter>(addr)
    }

    #[test(admin = @0x123)]
    public entry fun test_flow(admin: signer) acquires Counter 
    {
        account::create_account_for_test(signer::address_of(&admin));
        
        publish(&admin,5);
        assert!(get_count(signer::address_of(&admin))==5,1);
        increment(signer::address_of(&admin));
        assert!(get_count(signer::address_of(&admin))==6,1);
        reset(&admin);
        assert!(get_count(signer::address_of(&admin))==0,1);
        delete(&admin);
        assert!(resource_exists(signer::address_of(&admin))==false,1);
    }
}
```

在计数器示例中，您可能已经注意到 `get_count`、`increment`、`reset` 和 `delete` 函数被注释为 `acquires Counter`。当且仅当：

* `m::f` 的主体包含 `move_from<T>`、`borrow_global_mut<T>` 或 `borrow_global<T>` 指令，或
* `m::f` 的主体调用在同一个模块中声明的函数 `m::g`，该模块用 `acquires` 注释。

<!-- In the `counter` example, you might have noticed that the `get_count`, `increment`, `reset`, and `delete` functions are annotated with `acquires Counter`. A Move function `m::f` must be annotated with `acquires T` if and only if:

* The body of `m::f` contains a `move_from<T>`, `borrow_global_mut<T>`, or `borrow_global<T>` instruction, or
* The body of `m::f` invokes a function `m::g` declared in the same module that is annotated with `acquires.` -->
