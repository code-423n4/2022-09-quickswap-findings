# QA REPORT

## [LOW] The following require statements are missing an error message


### Proof of concept:
- [AlgebraPoolDeployer.sol#L37](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol#L37)
- [AlgebraFactory.sol#L62](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L62)

## [LOW] SafeMath should be used in versions < 0.8
In the following contracts there is a mathematical operation that may overflow. Using safemath would revert the transaction in that case.

### Proof of concept:
- [TickOverflowSafetyEchidnaTest.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TickOverflowSafetyEchidnaTest.sol)
- [TickTableEchidnaTest.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TickTableEchidnaTest.sol)

## [LOW] In the following functions consider verifying the fee parameter
Where the fee parameter validation is checking greater than 0% (which may happen by mistake) and less than 100%

### Proof of concept:
- [AdaptiveFeeEchidnaTest.sol#L39](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/AdaptiveFeeEchidnaTest.sol#L39)
- [SimulationTimeFactory.sol#L108](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/simulation/SimulationTimeFactory.sol#L108)

## [LOW] Missing nonReentrancy modifier
The following functions allows attackers to try reentrancy since they are calling to external contracts / transferring eth. Consider adding a nonReentrancy modifier.

### Proof of concept:
- [DataStorageTest.sol#L42](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/DataStorageTest.sol#L42)
- [TestAlgebraRouter.sol#L44](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraRouter.sol#L44)

## [LOW] Use safeTransfer
Use safeTransfer in the following locations

### Proof of concept:
- [TestAlgebraSwapPay.sol#L81](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraSwapPay.sol#L81)
- [TestAlgebraSwapPay.sol#L82](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraSwapPay.sol#L82)

## [LOW] Not verified input
At the following functions you should verify the parameters that are being assigned to a state variable.

### Proof of concept:
- [AlgebraFactory.sol#L84](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L84)
- [SimulationTimeFactory.sol#L91](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/simulation/SimulationTimeFactory.sol#L91)

## [LOW] Consider replacing assert with require
Assertions are a bad practice, use require instead.

### Proof of concept:
- [TickTableEchidnaTest.sol#L38](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TickTableEchidnaTest.sol#L38)
- [TokenDeltaMathEchidnaTest.sol#L112](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TokenDeltaMathEchidnaTest.sol#L112)

## [LOW] Missing pause functionality


### Proof of concept:
- [TestAlgebraSwapPay.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraSwapPay.sol)
- [TestAlgebraCallee.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraCallee.sol)

## [NON CRITICAL] The following events are not indexed


### Proof of concept:
- [TestAlgebraRouter.sol#L38](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraRouter.sol#L38)
- [TestAlgebraCallee.sol#L131](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraCallee.sol#L131)

## [NON CRITICAL] Floating pragma
Floating pragma is a bad practice, since it does not guaranty the same version at future deployments.

### Proof of concept:
- [LiquidityMathTest.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/LiquidityMathTest.sol)
- [UnsafeMathEchidnaTest.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/UnsafeMathEchidnaTest.sol)

## [NON CRITICAL] Consider emitting an event at the following functions


### Proof of concept:
- [DataStorageEchidnaTest.sol#L13](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/DataStorageEchidnaTest.sol#L13)
- [DataStorageTest.sol#L42](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/DataStorageTest.sol#L42)

## [NON CRITICAL] Missing function spec comments


### Proof of concept:
- [TickTableTest.sol#L25](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TickTableTest.sol#L25)
- [TickTableTest.sol#L32](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TickTableTest.sol#L32)

