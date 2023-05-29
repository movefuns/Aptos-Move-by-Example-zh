# 类型参数

**调用通用函数**

调用泛型函数时，可以在一对尖括号括起来的列表中指定函数类型参数的类型参数。

<!-- # Type Arguments

**Calling Generic Functions**

When calling a generic function, one can specify the type arguments for the function's type parameters in a list enclosed by a pair of angle brackets. -->

```rust
fun foo() {
    let x = id<bool>(true);
}
```

如果您不指定类型参数，Move 的类型推断将为您提供它们。

**使用通用结构**

类似地，在构造或析构泛型类型的值时，可以为结构的类型参数附加一个类型参数列表。

<!-- If you do not specify the type arguments, Move's type inference will supply them for you.

**Using Generic Structs**

Similarly, one can attach a list of type arguments for the struct's type parameters when constructing or destructing values of generic types. -->

```rust
fun foo() {
    let foo = Foo<bool> { x: true };
    let Foo<bool> { x } = foo;
}
```

<!-- If you do not specify the type arguments, Move's type inference will supply them for you.

**Type Argument Mismatch**

If you specify the type arguments and they conflict with the actual values supplied, an error will be given: -->

如果您不指定类型参数，Move 的类型推断将为您提供它们。

**类型参数不匹配**

如果您指定类型参数并且它们与提供的实际值冲突，则会给出错误：

```rust
fun foo() {
    let x = id<u64>(true); // error! true is not a u64
}
```

and similarly:

```rust
fun foo() {
    let foo = Foo<bool> { x: 0 }; // error! 0 is not a bool
    let Foo<address> { x } = foo; // error! bool is incompatible with address
}
```

**Unused Type Parameters**

For a struct definition, an unused type parameter is one that does not appear in any field defined in the struct, but is checked statically at compile time. Move allows unused type parameters so the following struct definition is valid:

**未使用的类型参数**

对于结构定义，未使用的类型参数未出现在结构中定义的任何字段中，但在编译时静态检查。Move 允许未使用的类型参数，因此以下结构定义是有效的：

```rust
struct Foo<T> {
    foo: u64
}
```

这在对某些概念建模时会很方便。这是一个例子：
<!-- This can be convenient when modeling certain concepts. Here is an example: -->

```rust
module my_addrx::M{
    // Currency Specifiers
    struct Currency1 {}
    struct Currency2 {}

    // A generic coin type that can be instantiated using a currency
    // specifier type.
    //   e.g. Coin<Currency1>, Coin<Currency2> etc.
    struct Coin<Currency> has store {
        value: u64
    }

    // Write code generically about all currencies
    public fun mint_generic<Currency>(value: u64): Coin<Currency> {
        Coin { value }
    }

    // Write code concretely about one currency
    public fun mint_concrete(value: u64): Coin<Currency1> {
        Coin { value }
    }
}

```

在此示例中，`struct Coin<Currency>` 在 `Currency` 类型参数上是泛型的，它指定硬币的货币，并允许以任何货币泛型或具体地以特定货币编写代码。即使货币类型参数没有出现在 `Coin` 中定义的任何字段中，这种通用性也适用。

<!-- In this example, `struct Coin<Currency>` is generic on the `Currency` type parameter, which specifies the currency of the coin and allows code to be written either generically on any currency or concretely on a specific currency. This genericity applies even when the `Currency` type parameter does not appear in any of the fields defined in `Coin`. -->
