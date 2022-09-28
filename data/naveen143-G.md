### EXPRESSIONS FOR CONSTANT VALUES SHOULD USE IMMUTABLE RATHER THAN CONSTANT IT WILL SAVE SOME GAS

Change the constant to Immutable throughout the contract the parameters are immutable, this will save some gas as access to const literals are resolved at compile time and do not require Sload’s.

These are some instances of this issue:


> https://github.com/code-423n4/2022-09-quickswap/blob/59c93c65443f9dec68473ca82740abfd8dd47ac3/src/core/contracts/libraries/Constants.sol

> https://github.com/code-423n4/2022-09-quickswap/blob/59c93c65443f9dec68473ca82740abfd8dd47ac3/src/core/contracts/libraries/TickMath.sol

> https://github.com/code-423n4/2022-09-quickswap/blob/59c93c65443f9dec68473ca82740abfd8dd47ac3/src/core/contracts/DataStorageOperator.sol



