# 引用

Move 提供两种类型的引用：

* 不可变 `&` - 不能修改基础值（或其任何字段）。
* mutable `&mut` - 允许通过对该引用的写入进行修改。

**参考运营商**：

这里的符号 `e: T` 代表“表达式 `e` 具有类型 `T`”。

# References

<!-- Move provides two types of references:

* immutable `&` - cannot modify underlying values (or any of its fields).
* mutable `&mut` - allow for modifications via a write through that reference.

**Reference Operators:**

Here the notation `e: T` stands for "expression `e` has type `T`". -->

| Syntax      | Type                                                  | Description                                                    |
| ----------- | ----------------------------------------------------- | -------------------------------------------------------------- |
| `&e`        | `&T` where `e: T` and `T` is a non-reference type     | Create an immutable reference to `e`                           |
| `&mut e`    | `&mut T` where `e: T` and `T` is a non-reference type | Create a mutable reference to `e`.                             |
| `&e.f`      | `&T` where `e.f: T`                                   | Create an immutable reference to field `f` of struct `e`.      |
| `&mut e.f`  | `&mut T` where `e.f: T`                               | Create a mutable reference to field `f` of struct`e`.          |
| `freeze(e)` | `&T` where `e: &mut T`                                | Convert the mutable reference `e` into an immutable reference. |

`&e.f` 和 `&mut e.f` 运算符既可用于在结构中创建新引用，也可用于扩展现有引用：

<!-- The `&e.f` and `&mut e.f` operators can be used both to create a new reference into a struct or to extend an existing reference: -->

```rust
let s = S { f: 10 };
let f_ref1: &u64 = &s.f; // works
let s_ref: &S = &s;
let f_ref2: &u64 = &s_ref.f // also works
```

只要两个结构在同一个模块中，具有多个字段的引用表达式就可以工作：
<!-- A reference expression with multiple fields works as long as both structs are in the same module: -->

```rust
struct A { b: B }
struct B { c : u64 }
fun f(a: &A): &u64 {
  &a.b.c
}
```

最后，请注意不允许引用引用：
<!-- Finally, note that references to references are not allowed: -->

```rust
let x = 7;
let y: &u64 = &x;
let z: &&u64 = &y; // will not compile
```

**通过参考文献读写**：

可以读取可变和不可变引用以生成引用值的副本。

只能写入可变引用。写入 `*x = v` 会丢弃先前存储在 `x` 中的值并用 `v` 更新它。

<!-- **Reading and writing through references:**

Both mutable and immutable references can be read to produce a copy of the referenced value.

Only mutable references can be written. A write `*x = v` discards the value previously stored in `x` and updates it with `v`. -->

| Syntax    | Type                                                                                 | Description                         |
| --------- | ------------------------------------------------------------------------------------ | ----------------------------------- |
| `*e`      | `T` where `e` is `&T` or `&mut T`                                                    | Read the value pointed to by `e`    |
| \*e1 = e2 | <p><code>()</code> where <code>e1: &#x26;mut T</code> and <code>e2: T</code><br></p> | Update the value in `e1` with `e2`. |

<!-- **Freeze inference:**

A mutable reference can be used in a context where an immutable reference is expected: -->

<!-- **冻结推理**： -->
**禁用推断**：

可变引用可以用在需要不可变引用的上下文中：

```rust
let x = 7;
let y: &u64 = &mut x;
```

这是有效的，因为在底层下，编译器在需要的地方插入 `freeze` 指令。

更多冻结推理的例子：
<!-- This works because the under the hood, the compiler inserts `freeze` instructions where they are needed.

More examples of freeze inference in action: -->

```rust
fun takes_immut_returns_immut(x: &u64): &u64 { x }

// freeze inference on return value
fun takes_mut_returns_immut(x: &mut u64): &u64 { x }

fun expression_examples() {
    let x = 0;
    let y = 0;
    takes_immut_returns_immut(&x); // no inference
    takes_immut_returns_immut(&mut x); // inferred freeze(&mut x)
    takes_mut_returns_immut(&mut x); // no inference

    assert!(&x == &mut y, 42); // inferred freeze(&mut y)
}

fun assignment_examples() {
    let x = 0;
    let y = 0;
    let imm_ref: &u64 = &x;

    imm_ref = &x; // no inference
    imm_ref = &mut y; // inferred freeze(&mut y)
}
```

**子类型**：

通过这个 `freeze` 推断，Move 类型检查器可以将 `&mut T` 视为 `&T` 的子类型。如上所示，这意味着对于任何使用 `&T` 值的表达式，也可以使用 `&mut T` 值。该术语用于错误消息中，以简明地指示在提供 `&T` 的地方需要 `&mut T`。例如

<!-- **Subtyping:**

With this `freeze` inference, the Move type checker can view `&mut T` as a subtype of `&T`. As shown above, this means that anywhere for any expression where a `&T` value is used, a `&mut T` value can also be used. This terminology is used in error messages to concisely indicate that a `&mut T` was needed where a `&T` was supplied. For example -->

```rust
address 0x42 {
module example {
    fun read_and_assign(store: &mut u64, new_value: &u64) {
        *store = *new_value
    }

    fun subtype_examples() {
        let x: &u64 = &0;
        let y: &mut u64 = &mut 1;

        x = &mut 1; // valid
        y = &2; // invalid!

        read_and_assign(y, x); // valid
        read_and_assign(x, y); // invalid!
    }
}
}
```

<!-- will yield the following error messages -->
将产生以下错误消息

```rust
error:

    ┌── example.move:12:9 ───
    │
 12 │         y = &2; // invalid!
    │         ^ Invalid assignment to local 'y'
    ·
 12 │         y = &2; // invalid!
    │             -- The type: '&{integer}'
    ·
  9 │         let y: &mut u64 = &mut 1;
    │                -------- Is not a subtype of: '&mut u64'
    │

error:

    ┌── example.move:15:9 ───
    │
 15 │         read_and_assign(x, y); // invalid!
    │         ^^^^^^^^^^^^^^^^^^^^^ Invalid call of '0x42::example::read_and_assign'. Invalid argument for parameter 'store'
    ·
  8 │         let x: &u64 = &0;
    │                ---- The type: '&u64'
    ·
  3 │     fun read_and_assign(store: &mut u64, new_value: &u64) {
    │                                -------- Is not a subtype of: '&mut u64'
    │
```

<!-- The only other types currently that has subtyping are tuples.

**Ownership:**

Both mutable and immutable references can always be copied and extended _even if there are existing copies or extensions of the same reference_: -->

当前唯一具有子类型的其他类型是元组。

**所有权**：

可变和不可变引用始终可以被复制和扩展，即使存在相同引用的现有副本或扩展：

```rust
fun reference_copies(s: &mut S) {
  let s_copy1 = s; // ok
  let s_extension = &mut s.f; // also ok
  let s_copy2 = s; // still ok
  ...
}
```

<!-- This might be surprising for programmers familiar with Rust's ownership system, which would reject the code above. Move's type system is more permissive in its treatment of copies, but equally strict in ensuring unique ownership of mutable references before writes. -->

对于熟悉 Rust 的所有权系统的程序员来说，这可能会令人惊讶，因为它会拒绝上面的代码。Move 的类型系统在处理副本方面更为宽松，但在写入前确保可变引用的唯一所有权方面同样严格。
