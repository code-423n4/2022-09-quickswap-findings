## 1. Consider replacing `require()` statements with custom errors
Introduced in v0.8.4, Custom errors are a more gas-efficient way to throw an error.
Note: Must be v0.8.4 or higher

## 2. `++i` is more efficient than `i++`
`++i` saves 5 gas compared to `i++`, This is because `i++`  increments a value and returns the old value, Doing so holds two numbers in memory. `++i` only ever uses one number in memory.


Lines Found:
 `DataStorage.sol`: [Line 307](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307)
 `FullMath.sol`: [Line 122](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/FullMath.sol#L122)

## 3. Initializing variable to false or zero costs more gas
Variables if not initialized are by default set to zero, false, etc.

Lines Affected:
`AlgebraPool.sol`: [Line 774](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L774)
`DataStorage.sol`: [Line 307](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307)
`TickMath.sol` [Line 71](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickMath.sol#L71)

## 4. Using  `!= 0` is more gas efficient compared to `> 0`
`!= 0 ` costs about 6 less gas compared to `> 0`

Here is an example: [Lines 505-506](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L505-L506) in `AlgebraPool.sol` can both replace `> 0` with `!= 0`
 