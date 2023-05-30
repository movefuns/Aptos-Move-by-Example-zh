---
title: 结构和能力
weight: 22
---

# 结构和能力

Struct 是一种在 move 中定义自定义类型的方法。它可以被描述为一个简单的`键-值`存储，其中键是属性的名称，值是存储的内容。

语法：

```rust
struct NAME has Abilities
{
    FIELD1 : TYPE1,
    FIELD2 : TYPE2,
    ...
}
```

**Struct** 最多可以有 4 个能力来定义如何使用、删除或存储这种类型的值。

`copy` - 可以复制值（或按值克隆）。
`drop` - 值可以在范围末尾被删除。
`key` - value 可以作为全局存储操作的键。
`store` - 值可以存储在全局存储中。

> 整数、向量、地址和布尔值等原始类型的能力是预定义且不可更改的：复制、删除和存储。

<!-- **Struct** can have up to 4 abilities which define how values of this type can be used, dropped or stored.

* **Copy** - value can be _copied_ (or cloned by value).
* **Drop** - value can be _dropped_ by the end of scope.
* **Key** - value can be _used as a key_ for global storage operations.
* **Store** - value can be _stored_ inside global storage.

{% hint style="info" %}
Abilities for Primitive types like _integers_, _vector_, _addresses_ and _boolean_  are pre-defined and unchangeable : _copy_, _drop_ and _store_.
{% endhint %} -->

```rust
module my_addrx::Application {
	use std::vector;
        use std::debug;
	use std::string::{String,utf8};

	struct Users has key,drop {
		list_of_users: vector<User>    //storing the list of the users
	}

	struct User has store,drop,copy {
		name:String,                   //information required for a typical user
		age:u8
	}

        //creating a user by adding the user to the existing list and returning the user
	public fun create_user(newUser: User,users: &mut Users): User{
		vector::push_back(&mut users.list_of_users,newUser);
		return newUser
	}
	
	#[test]
	fun test_create_friend(){
		let user1 = User {
			name:utf8(b"Tony"),
			age:50,
		};
		
        let users = Users{
			list_of_users: vector::empty<User>()
		};

		let createdUser = create_user(user1,&mut users);
        debug::print(&users);
        assert!(createdUser.name == utf8(b"Tony"),0);
	}
}
```
