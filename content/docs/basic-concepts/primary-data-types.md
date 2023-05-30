---
title: 主要数据类型
weight: 14
---

# 主要数据类型

Move 支持三种原始数据类型：整数、布尔值和地址。

```
Integer: u8,u64,u256
Boolean: true,false
Addresses: address
```

Move 没有字符串类型或浮动类型。

```rust
module my_addrx::PrimitiveTypes
{
    use std::debug::print;

    fun primitive_types() {
        
        //Integers: u8,u64,u128
        let a:u8=10;
        let b:u64=1000;
        let c:u128=10000;
        print(&a); print(&b); print(&c);

        //Boolean: true,false
        let b1:bool=true;
        let b2:bool=false;
        print(&b1); print(&b2);

        //Address: addresses in move are represented by @Variable_name
        let addx1:address=@std;
        let addx2:address=@0x123;
        print(&addx1); print(&addx2);


    }
    
    #[test]
    fun test_primitive_types() {
        primitive_types();
    }
}
```

<!-- # Primary data-types

Move supports  three primitive data types : integer, boolean and addresses.

```
Integer: u8,u64,u256
Boolean: true,false
Addresses: address
```

Move does not have string type or floating type.

```rust
module my_addrx::PrimitiveTypes
{
    use std::debug::print;

    fun primitive_types() {
        
        //Integers: u8,u64,u128
        let a:u8=10;
        let b:u64=1000;
        let c:u128=10000;
        print(&a); print(&b); print(&c);

        //Boolean: true,false
        let b1:bool=true;
        let b2:bool=false;
        print(&b1); print(&b2);

        //Address: addresses in move are represented by @Variable_name
        let addx1:address=@std;
        let addx2:address=@0x123;
        print(&addx1); print(&addx2);


    }
    
    #[test]
    fun test_primitive_types() {
        primitive_types();
    }
}
``` -->
