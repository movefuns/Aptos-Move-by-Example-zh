# 导入和别名

`use` 语法可用于为其他模块中的成员创建别名。`use` 可用于为整个模块或给定的表达式块范围创建别名。

**句法**：

<!-- # Uses and Aliases

The `use` syntax can be used to create aliases to members in other modules. `use` can be used to create aliases that last either for the entire module, or for a given expression block scope.

**Syntax:** -->

```rust
use <address>::<module name>;
use <address>::<module name> as <module alias name>;
use <address>::<module name>::<module member>;
use <address>::<module name>::<module member> as <member alias>;
use <address>::<module name>::{<module member>, <module member> as <member alias> ... };
```

一些例子：

```rust
use std::vector;
use std::debug as d;
use std::debug::print as p;
```

**`Self`**：

如果除了模块成员之外还需要为模块本身添加别名，则可以使用 `use` 和 `Self` 一次性完成。`Self` 是引用模块的 sorts 的成员。

<!-- If you need to add an alias to the Module itself in addition to module members, you can do that in a single `use` using `Self`. `Self` is a member of sorts that refers to the module. -->

```rust
use std::simple_map::{Self, SimpleMap};
```

<!-- **Inside a module:**

Inside of a `module` all `use` declarations are usable regardless of the order of declaration. -->

**在模块内部**：

在模块内部，无论声明顺序如何，所有 `use` 声明都是可用的。

```rust
module  my_addrx::example {
    use std::vector;

    fun example(): vector<u8> {
        let v = empty();
        vector::push_back(&mut v, 0);
        vector::push_back(&mut v, 10);
        v
    }

    use std::vector::empty;
}
```

<!-- The aliases declared by `use` in the module usable within that module. -->
在该模块中可用的模块中通过 `use` 声明的别名。

<!-- **Inside an expression:** -->

<!-- You can add `use` declarations to the beginning of any expression block -->

**在一个表达式中**：

您可以将 `use` 声明添加到任何表达式块的开头

```rust
module  my_addrx::example {

    fun example(): vector<u8> {
        use std::vector::{empty, push_back};

        let v = empty();
        push_back(&mut v, 0);
        push_back(&mut v, 10);
        v
    }
}
```


<!-- <pre class="language-rust"><code class="lang-rust"><strong>module  my_addrx::example {
</strong>
    fun example(): vector&#x3C;u8> {
        use std::vector::{empty, push_back};

        let v = empty();
        push_back(&#x26;mut v, 0);
        push_back(&#x26;mut v, 10);
        v
    }
}
</code></pre> -->

<!-- As with `let`, the aliases introduced by `use` in an expression block are removed at the end of that block. -->
与 `let` 一样，在表达式块中使用 `use` 引入的别名在该块的末尾被删除。

```rust
module  my_addrx::example {

    fun example(): vector<u8> {
        let result = {
            use std::vector::{empty, push_back};
            let v = empty();
            push_back(&mut v, 0);
            push_back(&mut v, 10);
            v
        };
        result
    }
}
```

<!-- Attempting to use the alias after the block ends will result in an error -->
在块结束后尝试使用别名将导致错误

```rust
fun example(): vector<u8> {
    let result = {
        use std::vector::{empty, push_back};
        let v = empty();
        push_back(&mut v, 0);
        push_back(&mut v, 10);
        v
    };
    let v2 = empty(); // ERROR!
//           ^^^^^ unbound function 'empty'
    result
}
```

<!-- Any `use` must be the first item in the block. If the `use` comes after any expression or `let`, it will result in a parsing error -->
任何 `use` 都必须是块中的第一项。如果 `use` 出现在任何表达式或 `let` 之后，将导致解析错误

```rust
{
    let x = 0;
    use std::vector; // ERROR!
    let v = vector::empty();
}
```

**命名约定**：

别名必须遵循与其他模块成员相同的规则。这意味着结构或常量的别名必须以 `A` 到 `Z` 开头。
<!-- Aliases must follow the same rules as other module members. This means that aliases to structs or constants must start with `A` to `Z`. -->

**Unused Use or Alias:**

An unused `use` will result in an error

**未使用的导入或别名**：

未使用的 `use` 将导致错误

```rust
module my_addrx::Example
{
    use std::debug; //ERROR -> Unused 'use' of alias 'debug'. Consider removing it

    fun printing()
    {
        //do nothing
    }   
}
```
