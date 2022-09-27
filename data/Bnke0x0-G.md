
### [G01] State variables only set in the constructor should be declared `immutable`

#### Impact
Avoids a Gsset (20000 gas)
#### Findings:
```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::17 => address public override owner;
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::23 => address public override farmingAddress;
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::26 => address public override vaultAddress;
2022-09-quickswap/src/core/contracts/AlgebraPoolDeployer.sol::16 => Parameters public override parameters;
2022-09-quickswap/src/core/contracts/AlgebraPoolDeployer.sol::18 => address private factory;
2022-09-quickswap/src/core/contracts/AlgebraPoolDeployer.sol::19 => address private owner;
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::20 => DataStorage.Timepoint[UINT16_MODULO] public override timepoints;
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::21 => AdaptiveFee.Configuration public feeConfig;
2022-09-quickswap/src/core/contracts/base/PoolState.sol::19 => uint256 public override totalFeeGrowth0Token;
2022-09-quickswap/src/core/contracts/base/PoolState.sol::21 => uint256 public override totalFeeGrowth1Token;
2022-09-quickswap/src/core/contracts/base/PoolState.sol::23 => GlobalState public override globalState;
2022-09-quickswap/src/core/contracts/base/PoolState.sol::26 => uint128 public override liquidity;
2022-09-quickswap/src/core/contracts/base/PoolState.sol::30 => uint32 public override liquidityCooldown;
2022-09-quickswap/src/core/contracts/base/PoolState.sol::32 => address public override activeIncentive;
```



### [G02] State variables can be packed into fewer storage slots

#### Impact
If variables occupying the same slot are both written the same 
function or by the constructor, avoids a separate Gsset (20000 gas). 
Reads of the variables are also cheaper
#### Findings:

```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::17 => address public override owner;
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::23 => address public override farmingAddress;
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::26 => address public override vaultAddress;
2022-09-quickswap/src/core/contracts/base/PoolState.sol::32 => address public override activeIncentive;
```



### [G03] `<array>.length` should not be looked up in every loop of a `for` loop

#### Impact
Even memory arrays incur the overhead of bit tests and bit shifts to 
calculate the array length. Storage array length checks incur an extra 
Gwarmaccess (100 gas) PER-LOOP.
#### Findings:

```
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::307 => for (uint256 i = 0; i < secondsAgos.length; i++) {
```



### [G04] `++i/i++` should be `unchecked{++i}`/`unchecked{++i}` when it is not possible for them to overflow, as is the case when used in `for` and `while` loops


#### Findings:
```
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::307 => for (uint256 i = 0; i < secondsAgos.length; i++) {
```



### [G05] Using `> 0` costs more gas than `!= 0` when used on a `uint` in a `require()` statement


#### Findings:
```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::110 => require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::224 => require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::434 => require(liquidityDesired > 0, 'IL');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::469 => require(liquidityActual > 0, 'IIL2');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::641 => require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::645 => require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::898 => require(_liquidity > 0, 'L');
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::46 => require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::52 => require(price > 0);
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::53 => require(liquidity > 0);
```



### [G06] It costs more gas to initialize variables to zero than to let the default of zero be applied


#### Findings:
```
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::359 => volumePerLiquidityInBlock = 0;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::774 => cache.volumePerLiquidityInBlock = 0;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::307 => for (uint256 i = 0; i < secondsAgos.length; i++) {
```



### [G07] `++i` costs less gas than `i++`, especially when it’s used in forloops (`--i`/`i--` too)


#### Findings:
```
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::307 => for (uint256 i = 0; i < secondsAgos.length; i++) {
```



### [G08] Splitting `require()` statements that use `&&` Cost gas


#### Findings:
```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::110 => require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::739 => require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::743 => require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::953 => require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::968 => require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::46 => require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
```



### [G09] Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead

#### Impact
> When using elements that are smaller than 32 bytes, your 
contract’s gas usage may be higher. This is because the EVM operates on 
32 bytes at a time. Therefore, if the element is smaller than that, the 
EVM must use more operations in order to reduce the size of the element 
from 32 bytes to the desired size.
> 

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html)
Use a larger size then downcast where needed
#### Findings:
```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::99 => uint16 alpha1,
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::100 => uint16 alpha2,
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::101 => uint32 beta1,
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::102 => uint32 beta2,
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::103 => uint16 gamma1,
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::104 => uint16 gamma2,
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::105 => uint32 volumeBeta,
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::106 => uint16 volumeGamma,
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::107 => uint16 baseFee
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::42 => uint128 liquidity; // The amount of liquidity concentrated in the range
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::43 => uint32 lastLiquidityAddTimestamp; // Timestamp of last adding of liquidity
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::46 => uint128 fees0; // The amount of token0 owed to a LP
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::47 => uint128 fees1; // The amount of token1 owed to a LP
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::85 => uint32 blockTimestamp,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::87 => uint160 secondsPerLiquidityCumulative,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::88 => uint88 volatilityCumulative,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::90 => uint144 volumePerLiquidityCumulative
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::98 => uint160 outerSecondPerLiquidity;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::99 => uint32 outerSecondsSpent;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::110 => uint160 innerSecondsSpentPerLiquidity,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::111 => uint32 innerSecondsSpent
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::137 => (int24 currentTick, uint16 currentTimepointIndex) = (globalState.tick, globalState.timepointIndex);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::148 => uint32 globalTime = _blockTimestamp();
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::149 => (int56 globalTickCumulative, uint160 globalSecondsPerLiquidityCumulative, , ) = _getSingleTimepoint(
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::171 => function getTimepoints(uint32[] calldata secondsAgos)
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::177 => uint160[] memory secondsPerLiquidityCumulatives,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::178 => uint112[] memory volatilityCumulatives,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::193 => function initialize(uint160 initialPrice) external override {
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::198 => uint32 timestamp = _blockTimestamp();
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::221 => (uint128 currentLiquidity, uint32 lastLiquidityAddTimestamp) = (_position.liquidity, _position.lastLiquidityAddTimestamp);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::227 => uint32 _liquidityCooldown = liquidityCooldown;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::234 => uint128 liquidityNext = LiquidityMath.addDelta(currentLiquidity, liquidityDelta);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::244 => uint128 fees0;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::247 => fees0 = uint128(FullMath.mulDiv(innerFeeGrowth0Token - _innerFeeGrowth0Token, currentLiquidity, Constants.Q128));
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::249 => uint128 fees1;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::252 => fees1 = uint128(FullMath.mulDiv(innerFeeGrowth1Token - _innerFeeGrowth1Token, currentLiquidity, Constants.Q128));
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::263 => uint160 price; // The square root of the current price in Q64.96 format
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::265 => uint16 timepointIndex; // The index of the last written timepoint
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::296 => uint32 time = _blockTimestamp();
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::297 => (int56 tickCumulative, uint160 secondsPerLiquidityCumulative, , ) = _getSingleTimepoint(time, 0, cache.tick, cache.timepointIndex, liquidity);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::354 => uint128 liquidityBefore = liquidity;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::355 => uint16 newTimepointIndex = _writeTimepoint(cache.timepointIndex, _blockTimestamp(), cache.tick, liquidityBefore, volumePerLiquidityInBlock);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::371 => uint160 currentPrice
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::421 => uint128 liquidityDesired,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::431 => uint128 liquidityActual
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::492 => uint128 amount0Requested,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::493 => uint128 amount1Requested
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::494 => ) external override lock returns (uint128 amount0, uint128 amount1) {
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::496 => (uint128 positionFees0, uint128 positionFees1) = (position.fees0, position.fees1);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::516 => uint128 amount
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::529 => (position.fees0, position.fees1) = (position.fees0.add128(uint128(amount0)), position.fees1.add128(uint128(amount1)));
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::537 => uint32 _time,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::539 => uint16 _index,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::540 => uint128 _liquidity
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::541 => ) private returns (uint16 newFee) {
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::552 => uint16 timepointIndex,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::553 => uint32 blockTimestamp,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::555 => uint128 liquidity,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::556 => uint128 volumePerLiquidityInBlock
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::557 => ) private returns (uint16 newTimepointIndex) {
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::562 => uint32 blockTimestamp,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::563 => uint32 secondsAgo,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::565 => uint16 timepointIndex,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::566 => uint128 liquidityStart
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::572 => uint160 secondsPerLiquidityCumulative,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::573 => uint112 volatilityCumulative,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::593 => uint160 limitSqrtPrice,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::596 => uint160 currentPrice;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::598 => uint128 currentLiquidity;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::631 => uint160 limitSqrtPrice,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::649 => uint160 currentPrice;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::651 => uint128 currentLiquidity;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::677 => uint128 volumePerLiquidityInBlock;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::679 => uint160 secondsPerLiquidityCumulative; // The global secondPerLiquidity at the moment
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::687 => uint16 fee; // The current dynamic fee
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::689 => uint16 timepointIndex; // The index of last written timepoint
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::693 => uint160 stepSqrtPrice; // The Q64.96 sqrt of the price at the start of the step
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::696 => uint160 nextTickPrice; // The Q64.96 sqrt of the price calculated from the _nextTick
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::706 => uint160 limitSqrtPrice
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::712 => uint160 currentPrice,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::714 => uint128 currentLiquidity,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::718 => uint32 blockTimestamp;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::763 => uint16 newTimepointIndex = _writeTimepoint(
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::897 => uint128 _liquidity = liquidity;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::900 => uint16 _fee = globalState.fee;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::925 => uint8 _communityFeeToken0 = globalState.communityFeeToken0;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::939 => uint8 _communityFeeToken1 = globalState.communityFeeToken1;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::952 => function setCommunityFee(uint8 communityFee0, uint8 communityFee1) external override lock onlyFactoryOwner {
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::967 => function setLiquidityCooldown(uint32 newLiquidityCooldown) external override onlyFactoryOwner {
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::16 => uint128 constant MAX_VOLUME_PER_LIQUIDITY = 100000 << 64; // maximum meaningful ratio of volume to liquidity
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::37 => function initialize(uint32 time, int24 tick) external override onlyPool {
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::54 => uint32 time,
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::55 => uint32 secondsAgo,
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::57 => uint16 index,
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::58 => uint128 liquidity
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::66 => uint160 secondsPerLiquidityCumulative,
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::67 => uint112 volatilityCumulative,
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::71 => uint16 oldestIndex;
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::73 => uint16 nextIndex = index + 1; // considering overflow
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::89 => uint32 time,
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::90 => uint32[] memory secondsAgos,
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::92 => uint16 index,
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::93 => uint128 liquidity
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::101 => uint160[] memory secondsPerLiquidityCumulatives,
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::102 => uint112[] memory volatilityCumulatives,
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::111 => uint32 time,
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::113 => uint16 index,
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::114 => uint128 liquidity
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::121 => uint16 index,
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::122 => uint32 blockTimestamp,
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::124 => uint128 liquidity,
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::125 => uint128 volumePerLiquidity
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::126 => ) external override onlyPool returns (uint16 indexUpdated) {
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::132 => uint128 liquidity,
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::135 => ) external pure override returns (uint128 volumePerLiquidity) {
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::141 => else return uint128(volumeShifted);
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::145 => function window() external pure override returns (uint32) {
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::151 => uint32 _time,
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::153 => uint16 _index,
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::154 => uint128 _liquidity
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::155 => ) external view override onlyPool returns (uint16 fee) {
2022-09-quickswap/src/core/contracts/base/PoolImmutables.sol::25 => function maxLiquidityPerTick() external pure override returns (uint128) {
2022-09-quickswap/src/core/contracts/base/PoolState.sol::9 => uint160 price; // The square root of the current price in Q64.96 format
2022-09-quickswap/src/core/contracts/base/PoolState.sol::11 => uint16 fee; // The current fee in hundredths of a bip, i.e. 1e-6
2022-09-quickswap/src/core/contracts/base/PoolState.sol::12 => uint16 timepointIndex; // The index of the last written timepoint
2022-09-quickswap/src/core/contracts/base/PoolState.sol::13 => uint8 communityFeeToken0; // The community fee represented as a percent of all collected fee in thousandths (1e-3)
2022-09-quickswap/src/core/contracts/base/PoolState.sol::14 => uint8 communityFeeToken1;
2022-09-quickswap/src/core/contracts/base/PoolState.sol::26 => uint128 public override liquidity;
2022-09-quickswap/src/core/contracts/base/PoolState.sol::27 => uint128 internal volumePerLiquidityInBlock;
2022-09-quickswap/src/core/contracts/base/PoolState.sol::30 => uint32 public override liquidityCooldown;
2022-09-quickswap/src/core/contracts/base/PoolState.sol::48 => /// @return A timestamp converted to uint32
2022-09-quickswap/src/core/contracts/base/PoolState.sol::49 => function _blockTimestamp() internal view virtual returns (uint32) {
2022-09-quickswap/src/core/contracts/base/PoolState.sol::50 => return uint32(block.timestamp); // truncation is desired
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::9 => // alpha1 + alpha2 + baseFee must be <= type(uint16).max
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::11 => uint16 alpha1; // max value of the first sigmoid
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::12 => uint16 alpha2; // max value of the second sigmoid
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::13 => uint32 beta1; // shift along the x-axis for the first sigmoid
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::14 => uint32 beta2; // shift along the x-axis for the second sigmoid
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::15 => uint16 gamma1; // horizontal stretch factor for the first sigmoid
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::16 => uint16 gamma2; // horizontal stretch factor for the second sigmoid
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::17 => uint32 volumeBeta; // shift along the x-axis for the outer volume-sigmoid
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::18 => uint16 volumeGamma; // horizontal stretch factor the outer volume-sigmoid
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::19 => uint16 baseFee; // minimum possible fee
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::26 => uint88 volatility,
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::29 => ) internal pure returns (uint16 fee) {
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::33 => if (sumOfSigmoids > type(uint16).max) {
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::35 => sumOfSigmoids = type(uint16).max;
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::38 => return uint16(config.baseFee + sigmoid(volumePerLiquidity, config.volumeGamma, uint16(sumOfSigmoids), config.volumeBeta)); // safe since alpha1 + alpha2 + baseFee _must_ be <= type(uint16).max
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::46 => uint16 g,
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::47 => uint16 alpha,
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::72 => uint16 g,
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::5 => uint8 internal constant RESOLUTION = 96;
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::9 => uint16 internal constant BASE_FEE = 100;
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::12 => // max(uint128) / ( (MAX_TICK - MIN_TICK) / TICK_SPACING )
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::13 => uint128 internal constant MAX_LIQUIDITY_PER_TICK = 11505743598341114571880798222544994;
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::15 => uint32 internal constant MAX_LIQUIDITY_COOLDOWN = 1 days;
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::16 => uint8 internal constant MAX_COMMUNITY_FEE = 250;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::12 => uint32 public constant WINDOW = 1 days;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::16 => uint32 blockTimestamp; // the block timestamp of the timepoint
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::18 => uint160 secondsPerLiquidityCumulative; // the seconds per liquidity since the pool was first initialized
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::19 => uint88 volatilityCumulative; // the volatility accumulator; overflow after ~34800 years is desired :)
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::21 => uint144 volumePerLiquidityCumulative; // the gmean(volumes)/liquidity accumulator
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::25 => /// @param dt Timedelta between timepoints, must be within uint32 range
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::68 => uint32 blockTimestamp,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::71 => uint128 liquidity,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::73 => uint128 volumePerLiquidity
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::75 => uint32 delta = blockTimestamp - last.blockTimestamp;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::80 => last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::81 => last.volatilityCumulative += uint88(_volatilityOnRange(delta, prevTick, tick, last.averageTick, averageTick)); // always fits 88 bits
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::95 => uint32 a,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::96 => uint32 b,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::97 => uint32 currentTime
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::107 => uint32 time,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::109 => uint16 index,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::110 => uint16 oldestIndex,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::111 => uint32 lastTimestamp,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::114 => uint32 oldestTimestamp = self[oldestIndex].blockTimestamp;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::150 => uint32 time,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::151 => uint32 target,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::152 => uint16 lastIndex,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::153 => uint16 oldestIndex
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::160 => beforeOrAt = self[uint16(current)]; // checking the "middle" point between the boundaries
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::161 => (bool initializedBefore, uint32 timestampBefore) = (beforeOrAt.initialized, beforeOrAt.blockTimestamp);
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::165 => atOrAfter = self[uint16(current + 1)]; // checking the next point after "middle"
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::166 => (bool initializedAfter, uint32 timestampAfter) = (atOrAfter.initialized, atOrAfter.blockTimestamp);
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::207 => uint32 time,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::208 => uint32 secondsAgo,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::210 => uint16 index,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::211 => uint16 oldestIndex,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::212 => uint128 liquidity
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::214 => uint32 target = time - secondsAgo;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::247 => uint32 timepointTimeDelta = atOrAfter.blockTimestamp - beforeOrAt.blockTimestamp;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::248 => uint32 targetDelta = target - beforeOrAt.blockTimestamp;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::252 => beforeOrAt.secondsPerLiquidityCumulative += uint160(
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::279 => uint32 time,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::280 => uint32[] memory secondsAgos,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::282 => uint16 index,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::283 => uint128 liquidity
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::289 => uint160[] memory secondsPerLiquidityCumulatives,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::290 => uint112[] memory volatilityCumulatives,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::295 => secondsPerLiquidityCumulatives = new uint160[](secondsAgos.length);
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::296 => volatilityCumulatives = new uint112[](secondsAgos.length);
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::299 => uint16 oldestIndex;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::301 => uint16 nextIndex = index + 1; // considering overflow
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::328 => uint32 time,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::330 => uint16 index,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::331 => uint128 liquidity
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::333 => uint16 oldestIndex;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::335 => uint16 nextIndex = index + 1; // considering overflow
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::343 => uint32 oldestTimestamp = oldest.blockTimestamp;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::351 => uint88 _oldestVolatilityCumulative = oldest.volatilityCumulative;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::352 => uint144 _oldestVolumePerLiquidityCumulative = oldest.volumePerLiquidityCumulative;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::362 => /// @param time The time of the dataStorage initialization, via block.timestamp truncated to uint32
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::366 => uint32 time,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::386 => uint16 index,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::387 => uint32 blockTimestamp,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::389 => uint128 liquidity,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::390 => uint128 volumePerLiquidity
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::391 => ) internal returns (uint16 indexUpdated) {
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::402 => uint16 oldestIndex;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::412 => uint32 _prevLastBlockTimestamp = _prevLast.blockTimestamp;
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::21 => uint160 price,
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::22 => uint128 liquidity,
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::25 => ) internal pure returns (uint160 resultPrice) {
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::37 => uint160 price,
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::38 => uint128 liquidity,
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::41 => ) internal pure returns (uint160 resultPrice) {
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::46 => uint160 price,
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::47 => uint128 liquidity,
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::51 => ) internal pure returns (uint160 resultPrice) {
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::64 => if (denominator >= liquidityShifted) return uint160(FullMath.mulDivRoundingUp(liquidityShifted, price, denominator)); // always fits in 160 bits
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::67 => return uint160(FullMath.divRoundingUp(liquidityShifted, (liquidityShifted / price).add(amount)));
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::80 => .add(amount <= type(uint160).max ? (amount << Constants.RESOLUTION) / liquidity : FullMath.mulDiv(amount, Constants.Q96, liquidity))
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::88 => return uint160(price - quotient); // always fits 160 bits
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::94 => uint160 to,
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::95 => uint160 from,
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::96 => uint128 liquidity
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::102 => uint160 to,
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::103 => uint160 from,
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::104 => uint128 liquidity
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::110 => uint160 to,
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::111 => uint160 from,
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::112 => uint128 liquidity
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::118 => uint160 to,
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::119 => uint160 from,
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::120 => uint128 liquidity
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::138 => uint160 currentPrice,
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::139 => uint160 targetPrice,
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::140 => uint128 liquidity,
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::142 => uint16 fee
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::147 => uint160 resultPrice,
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::18 => uint128 liquidityTotal; // the total position liquidity that references this tick
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::25 => uint160 outerSecondsPerLiquidity; // the seconds per unit of liquidity on the _other_ side of current tick, (relative meaning)
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::26 => uint32 outerSecondsSpent; // the seconds spent on the other side of the current tick, only has relative meaning
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::85 => uint160 secondsPerLiquidityCumulative,
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::87 => uint32 time,
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::93 => uint128 liquidityTotalBefore = data.liquidityTotal;
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::95 => uint128 liquidityTotalAfter = LiquidityMath.addDelta(liquidityTotalBefore, liquidityDelta);
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::134 => uint160 secondsPerLiquidityCumulative,
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::136 => uint32 time
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::18 => uint8 bitNumber;
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::84 => uint8 bitNumber;
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::102 => uint8 bitNumber;
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::24 => uint160 priceLower,
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::25 => uint160 priceUpper,
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::26 => uint128 liquidity,
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::46 => uint160 priceLower,
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::47 => uint160 priceUpper,
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::48 => uint128 liquidity,
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::62 => uint160 priceLower,
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::63 => uint160 priceUpper,
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::67 => ? getToken0Delta(priceLower, priceUpper, uint128(liquidity), true).toInt256()
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::68 => : -getToken0Delta(priceLower, priceUpper, uint128(-liquidity), false).toInt256();
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::77 => uint160 priceLower,
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::78 => uint160 priceUpper,
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::82 => ? getToken1Delta(priceLower, priceUpper, uint128(liquidity), true).toInt256()
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::83 => : -getToken1Delta(priceLower, priceUpper, uint128(-liquidity), false).toInt256();
```


### [G10] Duplicated `require()`/`revert()` checks should be refactored to a modifier or function


#### Findings:
```
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::608 => require(balance0Before.add(uint256(amount0)) <= balanceToken0(), 'IIA');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::614 => require(balance1Before.add(uint256(amount1)) <= balanceToken1(), 'IIA');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::641 => require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::645 => require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::739 => require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::743 => require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
```



### [G11] `require()` or `revert()` statements that check input arguments should be at the top of the function

#### Impact
Checks that involve constants should come before checks that involve state variables
#### Findings:
```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::62 => require(token0 != address(0));
2022-09-quickswap/src/core/contracts/AlgebraPoolDeployer.sol::37 => require(_factory != address(0));
2022-09-quickswap/src/core/contracts/AlgebraPoolDeployer.sol::38 => require(factory == address(0));
```

### [G12] Functions guaranteed to revert when called by normal users can be marked `payable`

#### Impact
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.
#### Findings:
```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::77 => function setOwner(address _owner) external override onlyOwner {
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::84 => function setFarmingAddress(address _farmingAddress) external override onlyOwner {
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::91 => function setVaultAddress(address _vaultAddress) external override onlyOwner {
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::952 => function setCommunityFee(uint8 communityFee0, uint8 communityFee1) external override lock onlyFactoryOwner {
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::967 => function setLiquidityCooldown(uint32 newLiquidityCooldown) external override onlyFactoryOwner {
2022-09-quickswap/src/core/contracts/AlgebraPoolDeployer.sol::36 => function setFactory(address _factory) external override onlyOwner {
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::37 => function initialize(uint32 time, int24 tick) external override onlyPool {
```



### [G13] Use a more recent version of solidity

#### Impact
Use a solidity version of at least 0.8.10 to have external calls skip
 contract existence checks if the external call has a return value
#### Findings:
```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/AlgebraPoolDeployer.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/base/PoolImmutables.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/base/PoolState.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::2 => pragma solidity =0.7.6;
```



### [G14] Bitshift for divide by 2

#### Impact
When multiply or dividing by a power of two, it is cheaper to bitshift than to use standard math operations.

There is a divide by 2 operation on this line
#### Findings:
```
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::88 => res += (xLowestDegree * gHighestDegree) / 2;
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::96 => res += (xLowestDegree * gHighestDegree) / 24;
```



### [G15] Amounts should be checked for 0 before calling a transfer


#### Findings:
```
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::479 => TransferHelper.safeTransfer(token0, sender, receivedAmount0 - amount0);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::482 => TransferHelper.safeTransfer(token1, sender, receivedAmount1 - amount1);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::505 => if (amount0 > 0) TransferHelper.safeTransfer(token0, recipient, amount0);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::506 => if (amount1 > 0) TransferHelper.safeTransfer(token1, recipient, amount1);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::548 => TransferHelper.safeTransfer(token, vault, amount);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::604 => if (amount1 < 0) TransferHelper.safeTransfer(token1, recipient, uint256(-amount1)); // transfer to recipient
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::610 => if (amount0 < 0) TransferHelper.safeTransfer(token0, recipient, uint256(-amount0)); // transfer to recipient
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::658 => if (amount1 < 0) TransferHelper.safeTransfer(token1, recipient, uint256(-amount1));
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::660 => if (amount0 < amountRequired) TransferHelper.safeTransfer(token0, sender, uint256(amountRequired.sub(amount0)));
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::662 => if (amount0 < 0) TransferHelper.safeTransfer(token0, recipient, uint256(-amount0));
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::664 => if (amount1 < amountRequired) TransferHelper.safeTransfer(token1, sender, uint256(amountRequired.sub(amount1)));
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::906 => TransferHelper.safeTransfer(token0, recipient, amount0);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::913 => TransferHelper.safeTransfer(token1, recipient, amount1);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::929 => TransferHelper.safeTransfer(token0, vault, fees0);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::943 => TransferHelper.safeTransfer(token1, vault, fees1);
```





### [G16] Multiple `address` mappings can be combined into a single `mapping` of an `address` to a `struct`, where appropriate

#### Impact
Saves a storage slot for the mapping. Depending on the circumstances 
and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. 
Reads and subsequent writes can also be cheaper when a function requires
 both values and they both fit in the same storage slot
#### Findings:
```
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::40 => mapping(int24 => Tick) storage self,
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::79 => mapping(int24 => Tick) storage self,
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::130 => mapping(int24 => Tick) storage self,
```






### [G17] Using `bools` for storage incurs overhead


#### Findings:
```
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::84 => bool initialized,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::293 => bool toggledBottom;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::294 => bool toggledTop;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::591 => bool zeroToOne,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::629 => bool zeroToOne,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::680 => bool computedLatestTimepoint; //  if we have already fetched _tickCumulative_ and _secondPerLiquidity_ from the DataOperator
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::686 => bool exactInput; // Whether the exact input or output is specified
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::695 => bool initialized; // True if the _nextTick is initialized
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::704 => bool zeroToOne,
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::728 => bool unlocked = globalState.unlocked;
2022-09-quickswap/src/core/contracts/base/PoolState.sol::15 => bool unlocked; // True if the contract is unlocked, otherwise - false
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::15 => bool initialized; // whether or not the timepoint is initialized
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::27 => bool initialized; // these 8 bits are set to prevent fresh sstores when crossing newly initialized ticks
```





### [G18] Using `private` rather than `public` for constants, saves gas

#### Impact
If needed, the value can be read from the verified contract source 
code. Savings are due to the compiler not having to create non-payable 
getter functions for deployment calldata, and not adding another entry 
to the method ID table
#### Findings:
```
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::12 => uint32 public constant WINDOW = 1 days;
```



### [G19] Usage of `assert()` instead of `require()`

#### Impact
Between solc 0.4.10 and 0.8.0, require() used REVERT (0xfd) opcode which refunded remaining gas on failure while assert() used INVALID (0xfe) opcode which consumed all the supplied gas. (see https://docs.soliditylang.org/en/v0.8.1/control-structures.html#error-handling-assert-require-revert-and-exceptions).require() should be used for checking error conditions on inputs and return values while assert() should be used for invariant checking (properly functioning code should never reach a failing assert statement, unless there is a bug in your contract you should fix).
From the current usage of assert, my guess is that they can be replaced with require, unless a Panic really is intended.
#### Findings:
```
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::190 => assert(false);
```



### [G20] Remove or replace unused state variables

#### Impact
Saves a storage slot. If the variable is assigned a non-zero value, 
saves Gsset (20000 gas). If it’s assigned a zero value, saves Gsreset 
(2900 gas). If the variable remains unassigned, there is no gas savings 
unless the variable is public, in which case the 
compiler-generated non-payable getter deployment cost is saved. If the 
state variable is overriding an interface’s public function, mark the 
variable as constant or immutable so that it does not use a storage slot
#### Findings:
```

2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::90 => uint32[] memory secondsAgos,
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::101 => uint160[] memory secondsPerLiquidityCumulatives,
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::102 => uint112[] memory volatilityCumulatives,
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::103 => uint256[] memory volumePerAvgLiquiditys
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::280 => uint32[] memory secondsAgos,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::289 => uint160[] memory secondsPerLiquidityCumulatives,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::290 => uint112[] memory volatilityCumulatives,
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::291 => uint256[] memory volumePerAvgLiquiditys
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::295 => secondsPerLiquidityCumulatives = new uint160[](secondsAgos.length);
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::296 => volatilityCumulatives = new uint112[](secondsAgos.length);
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::297 => volumePerAvgLiquiditys = new uint256[](secondsAgos.length);
```



### [G21] `abi.encode()` is less efficient than abi.encodePacked()


#### Findings:
```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::123 => pool = address(uint256(keccak256(abi.encodePacked(hex'ff', poolDeployer, keccak256(abi.encode(token0, token1)), POOL_INIT_CODE_HASH))));
2022-09-quickswap/src/core/contracts/AlgebraPoolDeployer.sol::51 => pool = address(new AlgebraPool{salt: keccak256(abi.encode(token0, token1))}());
```



### [G22] Optimize names to save gas

#### Impact
public/external function names and public member variable names can be optimized to save gas. See this
 link for an example of how it works. Below are the interfaces/abstract 
contracts that can be optimized so that the most frequently-called 
functions use the least amount of gas possible during method lookup. 
Method IDs that have two leading zero bytes can save 128 gas each during deployment, and renaming functions to have lower method IDs will save 22 gas per call, per sorted position shifted
#### Findings:
```
2022-09-quickswap/src/core/contracts/base/PoolImmutables.sol::8 => abstract contract PoolImmutables is IAlgebraPoolImmutables {
2022-09-quickswap/src/core/contracts/base/PoolState.sol::7 => abstract contract PoolState is IAlgebraPoolState {
```




