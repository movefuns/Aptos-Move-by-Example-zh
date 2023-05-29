# 错误

可以通过调用 `assert` 和 `abort` 来抛出错误。

**中止**：

`abort error_code //error code is of type u64`

```rust
module my_addrx::Errors
{
    use std::debug::print;
    use std::string::utf8;

    fun isEven(num:u64)
    {
        if(num%2==0)
        {    
            print(&utf8(b"No Error as the Number is Even"));
        }
        else    
        {    
            abort 11 //throwing error as the given number is not even
        }
    }

    #[test]
    fun testing()
    {
        isEven(3);
    }
}
```

**断言**：

`assert!(expression: bool, error_code: u64)`

```rust
module my_addrx::Errors
{

    fun isEven(num:u64)
    {
        assert!(num%2==0,11);
    }

    #[test]
    fun testing()
    {
        isEven(3);
    }
}
```
