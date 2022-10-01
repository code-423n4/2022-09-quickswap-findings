# GAS REPORT

## [GAS 00] Caching of msg.sender is unnecessary in the following locations


### Proof of concept:
- [FullMathEchidnaTest.sol#L35](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/FullMathEchidnaTest.sol#L35)
- [DataStorageEchidnaTest.sol#L134](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/DataStorageEchidnaTest.sol#L134)
- [DataStorageTest.sol#L44](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/DataStorageTest.sol#L44)
- [DataStorageOperator.sol#L156](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L156)
- [PriceMovementMathEchidnaTest.sol#L50](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/PriceMovementMathEchidnaTest.sol#L50)

## [GAS 01] Declare as payable If the onlyOwner modifier is present
You can save gas by adding a payable modifier to functions that can be only called by protocol owners.

### Proof of concept:
- [SimulationTimePoolDeployer.sol#L49](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/simulation/SimulationTimePoolDeployer.sol#L49)
- [AlgebraFactory.sol#L77](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L77)
- [AlgebraPoolDeployer.sol#L36](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol#L36)
- [DataStorageOperator.sol#L70](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L70)
- [DataStorageOperator.sol#L115](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L115)
