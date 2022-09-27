## 1.DEFAULT VALUE INITIALIZATION

### PROBLEM
If a variable is not set/initialized, it is assumed to have the default value (0, false, 0x0 etc depending on the data type). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

### PROOF OF CONCEPT
Instances include:
[DataStorage.sol#L307](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L307)
[]()

### MITIGATION
```
 for (uint256 i; i < secondsAgos.length; ++i)
```
## Caching the length in for loops

### PROBLEM
In the context of a for-loop that iterates over an array, it costs less gas to cache the array's length in a variable and read from this variable rather than use the arrays .length property. Reading the .length property for on the array will cause a recalculation of the array's length on each iteration of the loop which is a more expensive operation than reading from a stack variable.

For example, the following code:
```
for (uint i; i < arr.length; ++i) {
        // ...
}
````
This extra costs can be avoided by caching the array length (in stack): should be changed to:
```
uint length = arr.length;
for (uint i; i < length; ++i) {
        // ...
}
```
### INSTANCES


## 2.COMPARISONS: != IS MORE EFFICIENT THAN > IN REQUIRE (6 GAS LESS)
!= 0 costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled (6 gas)

For uints the minimum value would be 0 and never a negative value. Since it cannot be a negative value, then the check > 0 is essentially checking that the value is not equal to 0 therefore >0 can be replaced with !=0 which saves gas.

While it may seem that > 0 is cheaper than !=, this is only true without the optimizer enabled and outside a require statement. If you enable the optimizer at 10k AND you’re in a require statement, this will save gas.

## PROOF OF CONCEPT
Instances include:
[PriceMovementMath.sol#L52](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L52)
[PriceMovementMath.sol#L53](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L53)
```
require(price != 0);
require(liquidity != 0);
```
[AlgebraPool.sol#L224](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L224)
[AlgebraPool.sol#L434](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L434)
[AlgebraPool.sol#L454](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L454)
[AlgebraPool.sol#L454](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L455)
[AlgebraPool.sol#L454](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L469)
[AlgebraPool.sol#L454](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L441)
[AlgebraPool.sol#L454](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L445)
[AlgebraPool.sol#L454](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L898)


## 3.Require statements including conditions with the `&&` operator can be broken down in multiple require statements to save gas.

## PROOF OF CONCEPT
Instances include:
[DataStorageOperator.sol#L46](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L46)
changes:
```
require(_feeConfig.gamma1 != 0, 'Gammas must be > 0');
require(_feeConfig.gamma2 != 0, 'Gammas must be > 0');
require(_feeConfig.volumeGamma != 0, 'Gammas must be > 0');
```
[AlgebraFactory.sol#L110](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L110)
```
require(gamma1 != 0, 'Gammas must be > 0');
require(gamma2 != 0 , 'Gammas must be > 0');
require(volumeGamma != 0, 'Gammas must be > 0');
```
[AlgebraPool.sol#L739](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L739)
[AlgebraPool.sol#L743](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L743)
```
require(limitSqrtPrice < currentPrice, 'SPL');
require(limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
```
[AlgebraPool.sol#L953](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L953)
[AlgebraPool.sol#L968](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L968)
```
require((communityFee0 <= Constants.MAX_COMMUNITY_FEE);
require(communityFee1 <= Constants.MAX_COMMUNITY_FEE));
```