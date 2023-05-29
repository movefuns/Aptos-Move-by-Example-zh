---
description: Non-Fungible Token
---

# Aptos代币(Nft)

> 非同质化代币

## NFT概述 

一个是不可替代的或存储在区块链上的数据，它唯一地定义了资产的所有权。NFT 最初是在 中定义的，后来在 NFT 通常使用以下属性定义：

* 名称：资产的名称。  它在集合中必须是唯一的。
* 描述：资产的描述。
* uri：指向链下的 URL 指针，用于获取有关资产的更多信息。资产可以是图像或视频等媒体或更多元数据。
* supply：该 NFT 的总数量。许多 NFT 只有一个供应，而那些有多个供应的 NFT 被称为版本。
 
此外，大多数 NFT 是一个集合的一部分或一组具有共同属性的 NFT，例如，主题、创建者或最小合同。  每个集合都有一组相似的属性：

* 名称：集合的名称。该名称在创建者的帐户中必须是唯一的。
* 描述：集合的描述。
* uri：指向链下的 URL 指针，用于获取有关资产的更多信息。资产可以是图像或视频等媒体或更多元数据。
* supply：这个集合中 NFT 的总数。
* maximum：这个集合可以拥有的最大 NFT 数量。如果最大值设置为 0，则不跟踪供应。

<!-- # Aptos Token(Nft)

## Overview of NFT[​](https://aptos.dev/standards/aptos-token#overview-of-nft) <a href="#overview-of-nft" id="overview-of-nft"></a>

An [NFT](https://en.wikipedia.org/wiki/Non-fungible\_token) is a non-fungible [token](https://github.com/aptos-labs/aptos-core/blob/main/aptos-move/framework/aptos-token/sources/token.move) or data stored on a blockchain that uniquely defines ownership of an asset. NFTs were first defined in [EIP-721](https://eips.ethereum.org/EIPS/eip-721) and later expanded upon in [EIP-1155](https://eips.ethereum.org/EIPS/eip-1155). NFTs are typically defined using the following properties:

* `name`: The name of the asset. It must be unique within a collection.
* `description`: The description of the asset.
* `uri`: A URL pointer to off-chain for more information about the asset. The asset could be media such as an image or video or more metadata.
* `supply`: The total number of units of this NFT. Many NFTs have only a single supply, while those that have more than one are referred to as editions.

Additionally, most NFTs are part of a collection or a set of NFTs with a common attribute, for example, a theme, creator, or minimally contract. Each collection has a similar set of attributes:

* `name`: The name of the collection. The name must be unique within the creator's account.
* `description`: The description of the collection.
* `uri`: A URL pointer to off-chain for more information about the asset. The asset could be media such as an image or video or more metadata.
* `supply`: The total number of NFTs in this collection.
* `maximum`: The maximum number of NFTs that this collection can have. If `maximum` is set to 0, then supply is untracked. -->

## Token resources[​](https://aptos.dev/standards/aptos-token#token-resources) <a href="#token-resources" id="token-resources"></a>

如上图所示，与代币相关的数据同时存储在创建者账户和所有者账户中。
<!-- As shown in the diagram above, token-related data are stored at both the creator’s account and the owner’s account. -->

#### _Struct-level resources_[​](https://aptos.dev/standards/aptos-token#struct-level-resources) <a href="#struct-level-resources" id="struct-level-resources"></a>

下表描述了结构级别的字段。有关最终列表，请参阅 [Aptos Token Framework](https://github.com/aptos-labs/aptos-core/blob/main/aptos-move/framework/aptos-token/doc/overview.md)。

<!-- The following tables describe fields at the struct level. For the definitive list, see the [Aptos Token Framework](https://github.com/aptos-labs/aptos-core/blob/main/aptos-move/framework/aptos-token/doc/overview.md). -->

_**Resource stored at the creator’s address**_[**​**](https://aptos.dev/standards/aptos-token#resource-stored-at-the-creators-address)

| Field                                                                                                                                                                   | Description                                                                                                                                                                                                                                                |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`Collections`](https://github.com/aptos-labs/aptos-core/blob/main/aptos-move/framework/aptos-token/doc/token.md#resource-collections)                                  | Maintains a table called `collection_data`, which maps the collection name to the `CollectionData`. It also stores all the `TokenData` that this creator creates.                                                                                          |
| [`CollectionData`](https://github.com/aptos-labs/aptos-core/blob/main/aptos-move/framework/aptos-token/doc/token.md#struct-collectiondata)                              | Stores the collection metadata. The supply is the number of tokens created for the current collection. The maxium is the upper bound of tokens in this collection.                                                                                         |
| [`CollectionMutabilityConfig`](https://github.com/aptos-labs/aptos-core/blob/main/aptos-move/framework/aptos-token/doc/token.md#0x3\_token\_CollectionMutabilityConfig) | Specifies which field is mutable.                                                                                                                                                                                                                          |
| [`TokenData`](https://github.com/aptos-labs/aptos-core/blob/main/aptos-move/framework/aptos-token/doc/token.md#0x3\_token\_TokenData)                                   | Acts as the main struct for holding the token metadata. Properties is a where users can add their own properties that are not defined in the token data. Users can mint more tokens based on the `TokenData`, and those tokens share the same `TokenData`. |
| [`TokenMutabilityConfig`](https://github.com/aptos-labs/aptos-core/blob/main/aptos-move/framework/aptos-token/doc/token.md#0x3\_token\_TokenMutabilityConfig)           | Controls which fields are mutable.                                                                                                                                                                                                                         |
| [`TokenDataId`](https://github.com/aptos-labs/aptos-core/blob/main/aptos-move/framework/aptos-token/doc/token.md#0x3\_token\_TokenDataId)                               | An ID used for representing and querying `TokenData` on-chain. This ID mainly contains three fields including creator address, collection name and token name.                                                                                             |
| [`Royalty`](https://github.com/aptos-labs/aptos-core/blob/main/aptos-move/framework/aptos-token/doc/token.md#0x3\_token\_Royalty)                                       | Specifies the denominator and numerator for calculating the royalty fee. It also has the payee account address for depositing the royalty.                                                                                                                 |
| `PropertyValue`                                                                                                                                                         | Contains both value of a property and type of property.                                                                                                                                                                                                    |

_**Resource stored at the owner’s address**_[**​**](https://aptos.dev/standards/aptos-token#resource-stored-at-the-owners-address)

| Field                                                                                                                                   | Description                                                                                                                                                            |
| --------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`TokenStore`](https://github.com/aptos-labs/aptos-core/blob/main/aptos-move/framework/aptos-token/doc/token.md#0x3\_token\_TokenStore) | The main struct for storing the token owned by this address. It maps `TokenId` to the actual token.                                                                    |
| [`Token`](https://github.com/aptos-labs/aptos-core/blob/main/aptos-move/framework/aptos-token/doc/token.md#0x3\_token\_Token)           | The amount is the number of tokens.                                                                                                                                    |
| [`TokenId`](https://github.com/aptos-labs/aptos-core/blob/main/aptos-move/framework/aptos-token/doc/token.md#0x3\_token\_TokenId)       | `TokenDataId` points to the metadata of this token. The `property_version` represents a token with mutated `PropertyMap` from `default_properties` in the `TokenData`. |

有关更详细的说明，请参阅 [Aptos Token Framework](https://github.com/aptos-labs/aptos-core/blob/main/aptos-move/framework/aptos-token/doc/overview.md)。

<!-- For more detailed descriptions, see [Aptos Token Framework](https://github.com/aptos-labs/aptos-core/blob/main/aptos-move/framework/aptos-token/doc/overview.md). -->

## Token creation <a href="#token-creation" id="token-creation"></a>

每个 Aptos 令牌都属于一个集合。开发者首先需要通过 `create_collection_script` 创建一个集合，然后创建属于该集合的 token `create_token_script`。为了实现并行创建 `TokenData` 和 `Token`，开发人员可以创建无限集合和 `TokenData`，其中集合和 `TokenData` 的最大值设置为 0。使用此设置，令牌合约将不会跟踪令牌类型的供应（TokenData 计数）以及每种代币类型中的代币供应。因此，可以并行创建 `TokenData` 和 Token。

<!-- Every Aptos token belongs to a collection. The developer first needs to create a collection through `create_collection_script` and then create the token belonging to the collection `create_token_script`. To achieve parallel `TokenData` and `Token` creation, a developer can create unlimited collection and `TokenData` where the `maximum` of the collection and `TokenData` are set as 0. With this setting, the token contract won’t track the supply of types of token (`TokenData` count) and supply of token within each token type. As the result, the `TokenData` and token can be created in parallel. -->

```rust
/// create a empty token collection with parameters
public entry fun create_collection_script(
        creator: &signer,
        name: String,
        description: String,
        uri: String,
        maximum: u64,
        mutate_setting: vector<bool>,
    ) acquires Collections {
    
/// create token with raw inputs
 public entry fun create_token_script(
        account: &signer,
        collection: String,
        name: String,
        description: String,
        balance: u64,
        maximum: u64,
        uri: String,
        royalty_payee_address: address,
        royalty_points_denominator: u64,
        royalty_points_numerator: u64,
        mutate_setting: vector<bool>,
        property_keys: vector<String>,
        property_values: vector<vector<u8>>,
        property_types: vector<String>
    ) acquires Collections, TokenStore {    
```

Aptos 还强制对输入大小进行简单验证并防止重复：

* 代币名称 - 在每个集合中都是唯一的
* 集合名称 - 在每个帐户中都是唯一的
* 令牌和集合名称长度 - 小于 128 个字符
* URI 长度 - 小于 512 个字符
* 属性映射 - 最多可容纳 1000 个属性，每个键应小于 128 个字符

## 代币销毁

我们为令牌所有者和令牌创建者提供 `burn` 和 `burn_by_creator` 函数来销毁（或销毁）代币。然而，这两个功能也受到代币创建期间指定的配置的保护，因此创建者和所有者都清楚谁可以销毁令牌。仅当 `default_properties` 中的 `BURRNABLE_BY_OWNER` 属性设置为 `true` 时才允许刻录。当 `default_properties` 中的 `BURRNABLE_BY_CREATOR` 为真时，允许创建者刻录。一旦属于 `TokenData` 的所有代币都被销毁，`TokenData` 将从创建者的帐户中删除。类似地，如果属于一个集合的所有 `TokenData` 都被删除，则该 `CollectionData` 将从创建者的帐户中删除。

<!-- Aptos also enforces simple validation of the input size and prevents duplication:

* Token name - unique within each collection
* Collection name - unique within each account
* Token and collection name length - smaller than 128 characters
* URI length - smaller than 512 characters
* Property map - can hold at most 1000 properties, and each key should be smaller than 128 characters

## Token burn <a href="#token-burn" id="token-burn"></a>

We provide `burn` and `burn_by_creator` functions for token owners and token creators to burn (or destroy) tokens. However, these two functions are also guarded by configs that are specified during the token creation so that both creator and owner are clear on who can burn the token. Burn is allowed only when the `BURNABLE_BY_OWNER` property is set to `true` in `default_properties`. Burn by creator is allowed when `BURNABLE_BY_CREATOR` is `true` in `default_properties`. Once all the tokens belonging to a `TokenData` are burned, the `TokenData` will be removed from the creator’s account. Similarly, if all `TokenData` belonging to a collection are removed, the `CollectionData` will be removed from the creator’s account. -->

```rust
/// The token is owned at address owner
    public entry fun burn_by_creator(
        creator: &signer,
        owner: address,
        collection: String,
        name: String,
        property_version: u64,
        amount: u64,
    ) acquires Collections, TokenStore {
    
/// Burn a token by the token owner
    public entry fun burn(
        owner: &signer,
        creators_address: address,
        collection: String,
        name: String,
        property_version: u64,
        amount: u64
    ) acquires Collections, TokenStore {
```

## 代币转移

我们提供三种不同的模式来在发送方和接收方之间传输令牌。

**两步转移**
 
* 为了保护用户免受不想要的 NFT 的影响，必须首先向他们提供 NFT，然后再接受提供的 NFT。然后只有提供的 NFT 才会存入用户的代币商店。这是默认的令牌传输行为。例如：
* 如果 Alice 想给 Bob 发送一个 NFT，她必须先向 Bob 提供这个 NFT。  这个 NFT 仍然存储在 Alice 的账户下。
 只有当 Bob 认领 NFT 时，NFT 才会从 Alice 的账户中移除并存储在 Bob 的代币存储中。

<!-- ## Token transfer <a href="#token-transfer" id="token-transfer"></a>

We provide three different modes for transferring tokens between the sender and receiver.

_**Two-step transfer**_[**​**](https://aptos.dev/standards/aptos-token#two-step-transfer)

To protect users from receiving undesired NFTs, they must be first offered NFTs, and then accept the offered NFTs. Then only the offered NFTs will be deposited in the users' token stores. This is the default token transfer behavior. For example:

1. If Alice wants to send Bob an NFT, she must first offer Bob this NFT. This NFT is still stored under Alice’s account.
2. Only when Bob claims the NFT, will the NFT be removed from Alice’s account and stored in Bob’s token store. -->

#### _**Transfer with opt-in**_[**​**](https://aptos.dev/standards/aptos-token#transfer-with-opt-in)

If a user wants to receive direct transfer of the NFT, skipping the initial steps of offer and claim, then the user can call [`opt_in_direct_transfer`](https://github.com/aptos-labs/aptos-core/blob/main/aptos-move/framework/aptos-token/doc/token.md#0x3\_token\_opt\_in\_direct\_transfer) to allow other people to directly transfer the NFTs into the user's token store. After opting into direct transfer, the user can call [`transfer`](https://github.com/aptos-labs/aptos-core/blob/main/aptos-move/framework/aptos-token/doc/token.md#0x3\_token\_transfer) to transfer tokens directly. For example, Alice and anyone can directly send a token to Bob's token store once Bob opts in.

<!-- 选择加入转移  -->
如果用户想直接接收 NFT 的转账，跳过最初的报价和认领步骤，那么用户可以调用允许其他人直接将 NFT 转入用户的代币存储。选择直接转账后，用户可以直接调用转账。例如，一旦 Bob 选择加入，Alice 和任何人都可以直接将令牌发送到 Bob 的令牌存储。

<pre class="language-rust"><code class="lang-rust">public entry fun opt_in_direct_transfer(
         account: &#x26;signer,
         opt_in: bool
         ) acquires TokenStore {

/// Transfers `amount` of tokens from `from` to `to`.
/// The receiver `to` has to opt-in direct transfer first
<strong>public entry fun transfer_with_opt_in(
</strong>        from: &#x26;signer,
        creator: address,
        collection_name: String,
        token_name: String,
        token_property_version: u64,
        to: address,
        amount: u64,
    ) acquires TokenStore {
</code></pre>

_**Multi-agent transfer**_[**​**](https://aptos.dev/standards/aptos-token#multi-agent-transfer)

发送方和接收方都可以签署转账交易，直接将代币从发送方转移到接收方。例如，一旦 Alice 和 Bob 都签署了转账交易，则代币将直接从 Alice 的账户转给 Bob。

<!-- The sender and receiver can both sign a transfer transaction to directly transfer a token from the sender to receiver [`direct_transfer_script`](https://github.com/aptos-labs/aptos-core/blob/main/aptos-move/framework/aptos-token/doc/token.md#function-direct\_transfer\_script). For example, once Alice and Bob both sign the transfer transaction, the token will be directly transferred from Alice's account to Bob. -->

```rust
public entry fun direct_transfer_script(
        sender: &signer,
        receiver: &signer,
        creators_address: address,
        collection: String,
        name: String,
        property_version: u64,
        amount: u64,
    ) acquires TokenStore {
```
