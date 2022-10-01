# GAS REPORT

## [GAS] Caching msg.sender is unnecessary


### Proof of concept:
- [AlgebraPool.sol#L852](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L852)
- [TestAlgebraCallee.sol#L164](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraCallee.sol#L164)

## [GAS] Cache the following arrays size before the loop


Example: [DataStorageTest.sol#L55](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/DataStorageTest.sol#L55)

## [GAS] The following require statements could be split into multiple require statements to save gas


### Proof of concept:
- [TokenDeltaMathEchidnaTest.sol#L210](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TokenDeltaMathEchidnaTest.sol#L210)
- [AlgebraPool.sol#L967](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L967)

--