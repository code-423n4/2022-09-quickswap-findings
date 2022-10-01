# GAS REPORT

## [GAS] Mark as payable If has onlyOwner modifier
In order to save gas you can put a payable modifier for functions that are called by protocol owners.

### Proof of concept:
- [DataStorageOperator.sol#L70](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L70)
- [SimulationTimePoolDeployer.sol#L49](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/simulation/SimulationTimePoolDeployer.sol#L49)

## [GAS] Use > instead != to compare uint with 0


### Proof of concept:
- [PriceMovementMathEchidnaTest.sol#L15](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/PriceMovementMathEchidnaTest.sol#L15)
- [AlgebraPool.sol#L903](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L903)

## [GAS] Use assembly opcodes iszero in the following locations


### Proof of concept:
- [TickOverflowSafetyEchidnaTest.sol#L66](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TickOverflowSafetyEchidnaTest.sol#L66)
- [TokenDeltaMathEchidnaTest.sol#L218](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TokenDeltaMathEchidnaTest.sol#L218)

## [GAS] Use abiEncodePacked()


### Proof of concept:
- [TestAlgebraSwapPay.sol#L64](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraSwapPay.sol#L64)
- [TestAlgebraReentrantCallee.sol#L50](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraReentrantCallee.sol#L50)

--