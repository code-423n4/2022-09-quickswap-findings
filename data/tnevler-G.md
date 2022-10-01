# Report
## Gas Optimizations ## 

### [G-01]: X += Y (X -= Y) costs more gas than X = X + Y (X = X - Y) 
**Context:** 

1. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L84
2. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L88
3. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L92
4. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L96
5. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L100
6. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L104
7. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L107
8. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L58
9. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L59
10. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L119
11. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L79
12. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L81
13. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L83
14. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L251
15. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L255
16. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L256
17. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L100
18. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L112
19. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L115
20. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L92
21. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L95

**Recommendation:**

Change X += Y (X -= Y) to X = X + Y (X = X - Y).

### [G-02]: Don't initialize variable with its default value 
**Context:** 

DataStorage.sol [L307](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307)

**Description:**

Default value of uint is 0. It's unnecessary and costs more gas to initialize uint variavles to 0.

**Recommendation:**

Change "uint256 i = 0;" to "uint256 i;".


### [G-03]: >0 costs more gas than !=0 
**Context:** 

1. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L52
2. https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L53 

**Description:**

uint160 and uint128 type will never be less than 0.

**Recommendation:**

Change > 0 to !=0.

### [G-04]: i++ costs more gas than ++i 
**Context:**

DataStorage.sol [L307](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307)

**Recommendation:**

Change i++ to ++i.

### [G-05]: Use new variable instead of reading array length in every loop of a for-loop 
**Context:**

DataStorage.sol [L307](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307)

**Description:**

If you read the length of the array at each iteration of the loop, this consumes a lot of gas.

**Recommendation:**

Store the array’s length in a variable before the for-loop, and use this new variable in the loop.
