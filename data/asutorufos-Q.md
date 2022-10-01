Low Risk Issues

1. Missing checks for address(0x0) when assigning values to address state variables.
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L33
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L54-L55

Cases: 3

2.  If updating >0.8.0  REQUIRE() SHOULD BE USED FOR CHECKING ERROR CONDITIONS ON INPUTS AND RETURN VALUES WHILE ASSERT() SHOULD BE USED FOR INVARIANT CHECKING
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L190
If updating remaining gas will not be refunded incase of failure.

Cases: 1


Non-critical issues

1.  CONSTANTS SHOULD BE DEFINED RATHER THAN USING MAGIC NUMBERS
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L138

2.  Use a more recent version of solidity
Most of the version is around 0.7.6. Since the version is not updated there could be multiple issues of things breaking. 
Having newer versions could let you use things that could be deprecated now. Or better practice. 

3. REQUIRE()/REVERT() STATEMENTS SHOULD HAVE DESCRIPTIVE REASON STRINGS
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L52-L53
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L70-71
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L87
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L43
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L55

Cases: 6
