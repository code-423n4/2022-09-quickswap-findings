# QA Report for QuickSwap and StellaSwap contest

## Overview
During the audit, 2 non-critical issues were found.

№ | Title | Risk Rating  | Instance Count
--- | --- | --- | ---
NC-1 | [Maximum line length exceeded](#nc-1-maximum-line-length-exceeded) | Non-Critical | 32
NC-2 | [Order of Functions](#nc-2-order-of-functions) | Non-Critical | 3

## Non-Critical Risk Findings (2)
### NC-1. Maximum line length exceeded
##### Description
Some lines of code are too long.

##### Instances 
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L112
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L123
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L221
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L297
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L352
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L355
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L383
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L392
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L472
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L558
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L577
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L601
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L654
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L876
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L880
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L45
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L78
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L38
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L133
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L251
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L253
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L255
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L408
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L80
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L33
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L34
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L35
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L36
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L37
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L38
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L39
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TokenDeltaMath.sol#L53

##### Recommendation
According to [Style Guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#maximum-line-length), maximum suggested line length is 120 characters.  
Make the lines shorter.

#
### NC-2. Order of Functions
##### Description
According to [Style Guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-functions), ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier.  
Functions should be grouped according to their visibility and ordered:
1) constructor
2) receive function (if exists)
3) fallback function (if exists)
4) external
5) public
6) internal
7) private

##### Instances
- [private functions between internal](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L66) and [(2)](
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L94), [(3)](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L148).


##### Recommendation
Reorder functions where possible.