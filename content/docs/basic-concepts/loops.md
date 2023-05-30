---
title: 循环
weight: 20
---

# 循环

**循环**：

Move 提供了两种定义循环的方法：

* 使用 `while`
* 使用 `loop`（无限循环）

```rust
module my_addrx::Loops
{
    use std::debug::print;
    //lets find the sum of first N natural numbers using while and infinite loop

    //using while loop
    fun sum_using_while(n:u64) :u64
    {
        let sum=0;
        let i:u64=1;  //setting counter to 1
        while(i <= n) //This Loop will run until the expression inside the round brackets is valid
        {
            sum=sum+i;
            i=i+1;  //incrementing the counter
        }; //while is an expression therefore it should be end with a semicolon.
        sum //you can also return in functions like this.
    }
    
    //using infinite loop
    fun sum_using_loop(n:u64) :u64
    {
        let sum=0;
        let i:u64=1;
        loop
        {
            sum=sum+i;
            i=i+1;
            if(i>n)
                break;   //break statement terminates the loops 
        };
        sum
    }



    #[test]
    fun testing()
    {
        let sum = sum_using_while(10);
        print(&sum);
        let sum = sum_using_loop(10);
        print(&sum);
    }
}
```

> *与 `break` 语句类似，还有 `continue` 语句，它通过跳过当前迭代来强制程序执行下一次迭代。*
