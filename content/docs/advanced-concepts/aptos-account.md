---
title: Aptos 账户
weight: 47
---

# Aptos 账户

Aptos 区块链上的账户包含区块链资产。这些资产，例如硬币和 NFT，本质上是稀缺的，必须进行访问控制。任何此类资产都在区块链帐户中表示为资源。资源是一种 Move 语言原语，在其表示中强调访问控制和稀缺性。但是，资源也可以用来表示其他链上功能、识别信息和访问控制。

Aptos 区块链上的每个账户都由一个 32 字节的账户地址标识。账户可以存储数据，账户将这些数据存储在资源中。初始资源是帐户数据本身（身份验证密钥和序列号）。创建帐户后会添加其他资源，如货币或 NFT。你可以雇用在 

为重要客户保护 .apt 域，使他们令人难忘和独一无二。

与其他账户和地址是隐式的区块链不同，Aptos 上的账户是显式的，需要创建才能持有资源和模块。该帐户可以通过在那里转移 Aptos 代币 (APT) 来显式或隐式地创建。见 

部分了解更多详情。在某种程度上，这类似于其他链，在其他链中，地址需要先发送资金以获取燃料，然后才能发送交易。
显式帐户允许一流的功能，这些功能在其他网络上不可用，例如：

* 旋转认证密钥。帐户的身份验证密钥可以更改为通过不同的私钥进行控制。这类似于在 web2 世界中更改密码。

* 原生多重签名支持。Aptos 上的帐户支持 multi-ed25519，它允许在构造身份验证密钥时使用多重签名身份验证方案。未来可以很容易地引入更多的认证方案。

* 与生态系统的其余部分进行更多集成，以引入可以与 Aptos 帐户无缝集成的配置文件、域名等功能。

Aptos 中有两种类型的账户：

* 标准账户——这是一个典型的账户，对应于具有相应公钥/私钥对的地址。

* 资源账户——没有相应私钥的自治账户，供开发者用于存储资源或发布链上模块。

帐户地址为 32 字节。它们通常显示为 64 个十六进制字符，每个十六进制字符一个半字节。有时地址以 0x 为前缀。

```rust
Alice: 0xeeff357ea5c1a4e7bc11b2b17ff2dc2dcca69750bfef1e1ebcaccf8c8018175b
Bob: 0x19aadeca9388e009d136245b9a67423f3eee242b03142849eb4f81a4a409e59c
```

<!-- # Aptos account

An account on the Aptos blockchain contains blockchain assets. These assets, for example, coins and NFTs, are by nature scarce and must be access-controlled. Any such asset is represented in the blockchain account as a **resource**. A resource is a Move language primitive that emphasizes access control and scarcity in its representation. However, a resource can also be used to represent other on-chain capabilities, identifying information, and access control.

Each account on the Aptos blockchain is identified by a 32-byte account address. An account can store data, and the account stores this data in resources. The initial resource is the account data itself (authentication key and sequence number). Additional resources like currency or NFTs are added after creating the account. And you can employ the [Aptos Name Service](https://aptos.dev/integration/aptos-names-service-package) at [www.aptosnames.com](https://www.aptosnames.com/) to secure .apt domains for key accounts to make them memorable and unique.

Different from other blockchains where accounts and addresses are implicit, accounts on Aptos are explicit and need to be created before they can hold resources and modules. The account can be created explicitly or implicitly by transferring Aptos tokens (APT) there. See the [Creating an account](https://aptos.dev/concepts/accounts#creating-an-account) section for more details. In a way, this is similar to other chains where an address needs to be sent funds for gas before it can send transactions.

Explicit accounts allow first-class features that are not available on other networks such as:

* Rotating authentication key. The account's authentication key can be changed to be controlled via a different private key. This is similar to changing passwords in the web2 world.
* Native multisig support. Accounts on Aptos support multi-ed25519 which allows for a multisig authentication scheme when constructing the authentication key. In the future, more authentication schemes can be introduced easily.
* More integration with the rest of ecosystem to bring in features such as profiles, domain names, etc. that can be seamlessly integrated with the Aptos account.

There are two types of accounts in Aptos:

* _Standard account_ - This is a typical account corresponding to an address with a corresponding pair of public/private keys.
* _Resource account_ - An autonomous account without a corresponding private key used by developers to store resources or publish modules on chain.

Account addresses are 32-bytes. They are usually shown as 64 hex characters, with each hex character a nibble. Sometimes the address is prefixed with a 0x. -->

```rust
Alice: 0xeeff357ea5c1a4e7bc11b2b17ff2dc2dcca69750bfef1e1ebcaccf8c8018175b
Bob: 0x19aadeca9388e009d136245b9a67423f3eee242b03142849eb4f81a4a409e59c
```

如果有前导 0，它们可能会被排除：
<!-- If there are leading 0s, they may be excluded: -->

```rust
Dan: 0000357ea5c1a4e7bc11b2b17ff2dc2dcca69750bfef1e1ebcaccf8c8018175b 
Dan: 0x0000357ea5c1a4e7bc11b2b17ff2dc2dcca69750bfef1e1ebcaccf8c8018175b
Dan: 0x357ea5c1a4e7bc11b2b17ff2dc2dcca69750bfef1e1ebcaccf8c8018175b
```

**帐户标识符**

目前，Aptos 仅支持一个帐户使用一个统一的标识符。Aptos 上的帐户普遍表示为 32 字节的十六进制字符串。小于 32 字节的十六进制字符串也是有效的；在这些情况下，十六进制字符串可以用前导零填充，例如 0x1 => 0x0000000000000...01。

**创建 Aptos 帐户**

当用户请求创建帐户时，例如通过使用 Aptos SDK，将执行以下加密步骤：

* 首先生成一个新的私钥、公钥对。
* 从用户那里，获取账户的首选签名方案：账户是否应该使用单个签名，或者是否需要多个签名来签署交易。
* 将公钥与用户的签名方案结合生成一个 32 字节的认证密钥。
* 将账户序号初始化为0。认证密钥和序号都作为初始账户资源存储在账户中。
* 从初始身份验证密钥创建 32 字节帐户地址。

从现在开始，用户应该使用私钥来签署与该帐户的交易。
<!-- **Account identifiers**

Currently, Aptos supports only a single, unified identifier for an account. Accounts on Aptos are universally represented as a 32-byte hex string. A hex string shorter than 32-bytes is also valid; in those scenarios, the hex string can be padded with leading zeroes, e.g., 0x1 => 0x0000000000000...01.

**Creating Aptos account**

When a user requests to create an account, for example by using the Aptos SDK, the following cryptographic steps are executed:

* Start by generating a new private key, public key pair.
* From the user, get the preferred signature scheme for the account: If the account should use a single signature or if it should require multiple signatures for signing a transaction.
* Combine the public key with the user's signature scheme to generate a 32-byte authentication key.
* Initialize the account sequence number to 0. Both the authentication key and the sequence number are stored in the account as an initial account resource.
* Create the 32-byte account address from the initial authentication key.

From now on, the user should use the private key for signing the transactions with this account. -->

**使用签名者进行访问控制**

交易的发送者由签名者代表。当 Move 模块中的函数将签名者作为参数时，Aptos Move VM 会将签署交易的帐户的身份转换为 Move 模块入口点中的签名者。请参阅下面的 Move 示例代码，其中包含初始化和撤销函数中的签名者。当函数中未指定签名者时，例如下面的存款函数，则此函数不存在访问控制：

<!-- **Access control with signer**

The sender of a transaction is represented by a signer. When a function in a Move module takes `signer` as an argument, then the Aptos Move VM translates the identity of the account that signed the transaction into a signer in a Move module entry point. See the below Move example code with `signer` in the `initialize` and `withdraw` functions. When a `signer` is not specified in a function, for example, the below `deposit` function, then no access controls exist for this function: -->

```rust
module Test::Coin {
  struct Coin has key { amount: u64 }

  public fun initialize(account: &signer) {
    move_to(account, Coin { amount: 1000 });
  }

  public fun withdraw(account: &signer, amount: u64): Coin acquires Coin {
    let balance = &mut borrow_global_mut<Coin>(Signer::address_of(account)).amount;
    *balance = *balance - amount;
    Coin { amount }
  }

  public fun deposit(account: address, coin: Coin) acquires Coin {
      let balance = &mut borrow_global_mut<Coin>(account).amount;
      *balance = *balance + coin.amount;
      Coin { amount: _ } = coin;
  }
}
```

**资源帐户**

资源帐户是一种开发人员功能，用于管理独立于用户管理的帐户的资源，特别是发布模块和自动签署交易。

例如，开发人员可以使用资源帐户来管理用于模块发布的帐户，例如管理合同。合同本身不需要签名者初始化后。资源帐户为您提供了模块向其他模块提供签名者并代表模块签署交易的方法。

通常，资源帐户用于两个主要目的：

* 存储和隔离资源；一个模块创建一个资源帐户只是为了托管特定的资源。
* 将模块作为独立（资源）帐户发布，这是去中心化设计中的一个构建块，其中没有私钥可以控制资源帐户。所有权 (SignerCap) 可以保存在另一个模块中，例如治理。

**Resource accounts**

A resource account is a developer feature used to manage resources independent of an account managed by a user, specifically publishing modules and automatically signing for transactions.

For example, a developer may use a resource account to manage an account for module publishing, say managing a contract. The contract itself does not require a signer post initialization. A resource account gives you the means for the module to provide a signer to other modules and sign transactions on behalf of the module.

Typically, a resource account is used for two main purposes:

* Store and isolate resources; a module creates a resource account just to host specific resources.
* Publish module as a standalone (resource) account, a building block in a decentralized design where no private keys can control the resource account. The ownership (SignerCap) can be kept in another module, such as governance.
