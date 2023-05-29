# 地址

`address` 是 Move 中的内置类型，用于表示全局存储中的位置（有时称为帐户）。地址值是一个 256 位（32 字节）的标识符。在给定的地址，可以存储两件事：模块和资源。

虽然地址在底层是一个 256 位整数，但 Move 地址是有意不透明的——它们不能从整数创建，它们不支持算术运算，也不能被修改。尽管可能有有趣的程序会使用这样的功能（例如，C 中的指针算法填补了类似的空白），但 Move 不允许这种动态行为，因为它是从头开始设计来支持静态验证的。

您可以使用运行时地址值（`address`（地址）类型的值）访问该地址处的资源。  您不能在运行时通过地址值访问模块。

**地址及其语法**：

地址有两种形式，*命名的*或*数字的*。命名地址的语法遵循 Move 中任何命名标识符的相同规则。数字地址的语法不限于十六进制编码值，任何有效的 `u256` 数值都可以用作地址值，例如 `42`、`0xCAFE` 和 `2021` 都是有效的数字地址字面量。

为了区分地址何时在表达式上下文中使用，使用地址时的语法因使用地址的上下文而异：

当地址用作表达式时，地址必须以 `@` 字符为前缀，即 `@<numerical_value>` 或 `@<named_address_identifier>`。

在表达式上下文之外，地址可以不带前导 `@` 字符写入，即 `<numerical_value>` 或 `<named_address_identifier>`。

通常，您可以将 `@` 视为一个运算符，它将地址从名称空间项转换为表达式项。

```rust
let addrx1:address = @0x1; //numerical address example
let addrx2:address = @my_addrx;//named address example
```

**全局存储操作**：

地址值的主要目的是与全局存储操作交互。

地址值用于 `exists`、`borrow_global`、`borrow_global_mut` 和 `move_from` 操作。

唯一*不*使用地址的全局存储操作是 `move_to`，它使用 `signer`。

**所有权**：

与语言内置的其他标量值一样，地址值是隐式可复制的，这意味着它们可以在没有显式指令（如复制）的情况下复制。

<!-- # Address

`address` is a built-in type in Move that is used to represent locations (sometimes called accounts) in global storage. An `address` value is a 256-bit (32-byte) identifier. At a given address, two things can be stored: Module and Resource.

Although an `address` is a 256-bit integer under the hood, Move addresses are intentionally opaque---they cannot be created from integers, they do not support arithmetic operations, and they cannot be modified. Even though there might be interesting programs that would use such a feature (e.g., pointer arithmetic in C fills a similar niche), Move does not allow this dynamic behavior because it has been designed from the ground up to support static verification.

You can use runtime address values (values of type `address`) to access resources at that address. You _cannot_ access modules at runtime via address values.

**Addresses and their syntax:**

Addresses come in two flavors, _named_ or _numerical_. The syntax for a named address follows the same rules for any named identifier in Move. The syntax of a numerical address is not restricted to hex-encoded values, and any valid `u256`  numerical value can be used as an address value, e.g., `42`, `0xCAFE`, and `2021` are all valid numerical address literals.

To distinguish when an address is being used in an expression context or not, the syntax when using an address differs depending on the context where it's used:

* When an address is used as an expression the address must be prefixed by the `@` character, i.e., `@<numerical_value>`or `@<named_address_identifier>`.
* Outside of expression contexts, the address may be written without the leading `@` character, i.e., `<numerical_value>` or `<named_address_identifier>`.

In general, you can think of `@` as an operator that takes an address from being a namespace item to being an expression item.

```rust
let addrx1:address = @0x1; //numerical address example
let addrx2:address = @my_addrx;//named address example
```

**Global Storage Operations:**

The primary purpose of `address` values are to interact with the global storage operations.

`address` values are used with the `exists`, `borrow_global`, `borrow_global_mut`, and `move_from` operations.

The only global storage operation that _does not_ use `address` is `move_to`, which uses `signer`.

**Ownership:**

As with the other scalar values built-in to the language, `address` values are implicitly copyable, meaning they can be copied without an explicit instruction such as `copy`. -->
