# 虚类型参数

未使用的类型参数可以标记为幻像类型参数，不参与结构体的能力推导。这样，在派生泛型的能力时不考虑幻像类型参数的参数，从而避免了虚假能力注释的需要。为了使这个宽松的规则合理，Move 的类型系统保证声明为 phantom 的参数在结构定义中根本不使用，或者仅用作也声明为 phantom 的类型参数的参数。

**声明**：

在结构定义中，可以通过在声明前添加 *phantom* 关键字将类型参数声明为 phantom。如果一个类型参数被声明为 phantom，我们就说它是一个 phantom 类型参数。定义结构时，Move 的类型检查器确保每个幻像类型参数要么未在结构定义中使用，要么仅用作幻像类型参数的参数。

更正式地说，如果一个类型被用作幻像类型参数的实参，我们说该类型出现在幻像位置。有了这个定义，正确使用幻影参数的规则可以指定如下：幻影类型参数只能出现在幻影位置。

以下两个示例显示了虚拟参数的有效使用。在第一个中，参数 `T1` 在结构定义中根本没有使用。在第二个中，参数 `T1` 仅用作幻像类型参数的参数。

<!-- # Phantom Type Parameters

Unused type parameters can be marked as _phantom_ type parameters, which do not participate in the ability derivation for structs. In this way, arguments to phantom type parameters are not considered when deriving the abilities for generic types, thus avoiding the need for spurious ability annotations. For this relaxed rule to be sound, Move's type system guarantees that a parameter declared as `phantom` is either not used at all in the struct definition, or it is only used as an argument to type parameters also declared as `phantom`.

**Declaration**

In a struct definition a type parameter can be declared as phantom by adding the `phantom` keyword before its declaration. If a type parameter is declared as phantom we say it is a phantom type parameter. When defining a struct, Move's type checker ensures that every phantom type parameter is either not used inside the struct definition or it is only used as an argument to a phantom type parameter.

More formally, if a type is used as an argument to a phantom type parameter we say the type appears in _phantom position_. With this definition in place, the rule for the correct use of phantom parameters can be specified as follows: **A phantom type parameter can only appear in phantom position**.

The following two examples show valid uses of phantom parameters. In the first one, the parameter `T1` is not used at all inside the struct definition. In the second one, the parameter `T1` is only used as an argument to a phantom type parameter. -->

```rust
struct S1<phantom T1, T2> { f: u64 }
                  ^^
                  Ok: T1 does not appear inside the struct definition


struct S2<phantom T1, T2> { f: S1<T1, T2> }
                                  ^^
                                  Ok: T1 appears in phantom position
```

<!-- The following code shows examples of violations of the rule: -->
以下代码显示了违反规则的示例：

```rust
struct S1<phantom T> { f: T }
                          ^
                          Error: Not a phantom position

struct S2<T> { f: T }

struct S3<phantom T> { f: S2<T> }
                             ^
                             Error: Not a phantom position
```

<!-- **Instantiation**

When instantiating a struct, the arguments to phantom parameters are excluded when deriving the struct abilities. For example, consider the following code``:`` -->

**实例化**

实例化结构时，在派生结构能力时将排除幻形参数的实参。例如，考虑以下代码：

```rust
struct S<T1, phantom T2> has copy { f: T1 }
struct NoCopy {}
struct HasCopy has copy {}
```

<!-- Consider now the type `S<HasCopy, NoCopy>`. Since `S` is defined with `copy` and all non-phantom arguments have `copy` then `S<HasCopy, NoCopy>` also has `copy`.

**Phantom Type Parameters with Ability Constraints**[**​**](https://aptos.dev/guides/move-guides/book/generics#phantom-type-parameters-with-ability-constraints)

Ability constraints and phantom type parameters are orthogonal features in the sense that phantom parameters can be declared with ability constraints. When instantiating a phantom type parameter with an ability constraint, the type argument has to satisfy that constraint, even though the parameter is phantom. For example, the following definition is perfectly valid:

```rust
struct S<phantom T: copy> {}
```

The usual restrictions apply and `T` can only be instantiated with arguments having `copy`. -->


现在考虑类型 `S<HasCopy, NoCopy>`。由于 `S` 是用副本定义的，并且所有非幻影参数都有副本，因此 `S<HasCopy`, NoCopy>` 也有副本。

具有能力约束的虚类型参数
 
能力约束和幻影类型参数是正交特征，幻影参数可以用能力约束来声明。当使用能力约束实例化幻影类型参数时，类型参数必须满足该约束，即使该参数是幻影。例如，以下定义是完全有效的：

```rust
struct S<phantom T: copy> {}
```

通常的限制适用并且 `T` 只能用具有副本的参数实例化。
