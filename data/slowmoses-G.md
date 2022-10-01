## (1) Use Pre Increment Instead of Post Increment

Pre-increment uses a little less gas.

***

for (uint256 i = 0; i < secondsAgos.length; i++) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L307

## (2) Cache Array Length

Array length should be cached instead of requesting it over and over in a for loop.
The cost per iteration depends on the array type, 100 gas for storage, 3 each for memory and calldata.
Caching the value costs only 3 gas.

***

for (uint256 i = 0; i < secondsAgos.length; i++) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L307

## (3) Do not Initialize to Default Value

Non-const variables should not be initialized to their default value.
Just using the default costs less gas.
For example use: uint256 abc;
Instead of: uint256 abc=0;

***

for (uint256 i = 0; i < secondsAgos.length; i++) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L307

## (4) Require Statements with Multiple Checks Need More Gas than Multiple Require Statements.

See link for a detailed discussion.
https://github.com/code-423n4/2022-01-xdefi-findings/issues/128

***

require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L110

require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L739

require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L743

require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L953

require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L968

require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L46

## (5) Avoid Ints or Uints Smaller Than 256 Bits

Using ints or uints smaller than 256 bits can cost extra gas since the EVM operates with 32 bytes at a time.
Consider using int256 or uint256. You can downcast them when necessary.

***

uint16 alpha1,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L99

uint16 alpha2,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L100

uint32 beta1,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L101

uint32 beta2,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L102

uint16 gamma1,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L103

uint16 gamma2,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L104

uint32 volumeBeta,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L105

uint16 volumeGamma,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L106

using TickTable for mapping(int16 => uint256);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L38

using TickManager for mapping(int24 => TickManager.Tick);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L39

uint128 liquidity; // The amount of liquidity concentrated in the range
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L42

uint32 lastLiquidityAddTimestamp; // Timestamp of last adding of liquidity
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L43

uint128 fees0; // The amount of token0 owed to a LP
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L46

uint128 fees1; // The amount of token1 owed to a LP
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L47

modifier onlyValidTicks(int24 bottomTick, int24 topTick) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L59

uint32 blockTimestamp,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L85

int56 tickCumulative,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L86

uint88 volatilityCumulative,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L88

int24 averageTick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L89

int56 tickCumulative;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L97

uint32 outerSecondsSpent;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L99

function getInnerCumulatives(int24 bottomTick, int24 topTick)
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L103

int56 innerTickCumulative,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L109

(int24 currentTick, uint16 currentTimepointIndex) = (globalState.tick, globalState.timepointIndex);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L137

uint32 globalTime = _blockTimestamp();
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L148

(int56 globalTickCumulative, uint160 globalSecondsPerLiquidityCumulative, , ) = _getSingleTimepoint(
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L149

int24 tick = TickMath.getTickAtSqrtRatio(initialPrice);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L196

uint32 timestamp = _blockTimestamp();
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L198

int128 liquidityDelta,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L217

(uint128 currentLiquidity, uint32 lastLiquidityAddTimestamp) = (_position.liquidity, _position.lastLiquidityAddTimestamp);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L221

uint32 _liquidityCooldown = liquidityCooldown;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L227

uint128 liquidityNext = LiquidityMath.addDelta(currentLiquidity, liquidityDelta);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L234

uint128 fees0;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L244

uint128 fees1;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L249

int24 tick; // The current tick
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L264

uint16 timepointIndex; // The index of the last written timepoint
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L265

int24 bottomTick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L276

int24 topTick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L277

uint32 time = _blockTimestamp();
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L296

(int56 tickCumulative, uint160 secondsPerLiquidityCumulative, , ) = _getSingleTimepoint(time, 0, cache.tick, cache.timepointIndex, liquidity);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L297

int128 globalLiquidityDelta;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L351

uint128 liquidityBefore = liquidity;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L354

uint16 newTimepointIndex = _writeTimepoint(cache.timepointIndex, _blockTimestamp(), cache.tick, liquidityBefore, volumePerLiquidityInBlock);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L355

int24 bottomTick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L367

int24 topTick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L368

int128 liquidityDelta,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L369

int24 currentTick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L370

int24 bottomTick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L405

int24 bottomTick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L419

int24 topTick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L420

uint128 liquidityDesired,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L421

uint128 liquidityForRA1 = uint128(FullMath.mulDiv(uint256(liquidityActual), receivedAmount1, amount1));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L463

int24 bottomTick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L490

int24 topTick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L491

uint128 amount0Requested,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L492

) external override lock returns (uint128 amount0, uint128 amount1) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L494

(uint128 positionFees0, uint128 positionFees1) = (position.fees0, position.fees1);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L496

int24 bottomTick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L514

int24 topTick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L515

uint32 _time,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L537

int24 _tick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L538

uint16 _index,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L539

uint16 timepointIndex,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L552

uint32 blockTimestamp,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L553

int24 tick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L554

uint128 liquidity,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L555

uint32 blockTimestamp,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L562

uint32 secondsAgo,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L563

int24 startTick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L564

uint16 timepointIndex,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L565

int56 tickCumulative,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L571

int24 currentTick;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L597

uint128 currentLiquidity;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L598

int24 currentTick;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L650

uint128 currentLiquidity;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L651

uint128 volumePerLiquidityInBlock;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L677

int56 tickCumulative; // The global tickCumulative at the moment
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L678

uint16 fee; // The current dynamic fee
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L687

int24 startTick; // The tick at the start of a swap
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L688

uint16 timepointIndex; // The index of last written timepoint
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L689

int24 nextTick; // The tick till the current step goes
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L694

int24 currentTick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L713

uint128 currentLiquidity,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L714

uint32 blockTimestamp;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L718

int128 liquidityDelta;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L835

uint128 _liquidity = liquidity;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L897

uint16 _fee = globalState.fee;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L900

uint128 constant MAX_VOLUME_PER_LIQUIDITY = 100000 << 64; // maximum meaningful ratio of volume to liquidity
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L16

function initialize(uint32 time, int24 tick) external override onlyPool {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L37

uint32 time,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L54

uint32 secondsAgo,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L55

int24 tick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L56

uint16 index,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L57

int56 tickCumulative,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L65

uint16 oldestIndex;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L71

uint16 nextIndex = index + 1; // considering overflow
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L73

uint32 time,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L89

int24 tick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L91

uint16 index,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L92

uint32 time,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L111

int24 tick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L112

uint16 index,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L113

uint16 index,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L121

uint32 blockTimestamp,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L122

int24 tick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L123

uint128 liquidity,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L124

uint128 liquidity,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L132

uint32 _time,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L151

int24 _tick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L152

uint16 _index,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L153

(uint88 volatilityAverage, uint256 volumePerLiqAverage) = timepoints.getAverages(_time, _tick, _index, _liquidity);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L156

uint16 alpha1; // max value of the first sigmoid
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L11

uint16 alpha2; // max value of the second sigmoid
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L12

uint32 beta1; // shift along the x-axis for the first sigmoid
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L13

uint32 beta2; // shift along the x-axis for the second sigmoid
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L14

uint16 gamma1; // horizontal stretch factor for the first sigmoid
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L15

uint16 gamma2; // horizontal stretch factor for the second sigmoid
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L16

uint32 volumeBeta; // shift along the x-axis for the outer volume-sigmoid
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L17

uint16 volumeGamma; // horizontal stretch factor the outer volume-sigmoid
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L18

uint16 baseFee; // minimum possible fee
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L19

uint88 volatility,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L26

uint16 g,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L46

uint16 alpha,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L47

uint16 g,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L72

uint16 internal constant BASE_FEE = 100;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/Constants.sol#L9

int24 internal constant TICK_SPACING = 60;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/Constants.sol#L10

uint128 internal constant MAX_LIQUIDITY_PER_TICK = 11505743598341114571880798222544994;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/Constants.sol#L13

uint32 internal constant MAX_LIQUIDITY_COOLDOWN = 1 days;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/Constants.sol#L15

uint32 public constant WINDOW = 1 days;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L12

uint32 blockTimestamp; // the block timestamp of the timepoint
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L16

int56 tickCumulative; // the tick accumulator, i.e. tick * time elapsed since the pool was first initialized
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L17

uint88 volatilityCumulative; // the volatility accumulator; overflow after ~34800 years is desired :)
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L19

int24 averageTick; // average tick at this blockTimestamp
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L20

uint32 blockTimestamp,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L68

int24 tick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L69

int24 prevTick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L70

uint128 liquidity,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L71

int24 averageTick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L72

uint32 delta = blockTimestamp - last.blockTimestamp;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L75

uint32 a,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L95

uint32 b,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L96

uint32 time,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L107

int24 tick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L108

uint16 index,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L109

uint16 oldestIndex,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L110

uint32 lastTimestamp,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L111

uint32 oldestTimestamp = self[oldestIndex].blockTimestamp;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L114

int56 oldestTickCumulative = self[oldestIndex].tickCumulative;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L115

uint32 time,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L150

uint32 target,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L151

uint16 lastIndex,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L152

(bool initializedBefore, uint32 timestampBefore) = (beforeOrAt.initialized, beforeOrAt.blockTimestamp);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L161

(bool initializedAfter, uint32 timestampAfter) = (atOrAfter.initialized, atOrAfter.blockTimestamp);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L166

uint32 time,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L207

uint32 secondsAgo,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L208

int24 tick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L209

uint16 index,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L210

uint16 oldestIndex,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L211

uint32 target = time - secondsAgo;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L214

int24 avgTick = int24(_getAverageTick(self, time, tick, index, oldestIndex, last.blockTimestamp, last.tickCumulative));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L223

int24 prevTick = tick;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L224

uint32 timepointTimeDelta = atOrAfter.blockTimestamp - beforeOrAt.blockTimestamp;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L247

uint32 targetDelta = target - beforeOrAt.blockTimestamp;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L248

uint32 time,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L279

int24 tick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L281

uint16 index,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L282

uint16 oldestIndex;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L299

uint16 nextIndex = index + 1; // considering overflow
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L301

uint32 time,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L328

int24 tick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L329

uint16 index,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L330

) internal view returns (uint88 volatilityAverage, uint256 volumePerLiqAverage) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L332

uint16 oldestIndex;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L333

uint16 nextIndex = index + 1; // considering overflow
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L335

uint32 oldestTimestamp = oldest.blockTimestamp;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L343

uint88 _oldestVolatilityCumulative = oldest.volatilityCumulative;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L351

uint32 time,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L366

uint16 index,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L386

uint32 blockTimestamp,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L387

int24 tick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L388

uint128 liquidity,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L389

uint16 oldestIndex;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L402

int24 avgTick = int24(_getAverageTick(self, blockTimestamp, tick, index, oldestIndex, last.blockTimestamp, last.tickCumulative));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L408

int24 prevTick = tick;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L409

uint32 _prevLastBlockTimestamp = _prevLast.blockTimestamp;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L412

int56 _prevLastTickCumulative = _prevLast.tickCumulative;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L413

uint128 liquidity,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L22

uint128 liquidity,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L38

uint128 liquidity,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L47

uint128 liquidity,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L140

uint128 liquidityTotal; // the total position liquidity that references this tick
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L18

int128 liquidityDelta; // amount of net liquidity added (subtracted) when tick is crossed left-right (right-left),
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L19

int56 outerTickCumulative; // the cumulative tick value on the other side of the tick
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L24

uint32 outerSecondsSpent; // the seconds spent on the other side of the current tick, only has relative meaning
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L26

mapping(int24 => Tick) storage self,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L40

int24 bottomTick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L41

int24 topTick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L42

int24 currentTick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L43

mapping(int24 => Tick) storage self,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L79

int24 tick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L80

int24 currentTick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L81

int128 liquidityDelta,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L82

int56 tickCumulative,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L86

uint32 time,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L87

int128 liquidityDeltaBefore = data.liquidityDelta;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L92

uint128 liquidityTotalBefore = data.liquidityTotal;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L93

uint128 liquidityTotalAfter = LiquidityMath.addDelta(liquidityTotalBefore, liquidityDelta);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L95

mapping(int24 => Tick) storage self,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L130

int24 tick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L131

int56 tickCumulative,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L135

function toggleTick(mapping(int16 => uint256) storage self, int24 tick) internal {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L14

int16 rowNumber;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L17

mapping(int16 => uint256) storage self,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L69

int24 tick,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L70

) internal view returns (int24 nextTick, bool initialized) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L72

int24 tickSpacing = Constants.TICK_SPACING;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L74

int16 rowNumber;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L83

int16 rowNumber;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L101

uint128 liquidity,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TokenDeltaMath.sol#L26

uint128 liquidity,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TokenDeltaMath.sol#L48

int24 tick; // The current tick
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol#L10

uint16 fee; // The current fee in hundredths of a bip, i.e. 1e-6
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol#L11

uint16 timepointIndex; // The index of the last written timepoint
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol#L12

uint128 public override liquidity;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol#L26

uint128 internal volumePerLiquidityInBlock;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol#L27

uint32 public override liquidityCooldown;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol#L30

mapping(int24 => TickManager.Tick) public override ticks;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol#L35

mapping(int16 => uint256) public override tickTable;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol#L37

## (6) Boolean Variables Should be Avoided

Using UINT256(1) for true and UINT256(2) for false costs less gas than using boolean variables.

***

      bool initialized,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L84

    bool toggledBottom;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L293

    bool toggledTop;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L294

    bool zeroToOne,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L591

    bool zeroToOne,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L629

    bool computedLatestTimepoint; //  if we have already fetched _tickCumulative_ and _secondPerLiquidity_ from the DataOperator
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L680

    bool exactInput; // Whether the exact input or output is specified
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L686

    bool initialized; // True if the _nextTick is initialized
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L695

    bool zeroToOne,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L704

      bool unlocked = globalState.unlocked;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L728

    bool initialized; // whether or not the timepoint is initialized
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L15

  ) private pure returns (bool res) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L98

      (bool initializedBefore, uint32 timestampBefore) = (beforeOrAt.initialized, beforeOrAt.blockTimestamp);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L161

          (bool initializedAfter, uint32 timestampAfter) = (atOrAfter.initialized, atOrAfter.blockTimestamp);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L166

    bool zeroToOne
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L24

    bool zeroToOne
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L40

    bool zeroToOne,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L49

    bool fromInput
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L50

    bool zeroToOne,
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L137

    bool initialized; // these 8 bits are set to prevent fresh sstores when crossing newly initialized ticks
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L27

    bool upper
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L88

  ) internal returns (bool flipped) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L89

    bool lte
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L71

  ) internal view returns (int24 nextTick, bool initialized) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L72

    bool roundUp
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TokenDeltaMath.sol#L27

    bool roundUp
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TokenDeltaMath.sol#L49

    bool unlocked; // True if the contract is unlocked, otherwise - false
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol#L15


