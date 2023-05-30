---
title: 类型推断
weight: 38
---

# 类型推断

在大多数情况下，Move 编译器将能够推断类型参数，因此您不必显式写下它们。如果我们省略类型参数，上面的示例将如下所示：

<!-- # Type Inference

In most cases, the Move compiler will be able to infer the type arguments so you don't have to write them down explicitly. Here's what the examples above would look like if we omit the type arguments: -->

```rust
fun foo() {
    let x = id(true);
    //        ^ <bool> is inferred

    let foo = Foo { x: true };
    //           ^ <bool> is inferred

    let Foo { x } = foo;
    //     ^ <bool> is inferred
}
```

注意：当编译器无法推断类型时，您需要手动注释它们。一个常见的场景是调用一个类型参数只出现在返回位置的函数。

<!-- Note: when the compiler is unable to infer the types, you'll need annotate them manually. A common scenario is to call a function with type parameters appearing only at return positions. -->

```rust
module my_addrx::TypeInference{
    use std::vector;

    fun foo() {
        // let v = vector::empty();
        //                    ^ The compiler cannot figure out the element type.

        let v = vector::empty<u64>();
        //                 ^~~~~ Must annotate manually.
    }
}
```

<!-- However, the compiler will be able to infer the type if that return value is used later in that function: -->
但是，如果稍后在该函数中使用该返回值，编译器将能够推断出类型：

```rust
module my_addrx::TypeInference{
    use std::vector;

    fun foo() {
        let v = vector::empty();
        //                 ^ <u64> is inferred
        vector::push_back(&mut v, 100);
    }
}
```
