## `15000 - 3000` can be changed to `12000` and save some gas

Instead of performing mathematical operations, the result can be written directly.
___
## In `require()`, Use `!= 0` Instead of `> 0` With Uint Values  
  
In a require, when checking a uint, using `!= 0` instead of `> 0` saves 6 gas. This will jump over or avoid an extra `ISZERO` opcode.  
  
Instances include:  
[`src/core/contracts/AlgebraPool.sol:224`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L224)
[`src/core/contracts/AlgebraPool.sol:434`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L434)
[`src/core/contracts/AlgebraPool.sol:454`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L454)
[`src/core/contracts/AlgebraPool.sol:455`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L455)
[`src/core/contracts/AlgebraPool.sol:469`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L469)
[`src/core/contracts/AlgebraPool.sol:898`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L898)
[`src/core/contracts/libraries/PriceMovementMath.sol:52`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L52)
[`src/core/contracts/libraries/PriceMovementMath.sol:53`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L53)
___
## `++i` COSTS LESS GAS COMPARED TO `i++` OR `i += 1`  
  
`++i` costs less gas compared to `i++` or `i += 1` for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration). This statement is true even with the optimizer enabled.  
  
`i++` increments `i` and returns the initial value of `i`. Which means:  
  
`uint i = 1;  
i++; // == 1 but i == 2  `  
But `++i` returns the actual incremented value:  
  
`uint i = 1;  
++i; // == 2 and i == 2 too, so no need for a temporary variable `  
In the first case, the compiler has to create a temporary variable (when used) for returning `1` instead of `2`  
  
Instance includes:  
  
[`src/core/contracts/libraries/DataStorage.sol:307`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307)
I suggest using ++i instead of i++ to increment the value of a uint variable.
___
## Cache array length in for loops can save gas  
Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack.  
Caching the array length in the stack saves around 3 gas per iteration.  
  
Instances include:  
  
[`src/core/contracts/libraries/DataStorage.sol:307`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307)
___
## Not initializing `unit` variable to default value of zero  
  
Instance includes:  
[`src/core/contracts/libraries/DataStorage.sol:307`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307)
___
## USE A MORE RECENT VERSION OF SOLIDITY

Use a solidity version of at least 0.8.0 to get overflow protection without `LowGasSafeMath` Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than `revert()/require()` strings Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value.

Instances include:
[`src/core/contracts/AlgebraFactory.sol:2`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L2) 
[`src/core/contracts/AlgebraPool.sol:2`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L2)
[`src/core/contracts/AlgebraPoolDeployer.sol:2`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L2)
[`src/core/contracts/DataStorageOperator.sol:2`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L2)
[`src/core/contracts/libraries/AdaptiveFee.sol:2`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L2)
[`src/core/contracts/libraries/Constants.sol:2`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/Constants.sol#L2)
[`src/core/contracts/libraries/DataStorage.sol:2`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L2)
[`src/core/contracts/libraries/PriceMovementMath.sol:2`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L2)
[`src/core/contracts/libraries/TickManager.sol:2`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L2)
[`src/core/contracts/libraries/TickTable.sol:2`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L2)
[src/core/contracts/libraries/TokenDeltaMath.sol`:2`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TokenDeltaMath.sol#L2)
[`src/core/contracts/base/PoolImmutables.sol:2`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolImmutables.sol#L2)
[`src/core/contracts/base/PoolState.sol:2`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolState.sol#L2)