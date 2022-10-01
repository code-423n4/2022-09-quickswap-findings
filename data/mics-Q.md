Table Of Content
================

* [QA REPORT](#qa-report)
        * [Unused success return value](#unused-success-return-value)
        * [Use safe math for solidity version <8](#use-safe-math-for-solidity-version-8)
        * [SPDX license not provided in source file](#spdx-license-not-provided-in-source-file)
        * [Require with empty error message](#require-with-empty-error-message)
        * [Loss of precision by using division over possible multiplication](#loss-of-precision-by-using-division-over-possible-multiplication)
        * [Use safeTransfer() instead transfer()](#use-safetransfer-instead-transfer)
        * [Array access is out of bounds](#array-access-is-out-of-bounds)
        * [Missing two steps verification process](#missing-two-steps-verification-process)
        * [Missing 0 address check at transfer](#missing-0-address-check-at-transfer)
        * [Magical number should be documented and explained. Use a constant instead](#magical-number-should-be-documented-and-explained-use-a-constant-instead)
        * [Several functions are declaring named returns but then are using return statements. I suggest choosing only one for readability reasons.](#several-functions-are-declaring-named-returns-but-then-are-using-return-statements-i-suggest-choosing-only-one-for-readability-reasons)
        * [Consider adding constant variables instead of hardcoded strings](#consider-adding-constant-variables-instead-of-hardcoded-strings)
        * [Events not emitted for important state changes](#events-not-emitted-for-important-state-changes)
        * [Add event to the following functions](#add-event-to-the-following-functions)

# QA REPORT

## Unused success return value
The following calls ignores the return value of the called function that might indicate the the call failed.

### Code Instances:
- [TestAlgebraRouter.sol#L63](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraRouter.sol#L63)
- [AlgebraPoolSwapTest.sol#L40](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/AlgebraPoolSwapTest.sol#L40)
- [AlgebraPoolSwapTest.sol#L38](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/AlgebraPoolSwapTest.sol#L38)
- [TestAlgebraRouter.sol#L65](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraRouter.sol#L65)

## Use safe math for solidity version <8
You should use safe math for solidity version <8 since there is no default over/under flow check it those versions.'

### Code Instances:
- [DataStorageEchidnaTest.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/DataStorageEchidnaTest.sol)
- [AlgebraFactory.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol)
- [TestAlgebraReentrantCallee.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraReentrantCallee.sol)
- [PriceMovementMathTest.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/PriceMovementMathTest.sol)

## SPDX license not provided in source file
Before publishing, consider adding a comment containing 'SPDX-License-Identifier: MIT' at the beginning of each source file.

### Code Instances:
- [TestAlgebraReentrantCallee.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraReentrantCallee.sol)
- [AdaptiveFeeTest.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/AdaptiveFeeTest.sol)
- [TickTableTest.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TickTableTest.sol)
- [FullMathEchidnaTest.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/FullMathEchidnaTest.sol)

## Require with empty error message
The following instances of a require statements comes with no popper error message. That means in case of error thrown the user will not know the reason for the error. Consider adding an error message.

### Code Instances:
- [PriceMovementMathEchidnaTest.sol#L14](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/PriceMovementMathEchidnaTest.sol#L14)
- [TokenDeltaMathEchidnaTest.sol#L234](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TokenDeltaMathEchidnaTest.sol#L234)
- [TokenDeltaMathEchidnaTest.sol#L232](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TokenDeltaMathEchidnaTest.sol#L232)
- [TickTableEchidnaTest.sol#L33](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TickTableEchidnaTest.sol#L33)

## Loss of precision by using division over possible multiplication
In cases of computing a / b < c you could improve precision by doing instead a < c * b.

### Code Instances:
- [DataStorageOperator.sol#L137](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L137)
- [DataStorageOperator.sol#L138](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L138)

## Use safeTransfer() instead transfer()
Use openzeppelin safeTransfer() method instead of transfer() in the following locations.

### Code Instances:
- [TestAlgebraCallee.sol#L167](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraCallee.sol#L167)
- [TestAlgebraCallee.sol#L101](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraCallee.sol#L101)
- [TestAlgebraCallee.sol#L139](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraCallee.sol#L139)
- [TestAlgebraSwapPay.sol#L81](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraSwapPay.sol#L81)

## Array access is out of bounds
There is no check for the access to be in the array bounds.

### Code Instances:
- [DataStorageOperator.sol#L70](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L70)
- [DataStorageTest.sol#L100](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/DataStorageTest.sol#L100)

## Missing two steps verification process
The process of transferring ownership is dangerous since typing the wrong address can lead to severe implications. It is better to have to steps verification process with set and claim functions to decrease the chances of human error. Consider changing to two steps verification process of transferring privileges. Human mistakes can happen.

### Code Instances:
- [AlgebraFactory.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol)
- [SimulationTimeFactory.sol](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/simulation/SimulationTimeFactory.sol)

## Missing 0 address check at transfer
Some contracts does not support 0 transfer, then the transaction will revert with no explanation. We recommend to add a require statement that the amount is not 0.

### Code Instances:
- [AlgebraPool.sol#L912](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L912)
- [AlgebraPoolSwapTest.sol#L38](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/AlgebraPoolSwapTest.sol#L38)
- [AlgebraPool.sol#L547](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L547)
- [TestAlgebraSwapPay.sol#L82](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestAlgebraSwapPay.sol#L82)

## Magical number should be documented and explained. Use a constant instead


### Code Instances:
- [DataStorageEchidnaTest.sol#L69](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/DataStorageEchidnaTest.sol#L69)
- [DataStorageEchidnaTest.sol#L100](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/DataStorageEchidnaTest.sol#L100)
- [DataStorageEchidnaTest.sol#L136](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/DataStorageEchidnaTest.sol#L136)
- [TickTableEchidnaTest.sol#L29](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TickTableEchidnaTest.sol#L29)

## Several functions are declaring named returns but then are using return statements. I suggest choosing only one for readability reasons.
Using both named returns and a return statement isn't necessary. Removing one of those can improve code clarity.

### Code Instances:
- [AlgebraPool.sol#L557](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L557)
- [SafeMathTest.sol#L40](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/SafeMathTest.sol#L40)
- [AdaptiveFeeTest.sol#L21](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/AdaptiveFeeTest.sol#L21)
- [PriceMovementMathTest.sol#L22](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/PriceMovementMathTest.sol#L22)

## Consider adding constant variables instead of hardcoded strings
A good practice is to use constant variables instead of hardcoded strings in the code.

### Code Instances:
- [TestERC20.sol#L25](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestERC20.sol#L25)
- [AlgebraFactory.sol#L108](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L108)
- [TestERC20.sol#L66](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestERC20.sol#L66)
- [AlgebraPool.sol#L934](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L934)

## Events not emitted for important state changes
When changing state variables events are not emitted. Emitting events allows monitoring activities with off-chain monitoring tools.

### Code Instances:
- [TestERC20.sol#L45](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestERC20.sol#L45)
- [SimulationTimePoolDeployer.sol#L49](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/simulation/SimulationTimePoolDeployer.sol#L49)
- [DataStorageTest.sol#L24](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/DataStorageTest.sol#L24)
- [DataStorageTest.sol#L42](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/DataStorageTest.sol#L42)

## Add event to the following functions


### Code Instances:
- [DataStorageEchidnaTest.sol#L21](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/DataStorageEchidnaTest.sol#L21)
- [DataStorageTest.sol#L24](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/DataStorageTest.sol#L24)
- [TickOverflowSafetyEchidnaTest.sol#L41](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TickOverflowSafetyEchidnaTest.sol#L41)
- [TestERC20.sol#L45](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/test/TestERC20.sol#L45)
