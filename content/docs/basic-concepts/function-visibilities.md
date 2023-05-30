---
title: 函数可见性
weight: 18
---

# 函数可见性

move 中的函数可以声明为：

**private** - 默认情况下，move 中的函数是私有的（这意味着它们只能在同一模块内调用，其他模块和脚本不能在模块外访问。
**public** - 公共函数可以被任何模块或脚本中的任何函数调用。
**public(friend)** - public(friend) 函数可以被同一模块中的任何函数调用，也可以被在好友列表中明确定义的模块的函数调用。
**entry** - 入口函数是移动程序开始执行的函数。它们可以与 public 和 public(friend) 修饰符结合使用。

<!-- Functions in move can be declared as:

**private** - By default the functions in move are private(that means they can only be called within the same module and cannot be access outside the module by other modules and scripts.

**public** - A public function can be called by any function in any module or script.

**public(friend)** - A public(friend) function can be called by any function in the same module and by the function of the module which are explicitly defined in the friend list.

**entry** - Entry function are the function where move program starts its execution. They can be combined with public and public(friend) modifier. -->

```rust
address my_addrx{
module A{
    fun A_foo():u8{    //private function
        return 123
    }
}

module B{
    use std::debug::print;
    fun B_foo():u8{
        return my_addrx::A::A_foo()  //ERROR - as A_foo() is a private function
    }

    #[test]
    fun testing_B()
    {
        let number=B_foo();
        print(&number);
    }
}
}
```

```rust
address my_addrx{
module A{
    public fun A_foo():u8{    //public function
        return 123
    }
}

module B{
    use std::debug::print;
    fun B_foo():u8{
        return my_addrx::A::A_foo()  //Module B can call A_foo() as it is a public function
    }

    #[test]
    fun testing_B()
    {
        let number=B_foo();
        print(&number);
    }
}
}
```

```rust
address my_addrx{
module X{
    friend my_addrx::Y; //declaring friends of module Y

    public(friend) fun X_foo():u8{
        return 123
    }   
}

module Y{
    fun Y_foo():u8{
        return my_addrx::X::X_foo() //Module Y can call X_foo() as Y is in friend list of X
    }
}

module Z{
    fun Z_foo():u8{
        return my_addrx::X::X_foo() //ERROR - as module Z is not in friend list of X
    }   
}
}
```

```rust
module my_addrx::E{
    use std::debug::print;
    use std::string::utf8;
    public entry fun foo(){   //entry function can be private,public or public(friend)
        print(&utf8(b"This is an entry function"));
    }
    #[test]
    fun testing()
    {
        foo();
    }
}
```
