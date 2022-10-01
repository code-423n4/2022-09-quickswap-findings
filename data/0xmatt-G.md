# CodeArena Gas Optimization Report

## G01 - Use !=0 Instead of >0 On Uints

### Details
When dealing with unsigned integer types, comparisons with `!= 0` are cheaper than with `> 0`. This should only be done with unsigned variables, immutables and constants, unless the author knows that the value will always be 0 or positive.

### Instances

src/core/contracts/AlgebraPool.sol::224 => require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges
src/core/contracts/AlgebraPool.sol::228 => if (_liquidityCooldown > 0) { 
src/core/contracts/AlgebraPool.sol::434 => require(liquidityDesired > 0, 'IL');
src/core/contracts/AlgebraPool.sol::469 => require(liquidityActual > 0, 'IIL2');
src/core/contracts/AlgebraPool.sol::617 => if (communityFee > 0) { 
src/core/contracts/AlgebraPool.sol::667 => if (communityFee > 0) { 
src/core/contracts/AlgebraPool.sol::808 => if (cache.communityFee > 0) { 
src/core/contracts/AlgebraPool.sol::814 => if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);
src/core/contracts/AlgebraPool.sol::898 => require(_liquidity > 0, 'L');
src/core/contracts/AlgebraPool.sol::924 => if (paid0 > 0) { 
src/core/contracts/AlgebraPool.sol::927 => if (_communityFeeToken0 > 0) { 
src/core/contracts/AlgebraPool.sol::938 => if (paid1 > 0) { 
src/core/contracts/AlgebraPool.sol::941 => if (_communityFeeToken1 > 0) { 
src/core/contracts/DataStorageOperator.sol::46 => require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
src/core/contracts/DataStorageOperator.sol::138 => if (volume >= 2**192) volumeShifted = (type(uint256).max) / (liquidity > 0 ? liquidity : 1);
src/core/contracts/DataStorageOperator.sol::139 => else volumeShifted = (volume << 64) / (liquidity > 0 ? liquidity : 1);
src/core/contracts/libraries/DataStorage.sol::80 => last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0
src/core/contracts/libraries/PriceMovementMath.sol::52 => require(price > 0);
src/core/contracts/libraries/PriceMovementMath.sol::53 => require(liquidity > 0);

## G02 - Cache Array Length Outside Of Loop

### Details
Enumerating storage array length inside a loop creates an extra SLOAD operation for each iteration after the first. It is more gas efficient to use a temporary in-function variable to store the length, then read that value in the loop.

### Instances
src/core/contracts/libraries/DataStorage.sol::307 => for (uint256 i = 0; i < secondsAgos.length; i++) {

## G03 - unchecked { ++i;} is cheaper than i++ or i=i+1

Gas costs vary for different types of increment. In loops, the most gas-optimal option is `unchecked {++i};`, followed by `++i`. In [DataStorage.sol](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307-L315) the following loop is defined:

```
for (uint256 i = 0; i < secondsAgos.length; i++) {
```

Aside from storing array length outside of the loop (G02), this loop can be gas optimized as follows:

```
for (uint256 i = 0; i < secondsAgos.length; ++i) {
	// More readable, predecrement
```

If the authors are confident that secondsAgos.length will never exceed uint256 size using unchecked will provide further gas savings:

```
for (uint256 i = 0; i < secondsAgos.length;) {
	// ... rest of loop code to end of L314
	unchecked { ++i; }
}
```