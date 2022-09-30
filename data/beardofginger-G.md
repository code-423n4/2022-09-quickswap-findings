# > 0 is less efficient than != 0 for unsigned integers
The gas cost can be reduced by using != 0 instead of > 0 in the following locations:


libraries/PriceMovementMath.sol, Line 52:

		require(price > 0);

libraries/PriceMovementMath.sol, Line 53:

		require(liquidity > 0);

libraries/DataStorage.sol, Line 80:

		last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0

DataStorageOperator.sol, Line 138:

		if (volume >= 2**192) volumeShifted = (type(uint256).max) / (liquidity > 0 ? liquidity : 1);

DataStorageOperator.sol, Line 139:

		else volumeShifted = (volume << 64) / (liquidity > 0 ? liquidity : 1);

AlgebraPool.sol, Line 224:

		require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges

AlgebraPool.sol, Line 228:

		if (_liquidityCooldown > 0) {

AlgebraPool.sol, Line 237:

		liquidityNext > 0 ? (liquidityDelta > 0 ? _blockTimestamp() : lastLiquidityAddTimestamp) : 0

AlgebraPool.sol, Line 434:

		require(liquidityDesired > 0, 'IL');

AlgebraPool.sol, Line 451:

		if (amount0 > 0) receivedAmount0 = balanceToken0();

AlgebraPool.sol, Line 452:

		if (amount1 > 0) receivedAmount1 = balanceToken1();

AlgebraPool.sol, Line 454:

		if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');

AlgebraPool.sol, Line 455:

		if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');

AlgebraPool.sol, Line 469:

		require(liquidityActual > 0, 'IIL2');

AlgebraPool.sol, Line 505:

		if (amount0 > 0) TransferHelper.safeTransfer(token0, recipient, amount0);

AlgebraPool.sol, Line 506:

		if (amount1 > 0) TransferHelper.safeTransfer(token1, recipient, amount1);

AlgebraPool.sol, Line 617:

		if (communityFee > 0) {

AlgebraPool.sol, Line 641:

		require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');

AlgebraPool.sol, Line 645:

		require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');

AlgebraPool.sol, Line 667:

		if (communityFee > 0) {

AlgebraPool.sol, Line 734:

		(cache.amountRequiredInitial, cache.exactInput) = (amountRequired, amountRequired > 0);

AlgebraPool.sol, Line 808:

		if (cache.communityFee > 0) {

AlgebraPool.sol, Line 814:

		if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);

AlgebraPool.sol, Line 898:

		require(_liquidity > 0, 'L');

AlgebraPool.sol, Line 904:

		if (amount0 > 0) {

AlgebraPool.sol, Line 911:

		if (amount1 > 0) {

AlgebraPool.sol, Line 924:

		if (paid0 > 0) {

AlgebraPool.sol, Line 927:

		if (_communityFeeToken0 > 0) {

AlgebraPool.sol, Line 938:

		if (paid1 > 0) {

AlgebraPool.sol, Line 941:

		if (_communityFeeToken1 > 0) {

AlgebraFactory.sol, Line 110:

		require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');

# i++ is less efficient than ++i
The gas cost can be reduced by using ++i or i += 1 instead of i++ in the following location:

libraries/DataStorage.sol, Line 307:

		for (uint256 i = 0; i < secondsAgos.length; i++) {

# Initialising variables to 0 uses unecessary gas
Uninitialised variables default to 0x0. Hence, assigning them 0 uses gas unecessarily.

libraries/DataStorage.sol, Line 307:

		for (uint256 i = 0; i < secondsAgos.length; i++) {