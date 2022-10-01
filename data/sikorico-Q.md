# QA REPORT

## [QA 00] Missing MIT licence for the following contracts


### Proof of concept:
- [AdaptiveFeeTest.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/AdaptiveFeeTest.sol)
- [FullMathTest.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/FullMathTest.sol)
- [PoolState.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol)
- [BitMathEchidnaTest.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/BitMathEchidnaTest.sol)
- [PriceMovementMathTest.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/PriceMovementMathTest.sol)

## [QA 01] Use SafeMath in the following contracts to avoid unexpected overflow/underflow


### Proof of concept:
- [AdaptiveFeeTest.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/AdaptiveFeeTest.sol)
- [DataStorageTest.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/DataStorageTest.sol)
- [BitMathEchidnaTest.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/BitMathEchidnaTest.sol)
- [SimulationTimeFactory.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/simulation/SimulationTimeFactory.sol)
- [TestAlgebraCallee.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraCallee.sol)

## [QA 02] The following require statements are missing an error message


### Proof of concept:
- [TickOverflowSafetyEchidnaTest.sol#L43](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TickOverflowSafetyEchidnaTest.sol#L43)
- [DataStorageEchidnaTest.sol#L106](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/DataStorageEchidnaTest.sol#L106)
- [PriceMovementMathEchidnaTest.sol#L15](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/PriceMovementMathEchidnaTest.sol#L15)
- [TokenDeltaMathEchidnaTest.sol#L142](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TokenDeltaMathEchidnaTest.sol#L142)
- [TickOverflowSafetyEchidnaTest.sol#L83](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TickOverflowSafetyEchidnaTest.sol#L83)

## [QA 03] Timelock is missing for the following functions


### Proof of concept:
- [AlgebraPool.sol#L952](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L952)
- [TestERC20.sol#L45](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestERC20.sol#L45)
- [TickOverflowSafetyEchidnaTest.sol#L41](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TickOverflowSafetyEchidnaTest.sol#L41)
- [AlgebraPool.sol#L959](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L959)

## [QA 04] Not emitted event for state changes


### Proof of concept:
- [SimulationTimePoolDeployer.sol#L49](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/simulation/SimulationTimePoolDeployer.sol#L49)
- [DataStorageEchidnaTest.sol#L13](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/DataStorageEchidnaTest.sol#L13)
- [DataStorageTest.sol#L24](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/DataStorageTest.sol#L24)
- [DataStorageEchidnaTest.sol#L21](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/DataStorageEchidnaTest.sol#L21)
- [SimulationTimePoolDeployer.sol#L31](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/simulation/SimulationTimePoolDeployer.sol#L31)

## [QA 05] Missing event indexers


### Proof of concept:
- [TestAlgebraCallee.sol#L131](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraCallee.sol#L131)
- [TestAlgebraCallee.sol#L146](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraCallee.sol#L146)
- [TestAlgebraCallee.sol#L111](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraCallee.sol#L111)

## [QA 06] Remove the name from the following unused function parameters


### Proof of concept:
- [AlgebraPoolDeployer.sol#L49](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol#L49)
