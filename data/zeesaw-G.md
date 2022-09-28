## Issues

### Cache Array Length Outside of Loop

```
src/core/contracts/libraries/DataStorage.sol::294 => tickCumulatives = new int56[](secondsAgos.length);
src/core/contracts/libraries/DataStorage.sol::295 => secondsPerLiquidityCumulatives = new uint160[](secondsAgos.length);
src/core/contracts/libraries/DataStorage.sol::296 => volatilityCumulatives = new uint112[](secondsAgos.length);
src/core/contracts/libraries/DataStorage.sol::297 => volumePerAvgLiquiditys = new uint256[](secondsAgos.length);
src/core/contracts/libraries/DataStorage.sol::307 => for (uint256 i = 0; i < secondsAgos.length; i++) {
```
### Use != 0 instead of > 0 for Unsigned Integer Comparison

```
src/core/contracts/AlgebraFactory.sol::110 => require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
src/core/contracts/AlgebraPool.sol::224 => require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges
src/core/contracts/AlgebraPool.sol::228 => if (_liquidityCooldown > 0) {
src/core/contracts/AlgebraPool.sol::237 => liquidityNext > 0 ? (liquidityDelta > 0 ? _blockTimestamp() : lastLiquidityAddTimestamp) : 0
src/core/contracts/AlgebraPool.sol::434 => require(liquidityDesired > 0, 'IL');
src/core/contracts/AlgebraPool.sol::451 => if (amount0 > 0) receivedAmount0 = balanceToken0();
src/core/contracts/AlgebraPool.sol::452 => if (amount1 > 0) receivedAmount1 = balanceToken1();
src/core/contracts/AlgebraPool.sol::454 => if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');
src/core/contracts/AlgebraPool.sol::455 => if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');
src/core/contracts/AlgebraPool.sol::469 => require(liquidityActual > 0, 'IIL2');
src/core/contracts/AlgebraPool.sol::505 => if (amount0 > 0) TransferHelper.safeTransfer(token0, recipient, amount0);
src/core/contracts/AlgebraPool.sol::506 => if (amount1 > 0) TransferHelper.safeTransfer(token1, recipient, amount1);
src/core/contracts/AlgebraPool.sol::617 => if (communityFee > 0) {
src/core/contracts/AlgebraPool.sol::641 => require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');
src/core/contracts/AlgebraPool.sol::645 => require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');
src/core/contracts/AlgebraPool.sol::667 => if (communityFee > 0) {
src/core/contracts/AlgebraPool.sol::808 => if (cache.communityFee > 0) {
src/core/contracts/AlgebraPool.sol::814 => if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);
src/core/contracts/AlgebraPool.sol::898 => require(_liquidity > 0, 'L');
src/core/contracts/AlgebraPool.sol::904 => if (amount0 > 0) {
src/core/contracts/AlgebraPool.sol::911 => if (amount1 > 0) {
src/core/contracts/AlgebraPool.sol::924 => if (paid0 > 0) {
src/core/contracts/AlgebraPool.sol::927 => if (_communityFeeToken0 > 0) {
src/core/contracts/AlgebraPool.sol::938 => if (paid1 > 0) {
src/core/contracts/AlgebraPool.sol::941 => if (_communityFeeToken1 > 0) {
src/core/contracts/DataStorageOperator.sol::139 => else volumeShifted = (volume << 64) / (liquidity > 0 ? liquidity : 1);
src/core/contracts/libraries/DataStorage.sol::80 => last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0
src/core/contracts/libraries/PriceMovementMath.sol::52 => require(price > 0);
src/core/contracts/libraries/PriceMovementMath.sol::53 => require(liquidity > 0);
```

### Use Shift Right/Left instead of Division/Multiplication if possible

```
src/core/contracts/libraries/AdaptiveFee.sol::53 => uint256 g8 = uint256(g)**8; // < 128 bits (8*16)
src/core/contracts/libraries/AdaptiveFee.sol::60 => uint256 g8 = uint256(g)**8; // < 128 bits (8*16)
src/core/contracts/libraries/AdaptiveFee.sol::88 => res += (xLowestDegree * gHighestDegree) / 2;
src/core/contracts/libraries/DataStorage.sol::53 => volatility = uint256((K**2 * sumOfSquares + 6 * B * K * sumOfSequence + 6 * dt * B**2) / (6 * dt**2));
```
