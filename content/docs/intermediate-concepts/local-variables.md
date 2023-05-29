# 局部变量
Move 中的局部变量是词法（静态）范围的。新变量是用关键字 `let` 引入的，它将隐藏任何以前具有相同名称的局部变量。局部变量是可变的，可以直接更新也可以通过可变引用更新。

**语法**：

<!-- # Local variables

Local variables in Move are lexically (statically) scoped. New variables are introduced with the keyword `let`, which will shadow any previous local with the same name. Locals are mutable and can be updated both directly and via a mutable reference.

**Syntax:** -->

```rust
let <VARIABLE>:TYPE;  //explicit type annotation
let <VARIABLE>=<EXPRESSION>;  //implicit type annotation
```

```rust
module my_addrx::Variable
{
    fun local_variables()
    {
        let b:u8;
        let c=false;
        let d=b"hello world";
        let e:u64=10000;
        let f:address = @my_addrx;
    }
}
```

**变量必须在使用前赋值**

Move 的类型系统防止局部变量在赋值之前被使用。

<!-- **Variables must be assigned before use**

Move's type system prevents a local variable from being used before it has been assigned. -->

```rust
let a;
let b=a+10; //ERROR
```

**有效的变量名**

变量名可以包含下划线 `_`、字母 `a` 到 `z`、字母 `A` 到 `Z` 和数字 `0` 到 `9`。变量名必须以下划线 `_` 或字母 `a` 到 `z` 开头。它们不能以大写字母开头。

<!-- **Valid variable names**

Variable names can contain underscores `_`, letters `a` to `z`, letters `A` to `Z`, and digits `0` to `9`. Variable names must start with either an underscore `_` or a letter `a` through `z`. They _cannot_ start with uppercase letters. -->

```rust
// all valid
let x = 1;
let _x = 1;
let _A = 1;
let x0 = 1;
let xA = 1;
let foobar_123 = 1;

// all invalid
let X = 1; // ERROR!
let Foo = 1; // ERROR!
```

**带有元组的多个声明**

`let` 可以使用元组一次引入多个本地。括号内声明的局部变量被初始化为元组中的相应值。

<!-- **Multiple declarations with tuples**

`let` can introduce more than one local at a time using tuples. The locals declared inside the parenthesis are initialized to the corresponding values from the tuple. -->

```rust
//Valid
let (x,y,z) = (1,@0x1,false);
//Invalid
let (p,q) = (1,2,3,4);
```

**未使用变量的下划线**

在 Move 中必须使用每个变量（否则你的代码将无法编译），因此你不能初始化一个变量并保持不变。尽管您有一种方法可以将变量标记为*有意未使用*——使用下划线 `_`。

<!-- **Underscore for unused variables**

In Move every variable must be used (otherwise your code won't compile), hence you can't initialize one and leave it untouched. Though you have one way to mark variable as _intentionally unused_ - by using underscore `_`. -->

```rust
module my_addrx::Variable
{
    fun local_variables()
    {
        let a=1;   //compiling this module will result in error as "a" is not being used.
    }
}
```

```rust
module my_addrx::Variable
{
    fun local_variables()
    {
        let _=1; //compiling this module will not result in error
        
        let b=10;
        _=b;    //This is called shadowing
    }
}
```
