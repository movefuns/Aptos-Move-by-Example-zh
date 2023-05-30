---
title: 全局存储结构
weight: 40
---

# 全局存储结构

在 move 中，全局存储结构是一个持久化存储区域，允许开发人员将数据存储在区块链上。

MOVE 中的全局存储结构由一个映射数据类型组成，类似于其他编程语言中的哈希表或字典。这种映射数据类型允许开发人员存储键值对，其中键是唯一标识符，值是要存储的数据。

Move 程序的目的是读取和写入树形持久全局存储。程序无法访问此树之外的文件系统、网络或任何其他数据。

在伪代码中，全局存储类似于：

```rust
struct GlobalStorage {
  resources: Map<(address, ResourceType), ResourceValue>
  modules: Map<(address, ModuleName), ModuleBytecode>
}
```

在结构上，全局存储是一个森林，由以帐户地址为根的树组成。每个地址都可以存储资源数据值和模块代码值。正如上面的伪代码所示，每个地址最多可以存储一个给定类型的资源值和一个给定名称的模块。

<!-- # Global Storage Structure

In move the global storage structure is a persistent storage area that allows developers to store data on the blockchain.

The global storage structure in MOVE consists of a mapping data type, which is similar to a hash table or dictionary in other programming languages. This mapping data type allows developers to store key-value pairs, where the key is a unique identifier and the value is the data to be stored.

The purpose of Move programs is to `read from` and `write to` tree-shaped persistent global storage. Programs cannot access the filesystem, network, or any other data outside of this tree.

In pseudocode, the global storage looks something like:

```rust
struct GlobalStorage {
  resources: Map<(address, ResourceType), ResourceValue>
  modules: Map<(address, ModuleName), ModuleBytecode>
}
```

Structurally, global storage is a forest consisting of trees rooted at an account address. Each address can store both resource data values and module code values. As the pseudocode above indicates, each `address` can store at most one resource value of a given type and at most one module with a given name. -->
