## pre-increment ("++i") is cheaper
It costs less to use pre-increment ("++i") rather than post-increment( "i++" ) around 5 gas cheaper  than post-increment 
Example:
[Line 307](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307)



## Default Values
If  a variable is not initialized it will have a default value (0x0, false, 0, etc., depending on the data type)
 so instead of assigning a variable to zero like in [Line 307](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307) it can be rewritten as "uint256 i;" this is the same and saves gas because the initial value is zero for uint types



## Custom Errors
It is cheaper to use custom errors rather than require because it is both cheaper in deployment and runtime cost!
example:
[Line 43](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L43)
[Line 109](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L109)

## Remove Shorthand Addition/Subtraction Assignment
Instead of using the shorthand of addition/subtraction assignment operators ("+=", "-=")  it costs less to remove the shorthand (```x += y ``` same as ``` x = x+y```) 
For example: 
[Line 936](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L936) could be refactored to `paid1 = paid1 - balance1Before;`, 
[Line 811](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L811) could be refactored to `communityFeeAmount = communityFeeAmount + delta;` 
[Line 945](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L945) could be refactored to `totalFeeGrowth1Token = totalFeeGrowth1Token + FullMath.mulDiv(paid1 - fees1, Constants.Q128, _liquidity);`

## Seperating Conditions
Rather than using the "&&" operator for multiple checks on a singular require statement, it costs less gas  to use various require statements with one condition (saves about 3 gas)
Examples:
[Line 110](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L110)
[Line 953](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L953)

## Comparison Operators
 It costs less gas to use greater/less than operator (">", "<") than greater/less than or equal to operator  (">=", "<=") for example the two lines 608, 229 can be rewritten as:

[Line 608](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L608) could be refactored to `require(balance0Before.add(uint256(amount0)) < balanceToken0() + 1, 'IIA');`

[Line 229](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L229) could be refactored to `require((_blockTimestamp() - lastLiquidityAddTimestamp) > _liquidityCooldown - 1);`

