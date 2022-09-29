### G01: custom error save more gas
#### problem
Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met) while providing the same amount of information, as explained https://blog.soliditylang.org/2021/04/21/custom-errors/. 
Custom errors are defined using the error statement.
#### prof
libraries/TickManager.sol, 96, b"    require(liquidityTotalAfter < Constants.MAX_LIQUIDITY_PER_TICK + 1, 'LO');"
DataStorageOperator.sol, 45, b"    require(uint256(_feeConfig.alpha1) + uint256(_feeConfig.alpha2) + uint256(_feeConfig.baseFee) <= type(uint16).max, 'Max fee exceeded');"
DataStorageOperator.sol, 46, b"    require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');"
AlgebraPool.sol, 194, b"    require(globalState.price == 0, 'AI');"
AlgebraPool.sol, 224, b"      require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges"
AlgebraPool.sol, 434, b"    require(liquidityDesired > 0, 'IL');"
AlgebraPool.sol, 454, b"      if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');"
AlgebraPool.sol, 455, b"      if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');"
AlgebraPool.sol, 469, b"    require(liquidityActual > 0, 'IIL2');"
AlgebraPool.sol, 474, b"      require((amount0 = uint256(amount0Int)) <= receivedAmount0, 'IIAM2');"
AlgebraPool.sol, 475, b"      require((amount1 = uint256(amount1Int)) <= receivedAmount1, 'IIAM2');"
AlgebraPool.sol, 614, b"      require(balance1Before.add(uint256(amount1)) <= balanceToken1(), 'IIA');"
AlgebraPool.sol, 608, b"      require(balance0Before.add(uint256(amount0)) <= balanceToken0(), 'IIA');"
AlgebraPool.sol, 636, b"    require(globalState.unlocked, 'LOK');"
AlgebraPool.sol, 645, b"      require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');"
AlgebraPool.sol, 641, b"      require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');"
AlgebraPool.sol, 731, b"      require(unlocked, 'LOK');"
AlgebraPool.sol, 733, b"      require(amountRequired != 0, 'AS');"
AlgebraPool.sol, 743, b"        require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');"
AlgebraPool.sol, 739, b"        require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');"
AlgebraPool.sol, 898, b"    require(_liquidity > 0, 'L');"
AlgebraPool.sol, 921, b"    require(balance0Before.add(fee0) <= paid0, 'F0');"
AlgebraPool.sol, 935, b"    require(balance1Before.add(fee1) <= paid1, 'F1');"
libraries/TickTable.sol, 15, b"    require(tick % Constants.TICK_SPACING == 0, 'tick is not spaced'); // ensure that the tick is spaced"
AlgebraFactory.sol, 109, b"    require(uint256(alpha1) + uint256(alpha2) + uint256(baseFee) <= type(uint16).max, 'Max fee exceeded');"
AlgebraFactory.sol, 110, b"    require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');"
libraries/DataStorage.sol, 238, b"    require(lteConsideringOverflow(self[oldestIndex].blockTimestamp, target, time), 'OLD');"
### G02: COMPARISONS WITH ZERO FOR UNSIGNED INTEGERS
#### problem
0 is less gas efficient than !0 if you enable the optimizer at 10k AND you’re in a require statement. Detailed explanation with the opcodes https://twitter.com/gzeon/status/1485428085885640706
#### prof
AlgebraPool.sol, 228, b'        if (_liquidityCooldown > 0) {'
AlgebraPool.sol, 224, b"      require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges"
AlgebraPool.sol, 434, b"    require(liquidityDesired > 0, 'IL');"
AlgebraPool.sol, 451, b'      if (amount0 > 0) receivedAmount0 = balanceToken0();'
AlgebraPool.sol, 452, b'      if (amount1 > 0) receivedAmount1 = balanceToken1();'
AlgebraPool.sol, 454, b"      if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');"
AlgebraPool.sol, 454, b"      if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');"
AlgebraPool.sol, 455, b"      if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');"
AlgebraPool.sol, 455, b"      if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');"
AlgebraPool.sol, 469, b"    require(liquidityActual > 0, 'IIL2');"
AlgebraPool.sol, 505, b'      if (amount0 > 0) TransferHelper.safeTransfer(token0, recipient, amount0);'
AlgebraPool.sol, 506, b'      if (amount1 > 0) TransferHelper.safeTransfer(token1, recipient, amount1);'
AlgebraPool.sol, 617, b'    if (communityFee > 0) {'
AlgebraPool.sol, 667, b'    if (communityFee > 0) {'
AlgebraPool.sol, 808, b'      if (cache.communityFee > 0) {'
AlgebraPool.sol, 814, b'      if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);'
AlgebraPool.sol, 898, b"    require(_liquidity > 0, 'L');"
AlgebraPool.sol, 904, b'    if (amount0 > 0) {'
AlgebraPool.sol, 911, b'    if (amount1 > 0) {'
AlgebraPool.sol, 924, b'    if (paid0 > 0) {'
AlgebraPool.sol, 927, b'      if (_communityFeeToken0 > 0) {'
AlgebraPool.sol, 938, b'    if (paid1 > 0) {'
AlgebraPool.sol, 941, b'      if (_communityFeeToken1 > 0) {'
libraries/PriceMovementMath.sol, 52, b'    require(price > 0);'
libraries/PriceMovementMath.sol, 53, b'    require(liquidity > 0);'


### G03: PREFIX INCREMENT SAVE MORE GAS
#### problem
prefix increment ++i is more cheaper than postfix i++
#### prof
libraries/DataStorage.sol, 307, b'    for (uint256 i = 0; i < secondsAgos.length; i++) {'

### G04: resign the default value to the variables.
#### problem
 resign the default value to the variables will cost more gas.
#### prof
libraries/DataStorage.sol, 307, b'    for (uint256 i = 0; i < secondsAgos.length; i++) {'

### G05: ++I/I++ SHOULD BE UNCHECKED{++I}/UNCHECKED{I++} WHEN IT IS NOT POSSIBLE FOR THEM TO OVERFLOW, AS IS THE CASE WHEN USED IN FOR- AND WHILE-LOOPS
#### problem
The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop
#### prof
libraries/DataStorage.sol, 307, b'    for (uint256 i = 0; i < secondsAgos.length; i++) {'

### G06: Splitting require() statements that use && saves gas
#### problem
See this issue(https://github.com/code-423n4/2022-01-xdefi-findings/issues/128) which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper
#### prof:
DataStorageOperator.sol, 46, b"    require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');"
AlgebraPool.sol, 743, b"        require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');"
AlgebraPool.sol, 739, b"        require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');"
AlgebraFactory.sol, 110, b"    require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');"

### G07: FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED PAYABLE
#### problem
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost
#### prof
AlgebraPoolDeployer.sol, 41, b'  function setFactory(address _factory) external override onlyOwner '
AlgebraPoolDeployer.sol, 41, b'  function setFactory(address _factory) external override onlyOwner '
DataStorageOperator.sol, 50, b'  function changeFeeConfiguration(AdaptiveFee.Configuration calldata _feeConfig) external override '
AlgebraPool.sol, 260, b'  function _recalculatePosition(\n    Position storage _position,\n    int128 liquidityDelta,\n    uint256 innerFeeGrowth0Token,\n    uint256 innerFeeGrowth1Token\n  ) internal '
AlgebraPool.sol, 364, b'  function _updatePositionTicksAndFees(\n    address owner,\n    int24 bottomTick,\n    int24 topTick,\n    int128 liquidityDelta\n  )\n    private\n    returns (\n      Position storage position,\n      int256 amount0,\n      int256 amount1\n    )\n  '
AlgebraPool.sol, 364, b'  function _updatePositionTicksAndFees(\n    address owner,\n    int24 bottomTick,\n    int24 topTick,\n    int128 liquidityDelta\n  )\n    private\n    returns (\n      Position storage position,\n      int256 amount0,\n      int256 amount1\n    )\n  '
AlgebraPool.sol, 413, b'  function getOrCreatePosition(\n    address owner,\n    int24 bottomTick,\n    int24 topTick\n  ) private view returns (Position storage) '
AlgebraPool.sol, 413, b'  function getOrCreatePosition(\n    address owner,\n    int24 bottomTick,\n    int24 topTick\n  ) private view returns (Position storage) '
AlgebraPool.sol, 956, b'  function setCommunityFee(uint8 communityFee0, uint8 communityFee1) external override lock onlyFactoryOwner '
AlgebraPool.sol, 956, b'  function setCommunityFee(uint8 communityFee0, uint8 communityFee1) external override lock onlyFactoryOwner '
AlgebraPool.sol, 971, b'  function setLiquidityCooldown(uint32 newLiquidityCooldown) external override onlyFactoryOwner '
AlgebraPool.sol, 971, b'  function setLiquidityCooldown(uint32 newLiquidityCooldown) external override onlyFactoryOwner '
AlgebraFactory.sol, 81, b'  function setOwner(address _owner) external override onlyOwner '
AlgebraFactory.sol, 81, b'  function setOwner(address _owner) external override onlyOwner '
AlgebraFactory.sol, 88, b'  function setFarmingAddress(address _farmingAddress) external override onlyOwner '
AlgebraFactory.sol, 88, b'  function setFarmingAddress(address _farmingAddress) external override onlyOwner '
AlgebraFactory.sol, 95, b'  function setVaultAddress(address _vaultAddress) external override onlyOwner '
AlgebraFactory.sol, 95, b'  function setVaultAddress(address _vaultAddress) external override onlyOwner '
AlgebraFactory.sol, 114, b'  function setBaseFeeConfiguration(\n    uint16 alpha1,\n    uint16 alpha2,\n    uint32 beta1,\n    uint32 beta2,\n    uint16 gamma1,\n    uint16 gamma2,\n    uint32 volumeBeta,\n    uint16 volumeGamma,\n    uint16 baseFee\n  ) external override onlyOwner '
AlgebraFactory.sol, 114, b'  function setBaseFeeConfiguration(\n    uint16 alpha1,\n    uint16 alpha2,\n    uint32 beta1,\n    uint32 beta2,\n    uint16 gamma1,\n    uint16 gamma2,\n    uint32 volumeBeta,\n    uint16 volumeGamma,\n    uint16 baseFee\n  ) external override onlyOwner '

### G08: USING PRIVATE RATHER THAN PUBLIC FOR CONSTANTS, SAVES GAS
#### problem:
We can save getter function of public constants.
#### prof:
base/PoolImmutables.sol, 10, b'  address public immutable override dataStorageOperator;'
base/PoolImmutables.sol, 13, b'  address public immutable override factory;'
base/PoolImmutables.sol, 15, b'  address public immutable override token0;'
base/PoolImmutables.sol, 17, b'  address public immutable override token1;'
AlgebraFactory.sol, 20, b'  address public immutable override poolDeployer;'
libraries/DataStorage.sol, 12, b'  uint32 public constant WINDOW = 1 days;'

### G09: USE A MORE RECENT VERSION OF SOLIDITY


### G10: USAGE OF UINTS/INTS SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD
#### problem
When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.
#### prof
libraries/TickManager.sol, 51, b'      if (currentTick >= bottomTick) {'
libraries/TickManager.sol, 92, b'    int128 liquidityDeltaBefore = data.liquidityDelta;'
libraries/TickManager.sol, 92, b'    int128 liquidityDeltaBefore = data.liquidityDelta;'
libraries/TickManager.sol, 92, b'    int128 liquidityDeltaBefore = data.liquidityDelta;'
libraries/TickManager.sol, 93, b'    uint128 liquidityTotalBefore = data.liquidityTotal;'
libraries/TickManager.sol, 93, b'    uint128 liquidityTotalBefore = data.liquidityTotal;'
libraries/TickManager.sol, 93, b'    uint128 liquidityTotalBefore = data.liquidityTotal;'
libraries/TickManager.sol, 95, b'    uint128 liquidityTotalAfter = LiquidityMath.addDelta(liquidityTotalBefore, liquidityDelta);'
libraries/TickManager.sol, 95, b'    uint128 liquidityTotalAfter = LiquidityMath.addDelta(liquidityTotalBefore, liquidityDelta);'
libraries/TickManager.sol, 95, b'    uint128 liquidityTotalAfter = LiquidityMath.addDelta(liquidityTotalBefore, liquidityDelta);'
libraries/TickManager.sol, 95, b'    uint128 liquidityTotalAfter = LiquidityMath.addDelta(liquidityTotalBefore, liquidityDelta);'
libraries/TickManager.sol, 95, b'    uint128 liquidityTotalAfter = LiquidityMath.addDelta(liquidityTotalBefore, liquidityDelta);'
libraries/TickManager.sol, 96, b"    require(liquidityTotalAfter < Constants.MAX_LIQUIDITY_PER_TICK + 1, 'LO');"
libraries/TickManager.sol, 96, b"    require(liquidityTotalAfter < Constants.MAX_LIQUIDITY_PER_TICK + 1, 'LO');"
libraries/TickManager.sol, 96, b"    require(liquidityTotalAfter < Constants.MAX_LIQUIDITY_PER_TICK + 1, 'LO');"
libraries/TickManager.sol, 96, b"    require(liquidityTotalAfter < Constants.MAX_LIQUIDITY_PER_TICK + 1, 'LO');"
libraries/TickManager.sol, 101, b'    data.liquidityDelta = upper\n      ? int256(liquidityDeltaBefore).sub(liquidityDelta).toInt128()\n      : int256(liquidityDeltaBefore).add(liquidityDelta).toInt128();'
libraries/TickManager.sol, 103, b'    data.liquidityTotal = liquidityTotalAfter;'
libraries/TickManager.sol, 106, b'    if (liquidityTotalBefore == 0) {'
libraries/TickManager.sol, 106, b'    if (liquidityTotalBefore == 0) {'
libraries/TickManager.sol, 109, b'      if (tick <= currentTick) {'
libraries/TickManager.sol, 109, b'      if (tick <= currentTick) {'
libraries/TickManager.sol, 112, b'        data.outerSecondsPerLiquidity = secondsPerLiquidityCumulative;'
libraries/TickManager.sol, 113, b'        data.outerTickCumulative = tickCumulative;'
libraries/TickManager.sol, 114, b'        data.outerSecondsSpent = time;'
libraries/TickManager.sol, 140, b'    data.outerSecondsSpent = time - data.outerSecondsSpent;'
libraries/TickManager.sol, 141, b'    data.outerSecondsPerLiquidity = secondsPerLiquidityCumulative - data.outerSecondsPerLiquidity;'
libraries/TickManager.sol, 142, b'    data.outerTickCumulative = tickCumulative - data.outerTickCumulative;'
libraries/TickManager.sol, 147, b'    return data.liquidityDelta;'
libraries/TickManager.sol, 147, b'    return data.liquidityDelta;'
base/PoolImmutables.sol, 21, b'    return Constants.TICK_SPACING;'
base/PoolImmutables.sol, 26, b'    return Constants.MAX_LIQUIDITY_PER_TICK;'
DataStorageOperator.sol, 16, b'  uint128 constant MAX_VOLUME_PER_LIQUIDITY = 100000 << 64; // maximum meaningful ratio of volume to liquidity'
DataStorageOperator.sol, 20, b'  DataStorage.Timepoint[UINT16_MODULO] public override timepoints;'
DataStorageOperator.sol, 38, b'    return timepoints.initialize(time, tick);'
DataStorageOperator.sol, 38, b'    return timepoints.initialize(time, tick);'
DataStorageOperator.sol, 38, b'    return timepoints.initialize(time, tick);'
DataStorageOperator.sol, 38, b'    return timepoints.initialize(time, tick);'
...
