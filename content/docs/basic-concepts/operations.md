---
title: 操作
weight: 24
---

# 操作

**算术**

这些类型中的每一个都支持相同的一组检查算术运算。对于所有这些操作，两个参数（左侧和右侧操作数）必须属于同一类型。如果您需要对不同类型的值进行操作，则需要先执行强制转换。类似地，如果您预计操作的结果对于整数类型而言太大，请在执行操作之前执行转换为更大的大小。

所有算术运算都会中止，而不是以数学整数不会出现的方式（例如，上溢、下溢、被零除）。

<!-- # Operations

**Arthmetic**

Each of these types supports the same set of checked arithmetic operations. For all of these operations, both arguments (the left and right side operands) _must_ be of the same type. If you need to operate over values of different types, you will need to first perform a cast. Similarly, if you expect the result of the operation to be too large for the integer type, perform a cast to a larger size before performing the operation.

All arithmetic operations abort instead of behaving in a way that mathematical integers would not (e.g., overflow, underflow, divide-by-zero). -->

| Syntax | Operation           | Aborts If                                |
| ------ | ------------------- | ---------------------------------------- |
| `+`    | addition            | Result is too large for the integer type |
| `-`    | subtraction         | Result is less than zero                 |
| `*`    | multiplication      | Result is too large for the integer type |
| `%`    | modular division    | The divisor is `0`                       |
| `/`    | truncating division | The divisor is `0`                       |

```rust
module my_addrx::Operations
{
    use std::debug::print;

    fun arthmetic_operations(a:u64,b:u64)
    {
        let ans=a+b;       print(&ans);
        let ans=a-b;       print(&ans);
        let ans=a*b;       print(&ans); 
        let ans=a/b;       print(&ans);
        let ans=a%b;       print(&ans);

    }

    #[test]
    fun testing()
    {
        arthmetic_operations(10,3);
    }
}
```

**位运算**

整数类型支持以下按位运算，这些运算将每个数字视为一系列单独的位（0 或 1），而不是数字整数值。

按位运算不会中止。

<!-- **Bitwise**

The integer types support the following bitwise operations that treat each number as a series of individual bits, either 0 or 1, instead of as numerical integer values.

Bitwise operations do not abort. -->

| Syntax | Operation   | Description                                           |
| ------ | ----------- | ----------------------------------------------------- |
| `&`    | bitwise and | Performs a boolean and for each bit pairwise          |
| `\|`   | bitwise or  | Performs a boolean or for each bit pairwise           |
| `^`    | bitwise xor | Performs a boolean exclusive or for each bit pairwise |

```rust
module my_addrx::Operations
{
    use std::debug::print;

    fun bitwise_operations(a:u64,b:u64)
    {
        let ans=a|b;       print(&ans);
        let ans=a&b;       print(&ans);
        let ans=a^b;       print(&ans);  

    }

    #[test]
    fun testing()
    {
        bitwise_operations(1,0);
    }
}
```

**移位**

与按位运算类似，每个整数类型都支持位移。  但与其他操作不同的是，右侧操作数（要移动多少位）必须始终为 `u8`，并且不需要与左侧操作数（要移动的数字）匹配。

如果要移动的位数分别大于或等于 `8`、`16`、`32`、`64`、`128` 或 `256`（对于 `u8`、`u16`、`u32`、`u64`、`u128` 和 `u256`），位移可以中止。

<!-- **Bitshift**

Similar to the bitwise operations, each integer type supports bit shifts. But unlike the other operations, the righthand side operand (how many bits to shift by) must _always_ be a `u8` and need not match the left side operand (the number you are shifting).

Bit shifts can abort if the number of bits to shift by is greater than or equal to `8`, `16`, `32`, `64`, `128` or `256` for `u8`, `u16`, `u32`, `u64`, `u128` and `u256` respectively. -->

| Syntax | Operation   | Aborts if                                                               |
| ------ | ----------- | ----------------------------------------------------------------------- |
| `<<`   | shift left  | Number of bits to shift by is greater than the size of the integer type |
| `>>`   | shift right | Number of bits to shift by is greater than the size of the integer type |

```rust
module my_addrx::Operations
{
    use std::debug::print;

    fun bitshift_operations(a:u64)
    {
        let ans=a>>2;       print(&ans);
        let ans=a<<2;       print(&ans);

    }

    #[test]
    fun testing()
    {
        bitshift_operations(4);
    }
}
```

**比较**

整数类型是 Move 中*唯一*可以使用比较运算符的类型。两个参数必须是同一类型。如果你需要比较不同类型的整数，你需要先转换其中一个。

比较操作不会中止。

<!-- **Comparision**

Integer types are the _only_ types in Move that can use the comparison operators. Both arguments need to be of the same type. If you need to compare integers of different types, you will need to cast one of them first.

Comparison operations do not abort. -->

| Syntax | Operation                |
| ------ | ------------------------ |
| `<`    | less than                |
| `>`    | greater than             |
| `<=`   | less than or equal to    |
| `>=`   | greater than or equal to |

```rust
module my_addrx::Operations
{
    use std::debug::print;

    fun comparison_operations(a:u64,b:u64)
    {
        let ans=a < b;        print(&ans); //always give proper spacing between comparison operator and the operands
        let ans=a > b;        print(&ans);
        let ans=a <= b;       print(&ans);  
        let ans=a >= b;       print(&ans);  

    }

    #[test]
    fun testing()
    {
        comparison_operations(10,14);
    }
}
```

**比较**

与 Move 中的所有类型一样，所有整数类型都支持“等于”和“不等于”操作。两个参数必须是同一类型。如果你需要比较不同类型的整数，你需要先转换其中一个。

相等操作不会中止。

<!-- **Equality**

Like all types with drop in Move, all integer types support the "equal" and "not equal" operations. Both arguments need to be of the same type. If you need to compare integers of different types, you will need to cast one of them first.

Equality operations do not abort. -->

| Syntax | Operation |
| ------ | --------- |
| `==`   | equal     |
| `!=`   | not equal |

```rust
module my_addrx::Operations
{
    use std::debug::print;

    fun comparison_operations(a:u64,b:u64)
    {
        let ans=a == b;        print(&ans);
        let ans=a != b;        print(&ans);
    }

    #[test]
    fun testing()
    {
        comparison_operations(10,14);
    }
}
```

**强制转换**

一种大小的整数类型可以转换为另一种大小的整数类型。整数是 Move 中唯一支持转换的类型。

强制转换不会截断。如果结果对于指定类型而言太大，转换将中止

<!-- **Casting**

Integer types of one size can be cast to integer types of another size. Integers are the only types in Move that support casting.

Casts _do not_ truncate. Casting will abort if the result is too large for the specified type -->

| Syntax     | Operation                                            | Aborts if                              |
| ---------- | ---------------------------------------------------- | -------------------------------------- |
| `(e as T)` | Cast integer expression `e` into an integer type `T` | `e` is too large to represent as a `T` |

这里，`e` 的类型必须是 `8`、`16`、`32`、`64`、`128` 或 `256`，`T` 必须是 `u8`、`u16`、`u32`、`u64`、`u128` 或 `u256`。

<!-- Here, the type of `e` must be `8`, `16`, `32`, `64`, `128` or `256` and `T` must be `u8`, `u16`, `u32`, `u64`, `u128` or `u256`. -->

例如：

* `(x as u8)`
* `(y as u16)`
* `(873u16 as u32)`
* `(2u8 as u64)`
* `(1 + 3 as u128)`
* `(4/2 + 12345 as u256)`
