# [G-01] Improvements on for loop

The following changes would optimize gas for the loop highlighted bellow.

1. Avoid initializing the index with the default value of zero
2. Cache the length of the array outside the loop
3. The increment step can be made uncheck
4. Prefix increment costs less than postfix increment

```
File: src/core/contracts/libraries/DataStorage.sol
307: for (uint256 i = 0; i < secondsAgos.length; i++) {
```

# [G-02] Use != 0 instead of > 0 to save gas.

Replace `> 0` with `!= 0` for unsigned integers.

There are 30 instances of this issue.

```
File: src/core/contracts/AlgebraPool.sol
224: require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges
228: if (_liquidityCooldown > 0) {
237: liquidityNext > 0 ? (liquidityDelta > 0 ? _blockTimestamp() : lastLiquidityAddTimestamp) : 0
434: require(liquidityDesired > 0, 'IL');
451: if (amount0 > 0) receivedAmount0 = balanceToken0();
452: if (amount1 > 0) receivedAmount1 = balanceToken1();
454: if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');
455: if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');
469: require(liquidityActual > 0, 'IIL2');
505: if (amount0 > 0) TransferHelper.safeTransfer(token0, recipient, amount0);
506: if (amount1 > 0) TransferHelper.safeTransfer(token1, recipient, amount1);
617: if (communityFee > 0) {
641: require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');
645: require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');
667: if (communityFee > 0) {
734: (cache.amountRequiredInitial, cache.exactInput) = (amountRequired, amountRequired > 0);
808: if (cache.communityFee > 0) {
814: if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);
898: require(_liquidity > 0, 'L');
904: if (amount0 > 0) {
911: if (amount1 > 0) {
924: if (paid0 > 0) {
927: if (_communityFeeToken0 > 0) {
938: if (paid1 > 0) {
941: if (_communityFeeToken1 > 0) {
```

```
File: src/core/contracts/DataStorageOperator.sol
138: if (volume >= 2**192) volumeShifted = (type(uint256).max) / (liquidity > 0 ? liquidity : 1);
139: else volumeShifted = (volume << 64) / (liquidity > 0 ? liquidity : 1);
```

```
File: src/core/contracts/libraries/DataStorage.sol
80: last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0
```

```
File: src/core/contracts/libraries/PriceMovementMath.sol
52: require(price > 0);
53: require(liquidity > 0);
```

# [G-03] Use custom errors rather than require/revert strings to save gas

Note that the project would need to update to solidity v0.8.4 to be able to use custom errors.

There are 34 instances of this issue.


```
File: src/core/contracts/AlgebraFactory.sol
109: require(uint256(alpha1) + uint256(alpha2) + uint256(baseFee) <= type(uint16).max, 'Max fee exceeded');
110: require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
```

```
File: src/core/contracts/AlgebraPool.sol
60: require(topTick < TickMath.MAX_TICK + 1, 'TUM');
61: require(topTick > bottomTick, 'TLU');
62: require(bottomTick > TickMath.MIN_TICK - 1, 'TLM');
194: require(globalState.price == 0, 'AI');
224: require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges
434: require(liquidityDesired > 0, 'IL');
454: if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');
455: if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');
469: require(liquidityActual > 0, 'IIL2');
474: require((amount0 = uint256(amount0Int)) <= receivedAmount0, 'IIAM2');
475: require((amount1 = uint256(amount1Int)) <= receivedAmount1, 'IIAM2');
608: require(balance0Before.add(uint256(amount0)) <= balanceToken0(), 'IIA');
614: require(balance1Before.add(uint256(amount1)) <= balanceToken1(), 'IIA');
636: require(globalState.unlocked, 'LOK');
641: require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');
645: require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');
731: require(unlocked, 'LOK');
733: require(amountRequired != 0, 'AS');
739: require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
743: require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
898: require(_liquidity > 0, 'L');
921: require(balance0Before.add(fee0) <= paid0, 'F0');
935: require(balance1Before.add(fee1) <= paid1, 'F1');
```

```
File: src/core/contracts/DataStorageOperator.sol
27: require(msg.sender == pool, 'only pool can call this');
45: require(uint256(_feeConfig.alpha1) + uint256(_feeConfig.alpha2) + uint256(_feeConfig.baseFee) <= type(uint16).max, 'Max fee exceeded');
46: require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
```

```
File: src/core/contracts/base/PoolState.sol
41: require(globalState.unlocked, 'LOK');
```

```
File: src/core/contracts/libraries/DataStorage.sol
238: require(lteConsideringOverflow(self[oldestIndex].blockTimestamp, target, time), 'OLD');
```

```
File: src/core/contracts/libraries/PriceMovementMath.sol
70: require((product = amount * price) / amount == price); // if the product overflows, we know the denominator underflows
71: require(liquidityShifted > product); // in addition, we must check that the denominator does not underflow
```

```
File: src/core/contracts/libraries/TickManager.sol
96: require(liquidityTotalAfter < Constants.MAX_LIQUIDITY_PER_TICK + 1, 'LO');
```

```
File: src/core/contracts/libraries/TickTable.sol
15: require(tick % Constants.TICK_SPACING == 0, 'tick is not spaced'); // ensure that the tick is spaced
```

# [G-04] Use right/left shift instead of division/multiplication to save gas

There are 2 instances of this issue.

```
File: src/core/contracts/libraries/AdaptiveFee.sol
88: res += (xLowestDegree * gHighestDegree) / 2;
96: res += (xLowestDegree * gHighestDegree) / 24;
```

# [G-05] Using private rather than public for constants, saves gas

The values can still be inspected on the source code if necessary

There's 1 instance of this issue.

```
File: src/core/contracts/libraries/DataStorage.sol
12: uint32 public constant WINDOW = 1 days;
```

# [G-06] x += y costs more gas than x = x + y for state variables

There are 2 instances of this issue.

```
File: src/core/contracts/AlgebraPool.sol
257: _position.fees0 += fees0;
258: _position.fees1 += fees1;
```
