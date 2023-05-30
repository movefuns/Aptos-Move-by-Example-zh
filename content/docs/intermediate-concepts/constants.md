---
title: 常量
weight: 27
---

# 常量

常量是一种为`模块`或`脚本`内的共享静态值命名的方法。在 Move 中，常量是使用 `const` 关键字声明的。

常量必须在编译时已知。  常量的值存储在已编译的模块或脚本中。  每次使用常量时，都会制作该值的新副本。
句法：

<!-- # Constants

Constants are a way of giving a name to shared, static values inside of a `module` or `script`. In Move, constants are declared using the `const` keyword.

The constant's must be known at compilation. The constant's value is stored in the compiled module or script. And each time the constant is used, a new copy of that value is made. -->

**语法**：

```rust
const <NAME>: <TYPE> = <EXPRESSION>;
```

```rust
module my_addrx::Constants
{
    use std::debug::print;

    //Some Examples
    const X:u64=10;
    const Y:address=@my_addrx;
    const Z:bool=false;

    fun constants()
    {
        print(&X);
        print(&Y);
        print(&Z);
        
    }

    #[test]
    fun testing()
    {
        constants();
    }
}
```

**命名**：

常量必须以大写字母 `A` 到 `Z` 开头。在第一个字母之后，常量名称可以包含下划线 `_`、字母 `a` 到 `z`、字母 `A` 到 `Z` 或数字 `0` 到 `9`。

<!-- **Naming:**

Constants must start with a capital letter `A` to `Z`. After the first letter, constant names can contain underscores `_`, letters `a` to `z`, letters `A` to `Z`, or digits `0` to `9`. -->

```rust
//Valid
const Foo:u64=123;
const Flag:bool=true; 
const My_Addrx:address=@my_addrx;

//Invalid;
const x:u8=10;
const flag:bool=false;
const my_addrx:address=@my_addrx;
```

**其他一些要点**：

* 常量仅限于基本类型 `bool`、`u8`、`u16`、`u32`、`u64`、`u128`、`u256`、`address` 和 `vector<u8>`。
* 如果常量名称表示错误代码（例如，`EIndexOutOfBounds`），则常量名称应为大写驼峰式并以 `E` 开头；如果它们表示非错误值（例如，`MIN_STAKE`），则应为大写蛇形。
* 当前不支持 `public`（公共）常量。`const` 值只能在声明模块中使用。
* 无法在其模块或脚本范围之外访问常量值。

<!-- **Some other important points:**

* Constants are limited to the primitive types `bool`, `u8`, `u16`, `u32`, `u64`, `u128`, `u256`, `address`, and `vector<u8>`.
* Constant names should be upper camel case and begin with an `E` if they represent error codes (e.g., `EIndexOutOfBounds`) and upper snake case if they represent a non-error value (e.g., `MIN_STAKE`).
* `public` constants are not currently supported. `const` values can be used only in the declaring module.
* Constant value cannot be accessed outside its module or script scope. -->
