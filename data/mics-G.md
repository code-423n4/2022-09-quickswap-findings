Table Of Content
================

* [GAS REPORT](#gas-report)
        * [Not Efficient Struct Packing](#not-efficient-struct-packing)
        * [Use assembly opcodes iszero instead of solidity equation to save gas](#use-assembly-opcodes-iszero-instead-of-solidity-equation-to-save-gas)
        * [Caching array size](#caching-array-size)

# GAS REPORT

## Not Efficient Struct Packing
By reordering the struct variables you can decrease the number of slots in use and therefore reduce the gas cost of using the struct.

For instance, [AlgebraPool.sol#L675](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L675)

## Use assembly opcodes iszero instead of solidity equation to save gas


### Code Instances:
- [TickOverflowSafetyEchidnaTest.sol#L75](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TickOverflowSafetyEchidnaTest.sol#L75)
- [BitMathEchidnaTest.sol#L17](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/BitMathEchidnaTest.sol#L17)
- [AlgebraPool.sol#L193](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L193)
- [LowGasSafeMathEchidnaTest.sol#L21](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/LowGasSafeMathEchidnaTest.sol#L21)

## Caching array size
In the following for loops consider caching the array size instead of loading it every iteration.

For instance, [DataStorageTest.sol#L55](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/DataStorageTest.sol#L55)
