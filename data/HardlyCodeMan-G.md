## G001 - Don't Initialize Variables with Default Value
### TickMath.sol
1. Line 71 - uint256 msb = 0;
    - Fix - uint256 msb;

## G003 - Use != 0 instead of > 0 for Unsigned Integer Comparison
### AlgebraPool.sol
1. Line 224 - require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges
    - Fix - require(currentLiquidity != 0, 'NP'); // Do not recalculate the empty ranges
2. Line 228 - if (_liquidityCooldown > 0) {
    - Fix - if (_liquidityCooldown != 0) {
3. Line 237 - liquidityNext > 0 ? (liquidityDelta > 0 ? _blockTimestamp() : lastLiquidityAddTimestamp) : 0
    - Fix - liquidityNext != 0 ? (liquidityDelta > 0 ? _blockTimestamp() : lastLiquidityAddTimestamp) : 0
4. Line 434 - require(liquidityDesired > 0, 'IL');
    - Fix - require(liquidityDesired != 0, 'IL');
5. Line 451 - if (amount0 > 0) receivedAmount0 = balanceToken0();
    - Fix - if (amount0 != 0) receivedAmount0 = balanceToken0();
6. Line 452 - if (amount1 > 0) receivedAmount1 = balanceToken1();
    - Fix - if (amount1 != 0) receivedAmount1 = balanceToken1();
7. Line 454 - if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');
    - Fix - if (amount0 != 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) != 0, 'IIAM');
8. Line 455 - if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');
    - Fix - if (amount1 != 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) != 0, 'IIAM');
9. Line 469 - require(liquidityActual > 0, 'IIL2');
    - Fix - require(liquidityActual != 0, 'IIL2');
10. Line 505 - if (amount0 > 0) TransferHelper.safeTransfer(token0, recipient, amount0);
    - Fix - if (amount0 != 0) TransferHelper.safeTransfer(token0, recipient, amount0);
11. Line 506 - if (amount1 > 0) TransferHelper.safeTransfer(token1, recipient, amount1);
    - Fix - if (amount1 != 0) TransferHelper.safeTransfer(token1, recipient, amount1);
12. Line 617 - if (communityFee > 0) { 
    - Fix - if (communityFee != 0) { 
13. Line 667 - if (communityFee > 0) { 
    - Fix - if (communityFee != 0) { 
14. Line 808 - if (cache.communityFee > 0) { 
    - Fix - if (cache.communityFee != 0) { 
15. Line 814 - if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);
    - Fix - if (currentLiquidity != 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);
16. Line 898 - require(_liquidity > 0, 'L');
    - Fix - require(_liquidity != 0, 'L');
17. Line 904 - if (amount0 > 0) {
    - Fix - if (amount0 != 0) {
18. Line 911 - if (amount1 > 0) {
    - Fix - if (amount1 != 0) {
19. Line 924 - if (paid0 > 0) {
    - Fix - if (paid0 != 0) {
20. Line 927 - if (_communityFeeToken0 > 0) {
    - Fix - if (_communityFeeToken0 != 0) {
21. Line 938 - if (paid1 > 0) {
    - Fix - if (paid1 != 0) {
22. Line 941 - if (_communityFeeToken1 > 0) {
    - Fix - if (_communityFeeToken1 != 0) {

### DataStorageOperator.sol
1. Line 138 - if (volume >= 2**192) volumeShifted = (type(uint256).max) / (liquidity > 0 ? liquidity : 1);
    - Fix - if (volume >= 2**192) volumeShifted = (type(uint256).max) / (liquidity != 0 ? liquidity : 1);
2. Line 139 - else volumeShifted = (volume << 64) / (liquidity > 0 ? liquidity : 1);
    - Fix - else volumeShifted = (volume << 64) / (liquidity != 0 ? liquidity : 1);

### DataStorage.sol
1. Line 80 - last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1));
    - Fix - last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity != 0 ? liquidity : 1));

### FullMath.sol
1. Line 114 - require(denominator > 0);
    - Fix - require(denominator != 0);
2. Line 120 - if (mulmod(a, b, denominator) > 0) {
    - Fix - if (mulmod(a, b, denominator) != 0) {

### PriceMovementMath.sol
1. Line 52 - require(price > 0);
    - Fix - require(price != 0);
2. Line 53 - require(liquidity > 0);
    - Fix - require(liquidity != 0);

## G011 - Unnecessary checked arithmetic in for loop
### DataStorage.sol
1. Line 307 - 
    for (uint256 i = 0; i < secondsAgos.length; i++) {
        current = getSingleTimepoint(self, time, secondsAgos[i], tick, index, oldestIndex, liquidity);
        (tickCumulatives[i], secondsPerLiquidityCumulatives[i], volatilityCumulatives[i], volumePerAvgLiquiditys[i]) = (
            current.tickCumulative,
            current.secondsPerLiquidityCumulative,
            current.volatilityCumulative,
            current.volumePerLiquidityCumulative
        );
    }
    - Fix -
    for (uint256 i = 0; i < secondsAgos.length;) {
        current = getSingleTimepoint(self, time, secondsAgos[i], tick, index, oldestIndex, liquidity);
        (tickCumulatives[i], secondsPerLiquidityCumulatives[i], volatilityCumulatives[i], volumePerAvgLiquiditys[i]) = (
            current.tickCumulative,
            current.secondsPerLiquidityCumulative,
            current.volatilityCumulative,
            current.volumePerLiquidityCumulative
        );
        unchecked { ++i; }
    } 

## G012 - Use Prefix Increment instead of Postfix Increment if possible
### DataStorage.sol
1. G011 DataStorage.sol 307 - unchecked { i++; }
    - Consider Fix - unchecked { ++i; }

NB. This is my first submission and first time using markdown, please provide feedback on layout and improvements if time allows.