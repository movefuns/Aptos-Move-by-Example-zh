# 泛型

Move 语言支持使用泛型，它允许开发人员编写可用于不同类型数据的代码，而不必为每种特定类型重写代码。

Move 中的泛型类似于其他编程语言（如 Java 和 C#）中的泛型。它们允许开发人员编写可重用的代码，这些代码可以与满足一组特定要求的任何类型一起使用。这有助于减少代码重复并提高代码的可读性和可维护性。

在 Move 中，我们经常将术语泛型与类型参数和类型参数互换使用。

**声明类型参数**

函数和结构都可以在它们的签名中采用类型参数列表，用一对尖括号 `<...>` 括起来。

**泛型函数**

函数的类型参数位于函数名称之后和（值）参数列表之前。下面的代码定义了一个通用的身份函数，它接受任何类型的值并返回不变的值。
一旦定义，类型参数 `T` 就可以在参数类型、返回类型和函数体内使用。

<!-- # Generics

Move language  supports the use of generics, which allow developers to write code that can be used with different types of data without having to rewrite the code for each specific type.

Generics in Move are similar to generics in other programming languages such as Java and C#. They allow developers to write reusable code that can work with any type that meets a specific set of requirements. This can help reduce code duplication and improve code readability and maintainability.

In Move, we will often use the term generics interchangeably with _type parameters_ and _type arguments_.

**Declaring Type Parameters**

Both functions and structs can take a list of type parameters in their signatures, enclosed by a pair of angle brackets `<...>`.

**Generic Functions**

Type parameters for functions are placed after the function name and before the (value) parameter list. The following code defines a generic identity function that takes a value of any type and returns that value unchanged.

Once defined, the type parameter `T` can be used in parameter types, return types, and inside the function body. -->

```rust
module my_addrx::Generics
{
    //a generic identity function that takes a value of any type and returns that value unchanged
    fun example<T>(num: T): T {
        num
    }

    #[test]
    fun testing()
    {
        let x:u64 = example<u64>(8);
        let y:bool = example<bool>(true);

        assert!(x==8,1);
        assert!(y==true,1);

    }

}
```

**通用结构**

结构的类型参数放在结构名称之后，可用于命名字段的类型。

<!-- **Generic Structs**

Type parameters for structs are placed after the struct name, and can be used to name the types of the fields. -->

```rust
struct Foo<T> has copy, drop { x: T }

struct Bar<T1, T2> has copy, drop {
    x: T1,
    y: vector<T2>,
}
```
