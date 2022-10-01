# Gas Optimizations Report for QuickSwap and StellaSwap contest

## Overview
During the audit, 4 gas issues were found.

№ | Title | Instance Count
--- | --- | --- 
G-1 | [Postfix increment and decrement](#g-1-postfix-increment-and-decrement) | 1
G-2 | [<>.length in loops](#g-2-length-in-loops) | 1
G-3 | [Initializing variables with default value](#g-3-initializing-variables-with-default-value) | 1
G-4 | [x += y is more expensive than x = x + y](#g-4-x--y-is--more-expensive-than-x--x--y) | 21

## Gas Optimizations Findings (4)
### G-1. Postfix increment and decrement
##### Description
Prefix increment and decrement cost less gas than postfix.

##### Instances
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307

##### Recommendation
Consider using prefix increment and decrement where it is relevant. 

#
### G-2. <>.length in loops
##### Description
Reading the length of an array at each iteration of the loop consumes extra gas.

##### Instances
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307

##### Recommendation
Store the length of an array in a variable before the loop, and use it.

#
### G-3. Initializing variables with default value
##### Description
It costs gas to initialize integer variables with 0 or bool variables with false but it is not necessary.

##### Instances
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307

##### Recommendation
Remove initialization for default values.  
For example:
``` for (uint256 i; i < array.length; ++i) {```

#
### G-4. ```x += y``` is  more expensive than ```x = x + y```
##### Instances
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L84
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L88
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L92
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L96
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L100
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L104
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L107
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L58
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L59
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L119
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L79
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L81
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L83
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L251
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L255
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L256
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L100
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L112
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L115
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L92
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L95

##### Recommendation
Use ```x = x + y``` instead of ```x += y```.
Use ```x = x - y``` instead of ```x -= y```.