# 注释

注释是一段不可执行的代码，程序员可以添加这些代码以使其代码可读。

```rust
module my_addrx::Comments
{
    use std::debug::print;
    
    fun printing()
    {
        //This is a line comment

        /*
            This 
            is a
            block
            comment
        */
        
        let a=/*You can put block comment here*/12345;
        print(&/*Even here*/a);
        
    }

    #[test]
    
    fun testing()
    {
        printing();
    }
}
```
