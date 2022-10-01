### Non-Critical Issues List
| Number |Issues Details|Context|
|:--:|:-------|:--:|
| [N-01] | Update to `owner` emit | All contracts |
| [N-02] | Use ```require``` instead of ```assert``` | 1 |
| [N-03] | Include return parameters in NatSpec comments | All contracts |
| [N-04] | ```0``` address check | 3 |
| [N-05] | Long lines are not suitable for the `Solidity Style Guide` | 10 |
| [N-06] | For modern and more readable code; Update import usages | All contracts |
| [N-07] | Update to `owner` emit | 1 |
| [N-08] | `0 address` require in `setFactory` function causes factory address to be updated only 1 time | 1 |
| [N-09] | The use of new{salt} came in Solidity 8.0.0, but the contract uses 0.7.6 | 1 |
| [N-10] | Use `private` instead of using `internal` | 1 |
| [N-11] | Redundant `0 Address` check | 1 | 

Total 11 issues


### Low Risk Issues List
| Number |Issues Details|
|:--:|:-------|
| [L-01] | Add to blacklist function |

Total 1 issue

### [N-01] Use a more recent version of solidity

**Context:**
All Contracts

**Description:**
Old version of Solidity is used (`0.7.6`), newer version should be used.

### [N-02] Use ```require``` instead of ```assert```

**Context:**
[DataStorage.sol#L190](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L190)

**Description:**
Assert should not be used except for tests, require should be used.

Prior to Solidity 0.8.0, pressing a confirm consumes the remainder of the process's available gas instead of returning it, as request()/revert() did.

Assertion() should be avoided even after solidity version 0.8.0, because its documentation states "The Assert function generates an error of type Panic(uint256). … Code that works properly should never Panic, even on invalid external input. If this happens, you need to fix it in your contract. there's a mistake".


### [N-03] Include return parameters in NatSpec comments

**Context:**
All Contracts

**Description:**
It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation. In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.

https://docs.soliditylang.org/en/v0.8.15/natspec-format.html

**Recommendation:**
Include return parameters in NatSpec comments

_Recommendation  Code Style: (from Uniswap3)_
```js
    /// @notice Adds liquidity for the given recipient/tickLower/tickUpper position
    /// @dev The caller of this method receives a callback in the form of IUniswapV3MintCallback#uniswapV3MintCallback
    /// in which they must pay any token0 or token1 owed for the liquidity. The amount of token0/token1 due depends
    /// on tickLower, tickUpper, the amount of liquidity, and the current price.
    /// @param recipient The address for which the liquidity will be created
    /// @param tickLower The lower tick of the position in which to add liquidity
    /// @param tickUpper The upper tick of the position in which to add liquidity
    /// @param amount The amount of liquidity to mint
    /// @param data Any data that should be passed through to the callback
    /// @return amount0 The amount of token0 that was paid to mint the given amount of liquidity. Matches the value in the callback
    /// @return amount1 The amount of token1 that was paid to mint the given amount of liquidity. Matches the value in the callback
    function mint(
        address recipient,
        int24 tickLower,
        int24 tickUpper,
        uint128 amount,
        bytes calldata data
    ) external returns (uint256 amount0, uint256 amount1);
```

### [N-04] ```0``` address check

**Context:**
[AlgebraFactory.sol#L54](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L54)
[AlgebraFactory.sol#L55](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L55)
[PoolImmutables.sol#L30](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolImmutables.sol#L30)

**Description:**
Also check of the address to protect the code from 0x0 address  problem just in case. This is best practice or instead of suggesting that they verify address != 0x0, you could add some good natspec comments explaining what is valid and what is invalid and what are the implications of accidentally using an invalid address.

**Recommendation:**
like this ;
```if (_treasury == address(0)) revert ADDRESS_ZERO();```

### [N-05] Long lines are not suitable for the `Solidity Style Guide`

**Context:**
[AlgebraPool.sol#L221](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L221)
[AlgebraPool.sol#L297](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L297)
[AlgebraPool.sol#L355](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L355)
[AlgebraPool.sol#L392](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L392)
[AlgebraPool.sol#L472](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L472)
[AlgebraPool.sol#L601](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L601)
[AlgebraPool.sol#L654](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L654)
[AlgebraPool.sol#L876](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L876)
[DataStorage.sol#L255](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L255)
[DataStorage.sol#L133](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L133)

**Description:**
It is generally recommended that lines in the source code should not exceed 80-120 characters. Today's screens are much larger, so in some cases it makes sense to expand that. The lines above should be split when they reach that length, as the files will most likely be on GitHub and GitHub always uses a scrollbar when the length is more than 164 characters.

[why-is-80-characters-the-standard-limit-for-code-width](https://softwareengineering.stackexchange.com/questions/148677/why-is-80-characters-the-standard-limit-for-code-width)

**Recommendation:**
Multiline output parameters and return statements should follow the same style recommended for wrapping long lines found in the Maximum Line Length section.

https://docs.soliditylang.org/en/v0.8.17/style-guide.html#introduction

```js
thisFunctionCallIsReallyLong(
    longArgument1,
    longArgument2,
    longArgument3
);
```

### [N-06] For modern and more readable code; Update import usages

**Context:**
All Contracts

**Description:**
Our Solidity code is also cleaner in another way that might not be noticeable: the struct Point. We were importing it previously with global import but not using it. The Point struct ```polluted the source code``` with an unnecessary object we were not using because we did not need it. This was breaking the rule of modularity and modular programming: ```only import what you need```Specific imports with curly braces allow us to apply this rule better.

**Recommendation:**
```import {contract1 , contract2} from "filename.sol";```


### [N-07]  Update to `owner` emit

**Context:**
[AlgebraFactory.sol#L52](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L52)

**Recommendation:**
In this way, it is more readable and comprehensible for web pages and other users that read emit.

Current Code : 
```js
 constructor(address _poolDeployer, address _vaultAddress) {
    owner = msg.sender;
    emit Owner(msg.sender);

    poolDeployer = _poolDeployer;
    vaultAddress = _vaultAddress;
  }
```

Recommendation Code : 
```js
 constructor(address _poolDeployer, address _vaultAddress) {
    owner = msg.sender;
    emit OwnerUpdated(address(0), owner);

    poolDeployer = _poolDeployer;
    vaultAddress = _vaultAddress;
  }
```

### [N-08]  `0 address` require in `setFactory` function causes factory address to be updated only 1 time

**Context:**
[AlgebraPoolDeployer.sol#L38](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L38)

**Recommendation:**
`setFactory` require(factory == address(0)); query allows the function to be executed only once and restricts the set property, remove this from the code.
If it needs to be run once, add initalize or assign value from constrcutor.

```js
 function setFactory(address _factory) external override onlyOwner {
    require(_factory != address(0));
    require(factory == address(0));
    emit Factory(_factory);
    factory = _factory;
  }
```

### [N-09]  The use of new{salt} came in Solidity 8.0.0, but the contract uses 0.7.6

**Context:**
[AlgebraPoolDeployer.sol#L51](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L51)

**Description:**
Contracts can be created by other contracts using the new keyword. Since 0.8.0, new keyword supports create2 feature by specifying salt options.
Using the old pragma version of the contract may cause unexpected errors

```js
pool = address(new AlgebraPool{salt: keccak256(abi.encode(token0, token1))}());
```
**Recommendation:**
Use new solidity version.

### [N-10] Use `private` instead of using `internal`

**Context:**
[AlgebraFactory.sol#L116](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L116)

**Recommendation:**
POOL_INIT_CODE_HASH variable use only own contract,so Use ``private`` instead of using ``internal``.
this is best practice for security

Current Code:
```js
bytes32 internal constant POOL_INIT_CODE_HASH = 0x6ec6c9c8091d160c0aa74b2b14ba9c1717e95093bd3ac085cee99a49aab294a4;
```

Recommendation Code:
```js
bytes32 private constant POOL_INIT_CODE_HASH = 0x6ec6c9c8091d160c0aa74b2b14ba9c1717e95093bd3ac085cee99a49aab294a4;
```

### [N-11] Redundant `0 Address` check

**Context:**
[AlgebraFactory.sol#L62](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L62)

**Recommendation:**
This check looks redundant. It is anyway possible to pass an address that doesn’t actually refers to a token smart contract.

Current Code:
```js
require(token0 != address(0));
```
This code is executed even if the owner is not changed.


### [L-01] Add to blacklist function

**Description:**
Cryptocurrency mixing service, Tornado Cash, has been blacklisted in the OFAC.
A lot of blockchain companies, token projects, NFT Projects have ```blacklisted``` all Ethereum addresses owned by Tornado Cash listed in the US Treasury Department's sanction against the protocol.
https://home.treasury.gov/policy-issues/financial-sanctions/recent-actions/20220808
In addition, these platforms even ban accounts that have received ETH on their account with Tornadocash.

Some of these Projects;
* USDC (https://www.circle.com/en/usdc)
* Flashbots (https://www.paradigm.xyz/portfolio/flashbots )
* Aave (https://aave.com/)
* Uniswap
* Balancer
* Infura
* Alchemy 
* Opensea
* dYdX 

Details on the subject;
https://twitter.com/bantg/status/1556712790894706688?s=20&t=HUTDTeLikUr6Dv9JdMF7AA

https://cryptopotato.com/defi-protocol-aave-bans-justin-sun-after-he-randomly-received-0-1-eth-from-tornado-cash/

For this reason, every project in the Ethereum network must have a blacklist function, this is a good method to avoid legal problems in the future, apart from the current need.

Transactions from the project by an account funded by Tonadocash or banned by OFAC can lead to legal problems.Especially American citizens may want to get addresses to the blacklist legally, this is not an obligation

If you think that such legal prohibitions should be made directly by validators, this may not be possible:
https://www.paradigm.xyz/2022/09/base-layer-neutrality

```The ban on Tornado Cash makes little sense, because in the end, no one can prevent people from using other mixer smart contracts, or forking the existing ones. It neither hinders cybercrime, nor privacy.```

Here is the most beautiful and close to the project example; Manifold

Manifold Contract
https://etherscan.io/address/0xe4e4003afe3765aca8149a82fc064c0b125b9e5a#code

```js
     modifier nonBlacklistRequired(address extension) {
         require(!_blacklistedExtensions.contains(extension), "Extension blacklisted");
         _;
     }
```
Recommended Mitigation Steps add to Blacklist function and modifier.