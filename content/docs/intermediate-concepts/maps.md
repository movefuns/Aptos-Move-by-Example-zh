---
title: Map
weight: 32
---

# Map

**`0x1::simple_map`**

该模块为排序映射提供了一种解决方案，即它具有以下属性

* 键指向值
* 每个 `key` 必须是唯一的
* 可以在 O(Log N) 时间内找到密钥
* 数据按 `key` 排序存储
* 添加和删除需要 O(N) 时间

**定义空 map**：

<!-- # Maps

**`0x1::simple_map`**

This module provides a solution for sorted maps, that is it has the properties that

* Keys point to Values
* Each Key must be unique
* A Key can be found within O(Log N) time
* The data is stored as sorted by Key
* Adds and removals take O(N) time

**Defining an empty map:** -->

```rust
let NAME : SimpleMap<TYPE1,TYPE2> = simple_map::create();
```

<!-- **Operations on maps:**

Lets understand various _operations_ on maps using the module given below. -->
**map 操作**：

让我们使用下面给出的模块来了解 map 上的各种操作。

```rust
module my_addrx::Mapping
{
    use std::simple_map::{SimpleMap,Self};
    use std::string::{String,utf8};

    fun mapping_in_move()
    {
        let mp:SimpleMap<u64,String> = simple_map::create(); //creating an empty map where Key->integer and Value->string.
        
        
        //adding the key and corresponding value in the map
        simple_map::add(&mut mp,1 ,utf8(b"John")); 
        simple_map::add(&mut mp,2 ,utf8(b"Ben"));
        simple_map::add(&mut mp,3 ,utf8(b"Tony"));
        simple_map::add(&mut mp,4 ,utf8(b"Gwen"));
      
        //calculating the length of the map
        let l=simple_map::length(&mut mp);
        assert!(l==4,1);

        //checking if a given key exists or not in the vector
        let x=simple_map::contains_key(&mut mp,&2);
        assert!(x==true,1);

        //removing key value pair in the map
        simple_map::remove(&mut mp,&2);
        assert!(simple_map::contains_key(&mut mp,&2)==false,1);

        //borrowing immutable reference to the value of a given key        
        let v=simple_map::borrow(&mut mp,&3);
        assert!(v==&utf8(b"Tony"),1);

        //borrowing mutable reference to the value of a given key        
        let v=simple_map::borrow_mut(&mut mp,&3);
        *v=utf8(b"Changed Name");
        
        assert!(simple_map::borrow(&mut mp,&3)==&utf8(b"Changed Name"),1);


    }

    #[test]
    fun testing()
    {
        mapping_in_move();
    }
}t
```

有关 map 的更多信息，请关注[这个链接](https://github.com/aptos-labs/aptos-core/blob/mainnet/aptos-move/framework/aptos-stdlib/doc/simple\_map.md#@Specification\_1\_create)。

<!-- {% hint style="info" %}
For more information on maps follow [this link.](https://github.com/aptos-labs/aptos-core/blob/mainnet/aptos-move/framework/aptos-stdlib/doc/simple\_map.md#@Specification\_1\_create)
{% endhint %} -->
