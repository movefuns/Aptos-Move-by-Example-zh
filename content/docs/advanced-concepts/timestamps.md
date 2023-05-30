---
title: 时间戳
weight: 43
---

# 时间戳

**模块 `0x1::timestamp`**

该模块保留一个全局挂钟，以微秒为单位存储当前 Unix 时间。

**`now_microseconds`**

获取当前时间（以微秒为单位）。

**`now_seconds`**

以秒为单位获取当前时间。

<!-- # Timestamps

**Module `0x1::timestamp`**

This module keeps a global wall clock that stores the current Unix time in microseconds.

**`now_microseconds`**

Gets the current time in microseconds.

**`now_seconds`**

Gets the current time in seconds. -->

```rust
module my_addrx::UnderstandingTimestamp{
  
    use std::timestamp; 
    
    public entry fun time()
    {
        let t1=timestamp::now_microseconds();
        std::debug::print(&t1);
        
        let t2=timestamp::now_seconds();
        std::debug::print(&t2);
    }
 
 
     #[test(framework = @0x1)]
    fun testing(framework: signer)
    {
        // set up global time for testing purpose
        timestamp::set_time_has_started_for_testing(&framework);   
        time(); 
    }   
}
```
