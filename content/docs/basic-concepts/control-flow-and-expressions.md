# 控制流和表达式

控制流语句是导致选择遵循两条或多条路径中的哪条路径的语句。 

Move 通过使用 if-else 表达式和循环来实现控制流。

**如果表达**：

<!-- # Control flow and expressions

Control flow statement is a statement that results in a choice being made as to which of two or more paths to follow.&#x20;

Move achieves control flow by using if-else expressions and loops.

**If expression:** -->

```rust
// syntax for if expression
if(<bool-expression>)
    <expression>
else if(<bool-expression>)
    <expression>
..
..
else 
    <expression>

```

> *对于所有整数类型，除 0 之外的所有值都被视为 true。*

```rust
module my_addrx::IF_ELSE
{
    use std::debug::print;
    use std::string::utf8;
    
    fun control_flow()
    {
        let val:bool = true;
        if(val)
        {
            print(&utf8(b"If block"));
        }
        else{
            print(&utf8(b"Else block"));
        }; //if is an expression therefore it should be end with a semicolon.
    }
    
    #[test]
    fun testing()
    {
        control_flow();
    }
}
```
