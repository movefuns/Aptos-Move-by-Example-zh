# 所有权

在 Move 编程语言中，所有权是一个用于管理内存和资源的概念。所有权基于这样一种思想，即每个资源一次只能有一个所有者，并且所有权可以从一个所有者转移到另一个所有者。这允许 Move 加强内存安全，防止数据竞争，并实现高效的内存管理。

在 Move 中，所有权由借用检查器强制执行，它检查资源在移动或借用后是否未被使用。Move 也有一个线性类型系统，这意味着资源只能被消耗一次，必须按照特定的顺序移动或借用。

所有者是拥有变量的范围。变量可以在此范围内定义（例如使用关键字 `let`）或作为参数传递到范围内。由于 Move 中唯一的范围是函数的 - 没有其他方法可以将变量放入范围。

每个变量只有一个所有者，这意味着当一个变量作为参数传递给函数时，这个函数成为新的所有者，并且该变量不再属于第一个函数。或者你可以说这个其他函数获得了变量的所有权。

**移动和复制**

首先，您需要了解 Move VM 的工作原理，以及将您的值传递给函数时会发生什么。VM 中有两条字节码指令：MoveLoc 和 CopyLoc - 它们都可以分别通过关键字 `move` 和 `copy` 手动使用。

当一个变量被传递到另一个函数时——它被移动并使用 MoveLoc OpCode。让我们看看如果我们使用关键字移动，我们的代码会是什么样子：

<!-- # Ownership -->

<!-- In the Move programming language, ownership is a concept that is used to manage memory and resources. Ownership is based on the idea that each resource can only have one owner at a time, and ownership can be transferred from one owner to another. This allows Move to enforce memory safety, prevent data races, and enable efficient memory management. -->

<!-- In Move, ownership is enforced by the borrow checker, which checks that a resource is not used after it has been moved or borrowed. Move also has a linear type system, which means that resources can only be consumed once and must be moved or borrowed in a specific order. -->

<!-- Owner is a scope which _owns_ a variable. Variables either can be defined in this scope (e.g. with keyword `let`) or be passed into the scope as arguments. Since the only scope in Move is function's - there are no other ways to put variables into scope. -->

<!-- Each variable has only one owner, which means that when a variable is passed into function as argument, this function becomes the _new owner_, and the variable is no longer _owned_ by the first function. Or you may say that this other function _takes ownership_ of the variable. -->

<!-- **Move and Copy** -->

<!-- First, you need to understand how Move VM works, and what happens when you pass your value into a function. There are two bytecode instructions in VM: _MoveLoc_ and _CopyLoc_ - both of them can be manually used with keywords `move` and `copy` respectively. -->

<!-- When a variable is being passed into another function - it's being _moved_ and _MoveLoc_ OpCode is used. Let's see how our code would look if we've used keyword `move`: -->

```rust
script {
    use {{sender}}::M;

    fun main() {
        let a : Module::T = Module::create(10);

        M::value(move a); // variable a is moved

        // local a is dropped
    }
}

```

这是一个有效的 Move 代码，但是，知道该值仍将被移动，您不需要显式移动它。现在，当一切都清楚时，我们就可以开始复制了。

**关键字复制**

如果您需要将一个值传递给函数（它被移动的地方）并保存变量的副本，您可以使用关键字 `copy`。

<!-- This is a valid Move code, however, knowing that value will still be moved you don't need to explicitly _move_ it. Now when it's clear we can get to _copy_.

**Keyword copy**

If you need to pass a value to the function (where it's being moved) and save a copy of your variable, you can use keyword `copy`. -->

```rust
script {
    use {{sender}}::M;

    fun main() {
        let a : Module::T = Module::create(10);

        // we use keyword copy to clone structure
        // can be used as `let a_copy = copy a`
        M::value(copy a);
        M::value(a); // won't fail, a is still here
    }
}
```

<!-- In this example we've passed a _copy_ of a variable (hence value) `a` into the first call of the method `value` and saved `a` in local scope to use it again in a second call.

By copying a value we've duplicated it and increased the memory size of our program, so it can be used - but when you copy huge data it may become pricey in terms of memory. In blockchains every byte counts and affects the price of execution, so using `copy` all the time may cost you a lot. -->

在此示例中，我们将变量（因此值）`a` 的副本传递到方法 `value` 的第一次调用中，并将 `a` 保存在局部范围内以在第二次调用中再次使用它。

通过复制一个值，我们复制了它并增加了我们程序的内存大小，因此它可以被使用——但是当你复制大量数据时，它可能会在内存方面变得昂贵。在区块链中，每个字节都很重要并影响执行价格，因此一直使用 `copy `可能会花费很多。
