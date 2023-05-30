---
title: Aptos Coin
weight: 48
---

# Aptos Coin

[Coin](https://github.com/aptos-labs/aptos-core/blob/main/aptos-move/framework/aptos-framework/sources/coin.move) 为简单的、可替代的代币或硬币提供标准的、类型安全的框架。
<!-- [Coin](https://github.com/aptos-labs/aptos-core/blob/main/aptos-move/framework/aptos-framework/sources/coin.move) provides a standard, typesafe framework for simple, fungible tokens or coins. -->

## Structures <a href="#structures" id="structures"></a>

* ### _Reusability_ <a href="#reusability" id="reusability"></a>

<!-- A coin is defined in Move as: -->
coin 在 Move 中被定义为：

```rust
struct Coin<phantom CoinType> has store {
    /// Amount of coin this address has.
    value: u64,
}
```

Coin 使用 `CoinType` 来支持不同 Coins 的 Coin 框架的可重用性。例如，`Coin<A>` 和 `Coin<B>` 是两个不同的钱币。

<!-- A Coin uses the `CoinType` to support re-usability of the Coin framework for distinct Coins. For example, `Coin<A>` and `Coin<B>` are two distinct coins. -->

* ### _Global Store_ <a href="#reusability" id="reusability"></a>

<!-- Coin also supports a resource for storing coins in global store: -->
Coin 还支持在全局商店中存储硬币的资源：

```rust
struct CoinStore<phantom CoinType> has key {
    coin: Coin<CoinType>,
    frozen: bool,
    deposit_events: EventHandle<DepositEvent>,
    withdraw_events: EventHandle<WithdrawEvent>,
}
```

钱币信息或元数据存储在硬币创建者帐户下的全球商店中：
<!-- Coin information or metadata is stored in global store under the coin creators account: -->

```rust
struct CoinInfo<phantom CoinType> has key {
    name: string::String,
    /// Symbol of the coin, usually a shorter version of the name.
    /// For example, Singapore Dollar is SGD.
    symbol: string::String,
    /// Number of decimals used to get its user representation.
    /// For example, if `decimals` equals `2`, a balance of `505` coins should
    /// be displayed to a user as `5.05` (`505 / 10 ** 2`).
    decimals: u8,
    /// Amount of this coin type in existence.
    supply: Option<OptionalAggregator>,
}
```

## Creating a new CoinType <a href="#creating-a-new-cointype" id="creating-a-new-cointype"></a>

硬币创建者可以向链上帐户发布一个新模块，该模块定义一个结构来表示新的 `CoinType`。然后硬币创建者将从该帐户调用 `coin:initialize<CoinType>` 以将其注册为有效硬币，并作为回报接收回结构，这些结构允许调用函数来销毁和铸造硬币并冻结 `CoinStores`。这些将需要由创建者存储在全局存储中以管理代币的使用。

<!-- A coin creator can publish to an on-chain account a new module that defines a struct to represent a new `CoinType`. The coin creator will then call `coin:initialize<CoinType>` from that account to register this as a valid coin, and in return receive back structs that enable calling the functions to burn and mint coins and freeze `CoinStore`s. These will need to be stored in global storage by the creator to manage the use of the coin. -->

```rust
public fun initialize<CoinType>(
    account: &signer,
    name: string::String,
    symbol: string::String,
    decimals: u8,
    monitor_supply: bool,
): (BurnCapability<CoinType>, FreezeCapability<CoinType>, MintCapability<CoinType>) {
```

* 上面的前三个（名称、符号、小数）是纯元数据，对链上应用程序没有影响。一些应用程序可能使用小数来将单个硬币与分数硬币等同起来。
* 监控供应 (`monitor_supply`) 有助于跟踪供应总量。但是，由于并行执行器的工作方式，打开此选项将阻止 mint 和 burn 的任何并行执行。如果硬币将定期铸造或销毁，请考虑禁用 `monitor_supply`。

<!-- * The first three of the above (`name`, `symbol`, `decimals`) are purely metadata and have no impact for on-chain applications. Some applications may use decimal to equate a single Coin from fractional coin. -->
<!-- * Monitoring supply (`monitor_supply`) helps track total coins in supply. However, due to the way the parallel executor works, turning on this option will prevent any parallel execution of mint and burn. If the coin will be regularly minted or burned, consider disabling `monitor_supply`. -->

## Minting Coins <a href="#minting-coins" id="minting-coins"></a>

如果创建者或管理者想要铸造硬币，他们必须检索对在初始化中生成的 `MintCapability` 的引用，并调用：

<!-- If the creator or manager would like to mint coins, they must retrieve a reference to their `MintCapability`, which was produced in the `initialize`, and call: -->

```rust
public fun mint<CoinType>(
    amount: u64,
    _cap: &MintCapability<CoinType>,
): Coin<CoinType> acquires CoinInfo {
```

<!-- This will produce a new Coin struct containing a value as dictated by the `amount`. If supply is tracked, then it will also be adjusted. -->
这将产生一个新的 Coin 结构，其中包含一个由金额决定的值。如果跟踪供应，那么它也会进行调整。

## Burning Coins[​](https://aptos.dev/standards/aptos-coin#burning-coins) <a href="#burning-coins" id="burning-coins"></a>

如果创建者或管理者想要销毁代币，他们必须检索在初始化时生成的对其 BurnCapability 的引用，并调用：

If the creator or manager would like to burn coins, they must retrieve a reference to their `BurnCapability`, which was produced in the `initialize`, and call:

```rust
public fun burn<CoinType>(
    coin: Coin<CoinType>,
    _cap: &BurnCapability<CoinType>,
) acquires CoinInfo {
```

创建者或管理者也可以从 `CoinStore` 销毁硬币：

<!-- The creator or manager can also burn coins from a `CoinStore`: -->

```rust
public fun burn_from<CoinType>(
    account_addr: address,
    amount: u64,
    burn_cap: &BurnCapability<CoinType>,
) acquires CoinInfo, CoinStore {
```

## Freezing Accounts[​](https://aptos.dev/standards/aptos-coin#freezing-accounts) <a href="#freezing-accounts" id="freezing-accounts"></a>

<!-- If the creator or manager would like to freeze a `CoinStore` on a specific account, they must retrieve a reference to their `FreezeCapability`, which was produced in `initialize`, and call: -->
<!-- 冻结账户  -->
如果创建者或管理者想冻结特定账户上的 `CoinStore`，他们必须检索对他们在初始化时产生的 `FreezeCapability` 的引用，并调用：

```rust
public entry fun freeze_coin_store<CoinType>(
    account_addr: address,
    _freeze_cap: &FreezeCapability<CoinType>,
) acquires CoinStore {
```

## Merging Coins[​](https://aptos.dev/standards/aptos-coin#merging-coins) <a href="#merging-coins" id="merging-coins"></a>

<!-- Two coins of the same type can be merged into a single Coin struct that represents the accumulated value of the two coins independently by calling: -->

两个相同类型的硬币可以合并成一个单独的 Coin 结构，通过调用：

```rust
public fun merge<CoinType>(
    dst_coin: &mut Coin<CoinType>,
    source_coin: Coin<CoinType>,
) {
```

## Extracting Coins[​](https://aptos.dev/standards/aptos-coin#extracting-coins) <a href="#extracting-coins" id="extracting-coins"></a>

<!-- A Coin can have value deducted to create another Coin by calling: -->
可以通过调用以下方式扣除一个硬币的价值以创建另一个硬币：

```rust
public fun extract<CoinType>(
        coin: &mut Coin<CoinType>,
        amount: u64,
): Coin<CoinType> {
```

## Withdrawing Coins from CoinStore[​](https://aptos.dev/standards/aptos-coin#withdrawing-coins-from-coinstore) <a href="#withdrawing-coins-from-coinstore" id="withdrawing-coins-from-coinstore"></a>

`CoinStore` 的持有者可以通过调用以下方式提取指定价值的 Coin：
<!-- A holder of a `CoinStore` can extract a Coin of a specified value by calling: -->

```rust
public fun withdraw<CoinType>(
    account: &signer,
    amount: u64,
): Coin<CoinType> acquires CoinStore {
```

## Depositing Coins into CoinStore[​](https://aptos.dev/standards/aptos-coin#depositing-coins-into-coinstore) <a href="#depositing-coins-into-coinstore" id="depositing-coins-into-coinstore"></a>

<!-- Any entity can deposit coins into an account’s `CoinStore` by calling: -->
任何实体都可以通过调用以下方式将硬币存入帐户的 `CoinStore`：

```rust
public fun deposit<CoinType>(
        account_addr: address,
        coin: Coin<CoinType>,
) acquires CoinStore {
```

## Transferring Coins[​](https://aptos.dev/standards/aptos-coin#transferring-coins) <a href="#transferring-coins" id="transferring-coins"></a>

`CoinStore` 的持有人可以通过调用以下方式直接将硬币从他们的账户转移到另一个账户的 `CoinStore`：
<!-- A holder of a `CoinStore` can directly transfer coins from their account to another account’s `CoinStore` by calling: -->

```rust
public entry fun transfer<CoinType>(
    from: &signer,
    to: address,
    amount: u64,
) acquires CoinStore {
```

## Events[​](https://aptos.dev/standards/aptos-coin#events) <a href="#events" id="events"></a>

```rust
struct DepositEvent has drop, store {
    amount: u64,
}
```

```rust
struct WithdrawEvent has drop, store {
    amount: u64,
}
```
