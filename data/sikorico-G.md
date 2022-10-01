# GAS REPORT

## [GAS 00] Use ```> 0``` instead ```!= 0``` to check if an unsigned int is not 0


### Proof of concept:
- [DataStorageEchidnaTest.sol#L115](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/DataStorageEchidnaTest.sol#L115)
- [TokenDeltaMathEchidnaTest.sol#L141](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TokenDeltaMathEchidnaTest.sol#L141)
- [FullMathEchidnaTest.sol#L50](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/FullMathEchidnaTest.sol#L50)
- [AlgebraPool.sol#L433](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L433)
- [PriceMovementMathEchidnaTest.sol#L14](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/PriceMovementMathEchidnaTest.sol#L14)

## [GAS 01] abiEncodePacked() instead abiEncode() in the following locations


### Proof of concept:
- [TestAlgebraCallee.sol#L155](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraCallee.sol#L155)
- [TestAlgebraCallee.sol#L86](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraCallee.sol#L86)
- [TestAlgebraRouter.sol#L54](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraRouter.sol#L54)
- [TestAlgebraCallee.sol#L61](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraCallee.sol#L61)
- [TestAlgebraCallee.sol#L70](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraCallee.sol#L70)
