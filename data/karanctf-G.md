## [G-1] Use `unchecked` preincrement to save gas
```solidity
DataStorage.sol:307:    for (uint256 i = 0; i < secondsAgos.length; i++) {
```
## [G-2] Cache .length in loops

```solidity
DataStorage.sol:307:    for (uint256 i = 0; i < secondsAgos.length; i++) {
```

## [G-3] Donot use default values Explicit initialization with zero is not required for variable declaration of numTokens because `uints are 0` by default.removeing this will reduce contract size and save a bit of gas.

```solidity
DataStorage.sol:307:    for (uint256 i = 0; i < secondsAgos.length; i++) {
```


## [G-4] Variable == false|0 -> !variable or variable ==  true -> variable
```solidity
TickTable.sol:15:    require(tick % Constants.TICK_SPACING == 0, 'tick is not spaced'); // ensure that the tick is spaced
TickManager.sol:105:    flipped = (liquidityTotalAfter == 0);
TickManager.sol:106:    if (liquidityTotalBefore == 0) {
AlgebraPool.sol:194:    require(globalState.price == 0, 'AI');
AlgebraPool.sol:223:    if (liquidityDelta == 0) {
AlgebraPool.sol:867:      if (amountRequired == 0 || currentPrice == limitSqrtPrice) {
DataStorage.sol:80:    last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0
DataStorage.sol:217:    if (secondsAgo == 0 || lteConsideringOverflow(self[index].blockTimestamp, target, time)) {
PriceMovementMath.sol:57:      if (amount == 0) return price;
```

## [G-05] Use `!= 0` instead of `> 0`
```solidity
AlgebraPool.sol:224:      require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges
AlgebraPool.sol:228:        if (_liquidityCooldown > 0) {
AlgebraPool.sol:237:        liquidityNext > 0 ? (liquidityDelta > 0 ? _blockTimestamp() : lastLiquidityAddTimestamp) : 0
AlgebraPool.sol:434:    require(liquidityDesired > 0, 'IL');
AlgebraPool.sol:451:      if (amount0 > 0) receivedAmount0 = balanceToken0();
AlgebraPool.sol:452:      if (amount1 > 0) receivedAmount1 = balanceToken1();
AlgebraPool.sol:454:      if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');
AlgebraPool.sol:455:      if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');
AlgebraPool.sol:469:    require(liquidityActual > 0, 'IIL2');
AlgebraPool.sol:505:      if (amount0 > 0) TransferHelper.safeTransfer(token0, recipient, amount0);
AlgebraPool.sol:506:      if (amount1 > 0) TransferHelper.safeTransfer(token1, recipient, amount1);
AlgebraPool.sol:617:    if (communityFee > 0) {
AlgebraPool.sol:641:      require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');
AlgebraPool.sol:645:      require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');
AlgebraPool.sol:667:    if (communityFee > 0) {
AlgebraPool.sol:734:      (cache.amountRequiredInitial, cache.exactInput) = (amountRequired, amountRequired > 0);
AlgebraPool.sol:808:      if (cache.communityFee > 0) {
AlgebraPool.sol:814:      if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);
AlgebraPool.sol:898:    require(_liquidity > 0, 'L');
AlgebraPool.sol:904:    if (amount0 > 0) {
AlgebraPool.sol:911:    if (amount1 > 0) {
AlgebraPool.sol:924:    if (paid0 > 0) {
AlgebraPool.sol:927:      if (_communityFeeToken0 > 0) {
AlgebraPool.sol:938:    if (paid1 > 0) {
AlgebraPool.sol:941:      if (_communityFeeToken1 > 0) {
DataStorageOperator.sol:46:    require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
DataStorageOperator.sol:138:    if (volume >= 2**192) volumeShifted = (type(uint256).max) / (liquidity > 0 ? liquidity : 1);
DataStorageOperator.sol:139:    else volumeShifted = (volume << 64) / (liquidity > 0 ? liquidity : 1);
DataStorage.sol:80:    last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0
PriceMovementMath.sol:52:    require(price > 0);
PriceMovementMath.sol:53:    require(liquidity > 0);
```

## [G-6 ]<x> += <y>` costs more gas than `<x> = <x> + <y>` 
```solidity
TickTable.sol:16:    tick /= Constants.TICK_SPACING; // compress tick
TickTable.sol:92:        tick -= int24(255 - getMostSignificantBit(_row));
TickTable.sol:95:        tick -= int24(bitNumber);
TickTable.sol:100:      tick += 1;
TickTable.sol:112:        tick += int24(getSingleSignificantBit(-_row & _row)); // least significant bit
TickTable.sol:115:        tick += int24(255 - bitNumber);
AdaptiveFee.sol:83:    gHighestDegree /= g; // g**7
AdaptiveFee.sol:84:    res += xLowestDegree * gHighestDegree;
AdaptiveFee.sol:86:    gHighestDegree /= g; // g**6
AdaptiveFee.sol:87:    xLowestDegree *= x; // x**2
AdaptiveFee.sol:88:    res += (xLowestDegree * gHighestDegree) / 2;
AdaptiveFee.sol:90:    gHighestDegree /= g; // g**5
AdaptiveFee.sol:91:    xLowestDegree *= x; // x**3
AdaptiveFee.sol:92:    res += (xLowestDegree * gHighestDegree) / 6;
AdaptiveFee.sol:94:    gHighestDegree /= g; // g**4
AdaptiveFee.sol:95:    xLowestDegree *= x; // x**4
AdaptiveFee.sol:96:    res += (xLowestDegree * gHighestDegree) / 24;
AdaptiveFee.sol:98:    gHighestDegree /= g; // g**3
AdaptiveFee.sol:99:    xLowestDegree *= x; // x**5
AdaptiveFee.sol:100:    res += (xLowestDegree * gHighestDegree) / 120;
AdaptiveFee.sol:102:    gHighestDegree /= g; // g**2
AdaptiveFee.sol:103:    xLowestDegree *= x; // x**6
AdaptiveFee.sol:104:    res += (xLowestDegree * gHighestDegree) / 720;
AdaptiveFee.sol:106:    xLowestDegree *= x; // x**7
AdaptiveFee.sol:107:    res += (xLowestDegree * g) / 5040 + (xLowestDegree * x) / (40320);
TickManager.sol:58:      innerFeeGrowth0Token -= upper.outerFeeGrowth0Token;
TickManager.sol:59:      innerFeeGrowth1Token -= upper.outerFeeGrowth1Token;
AlgebraPool.sol:257:      _position.fees0 += fees0;
AlgebraPool.sol:258:      _position.fees1 += fees1;
AlgebraPool.sol:801:        amountRequired -= (step.input + step.feeAmount).toInt256(); // decrease remaining input amount
AlgebraPool.sol:804:        amountRequired += step.output.toInt256(); // increase remaining output amount (since its negative)
AlgebraPool.sol:810:        step.feeAmount -= delta;
AlgebraPool.sol:811:        communityFeeAmount += delta;
AlgebraPool.sol:814:      if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);
AlgebraPool.sol:922:    paid0 -= balance0Before;
AlgebraPool.sol:931:      totalFeeGrowth0Token += FullMath.mulDiv(paid0 - fees0, Constants.Q128, _liquidity);
AlgebraPool.sol:936:    paid1 -= balance1Before;
AlgebraPool.sol:945:      totalFeeGrowth1Token += FullMath.mulDiv(paid1 - fees1, Constants.Q128, _liquidity);
DataStorage.sol:79:    last.tickCumulative += int56(tick) * delta;
DataStorage.sol:80:    last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0
DataStorage.sol:81:    last.volatilityCumulative += uint88(_volatilityOnRange(delta, prevTick, tick, last.averageTick, averageTick)); // always fits 88 bits
DataStorage.sol:83:    last.volumePerLiquidityCumulative += volumePerLiquidity;
DataStorage.sol:119:        index -= 1; // considering underflow
DataStorage.sol:251:      beforeOrAt.tickCumulative += ((atOrAfter.tickCumulative - beforeOrAt.tickCumulative) / timepointTimeDelta) * targetDelta;
DataStorage.sol:252:      beforeOrAt.secondsPerLiquidityCumulative += uint160(
DataStorage.sol:255:      beforeOrAt.volatilityCumulative += ((atOrAfter.volatilityCumulative - beforeOrAt.volatilityCumulative) / timepointTimeDelta) * targetDelta;
DataStorage.sol:256:      beforeOrAt.volumePerLiquidityCumulative +=
```

## [G-07] Splitting `require()` statements that use `&&` saves gas
If you're using the Optimizer at 200, instead of using the `&&` operator in a single require statement to check multiple conditions, I suggest using multiple require statements with 1 condition per require statement
```solidity
AlgebraFactory.sol:110:    require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
```
