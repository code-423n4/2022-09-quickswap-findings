# QA Report

## ✅ L-1: `require()` should be used instead of `assert()`

### 📝 Description
Prior to solidity version 0.8.0, hitting an assert consumes the remainder of the transaction's available gas rather than returning it, as `require()`/`revert()` do. `assert()` should be avoided even past solidity version 0.8.0 as its [documentation](https://docs.soliditylang.org/en/v0.8.14/control-structures.html#panic-via-assert-and-error-via-require) states that The assert function creates an error of type Panic(uint256). ... Properly functioning code should never create a Panic, not even on invalid external input. If this happens, then there is a bug in your contract which you should fix

### 💡 Recommendation
You should change from `assert()` to `require()`

### 🔍 Findings:
```2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L190``` [assert(false);](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L190 )


## ✅ N-1: No use of two-phase ownership transfers

### 📝 Description
Consider adding a two-phase transfer, where the current owner nominates the next owner, and the next owner has to call `accept*()` to become the new owner. This prevents passing the ownership to an account that is unable to use it.

### 💡 Recommendation
Consider implementing a two step process where the admin nominates an account and the nominated account needs to call an acceptOwnership() function for the transfer of admin to fully succeed. This ensures the nominated EOA account is a valid and active account.

### 🔍 Findings:
```2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L51``` [owner = msg.sender;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L51 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L81``` [owner = _owner;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L81 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L32``` [owner = msg.sender;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L32 )


## ✅ N-2: Use a more recent version of solidity

### 📝 Description
Use a solidity version of at least 0.8.4 to get bytes.concat() instead of abi.encodePacked (<bytes>, <bytes>)
Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked (<str>, <str>)
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions

### 💡 Recommendation
Use more recent version of solidity.

### 🔍 Findings:
```2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/base/PoolImmutables.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolImmutables.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/base/PoolState.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolState.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/Constants.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/Constants.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/TokenDeltaMath.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TokenDeltaMath.sol#L2 )


## ✅ N-3: Use `string.concat()` or`bytes.concat()`

### 📝 Description
Solidity version 0.8.4 introduces `bytes.concat()` (vs `abi.encodePacked(<bytes>,<bytes>)`)Solidity version 0.8.12 introduces `string.concat()`(vs `abi.encodePacked(<str>,<str>)`)

### 💡 Recommendation
Use concat instead of abi.encodePacked

### 🔍 Findings:
```2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L124``` [pool = address(uint256(keccak256(abi.encodePacked(hex'ff', poolDeployer, keccak256(abi.encode(token0, token1)), POOL_INIT_CODE_HASH))));](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L124 )


