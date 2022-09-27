Total Gas Optimizations: 15

## (1) Abi.encode() Is Less Efficient Than Abi.encodepacked()

Severity: Gas Optimizations

## Proof Of Concept


	pool = address(uint256(keccak256(abi.encodePacked(hex'ff', poolDeployer, keccak256(abi.encode(token0, token1)), POOL_INIT_CODE_HASH))));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L123

	pool = address(new AlgebraPool{salt: keccak256(abi.encode(token0, token1))}());
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol#L51





## (2) <Array>.length Should Not Be Looked Up In Every Loop Of A For-loop

Severity: Gas Optimizations

The overheads outlined below are PER LOOP, excluding the first loop

storage arrays incur a Gwarmaccess (100 gas)
memory arrays use MLOAD (3 gas)
calldata arrays use CALLDATALOAD (3 gas)

Caching the length changes each of these to a DUP<N> (3 gas), and gets rid of the extra DUP<N> needed to store the stack offset

## Proof Of Concept

	for (uint256 i = 0; i < secondsAgos.length; i++) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L307






## (3) Using Calldata Instead Of Memory For Read-only Arguments In External Functions Saves Gas

Severity: Gas Optimizations

When a function with a memory array is called externally, the abi.decode() step has to use a for-loop to copy each index of the calldata to the memory index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 * <mem_array>.length). Using calldata directly, obliviates the need for such a loop in the contract code and runtime execution. Structs have the same overhead as an array of length one


## Proof Of Concept

  function getTimepoints(
    uint32 time,
    uint32[] memory secondsAgos,
    int24 tick,
    uint16 index,
    uint128 liquidity
  )
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L88

	function getTimepoints(
    Timepoint[UINT16_MODULO] storage self,
    uint32 time,
    uint32[] memory secondsAgos,
    int24 tick,
    uint16 index,
    uint128 liquidity
  )
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L277






## (4) ++i Costs Less Gas Than i++, Especially When It’s Used In For-loops (--i/i-- Too)

Severity: Gas Optimizations

Saves 6 gas per loop

## Proof Of Concept


	for (uint256 i = 0; i < secondsAgos.length; i++) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L307



## Recommended Mitigation Steps

For example, use ++i instead of i++



## (5) It Costs More Gas To Initialize Variables To Zero Than To Let The Default Of Zero Be Applied

Severity: Gas Optimizations

## Proof Of Concept


	for (uint256 i = 0; i < secondsAgos.length; i++) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L307






## (6) Using > 0 Costs More Gas Than != 0 When Used On A Uint In A Require() Statement

Severity: Gas Optimizations

This change saves 6 gas per instance

## Proof Of Concept

	require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L224

	require(liquidityDesired > 0, 'IL');
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L434

	if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L454

	if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L455

	require(liquidityActual > 0, 'IIL2');
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L469

	require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L641

	require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L645

	require(_liquidity > 0, 'L');
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L898

	require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L46

	require(price > 0);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L52

	require(liquidity > 0);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L53





## (7) Multiplication/division By Two Should Use Bit Shifting

Severity: Gas Optimizations

<x> * 2 is equivalent to <x> << 1 and <x> / 2 is the same as <x> >> 1. The MUL and DIV opcodes cost 5 gas, whereas SHL and SHR only cost 3 gas

## Proof Of Concept

	res += (xLowestDegree * gHighestDegree) / 2;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L88






## (8) Not Using The Named Return Variables When A Function Returns, Wastes Deployment Gas

Severity: Gas Optimizations

## Proof Of Concept

	function _writeTimepoint(
    uint16 timepointIndex,
    uint32 blockTimestamp,
    int24 tick,
    uint128 liquidity,
    uint128 volumePerLiquidityInBlock
  ) private returns (uint16 newTimepointIndex) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L551

	function getAverages(
    uint32 time,
    int24 tick,
    uint16 index,
    uint128 liquidity
  ) external view override onlyPool returns (uint112 TWVolatilityAverage, uint256 TWVolumePerLiqAverage) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L110

	function write(
    uint16 index,
    uint32 blockTimestamp,
    int24 tick,
    uint128 liquidity,
    uint128 volumePerLiquidity
  ) external override onlyPool returns (uint16 indexUpdated) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L120

	function calculateVolumePerLiquidity(
    uint128 liquidity,
    int256 amount0,
    int256 amount1
  ) external pure override returns (uint128 volumePerLiquidity) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L131

	function getFee(
    uint32 _time,
    int24 _tick,
    uint16 _index,
    uint128 _liquidity
  ) external view override onlyPool returns (uint16 fee) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L150

	function getFee(
    uint88 volatility,
    uint256 volumePerLiquidity,
    Configuration memory config
  ) internal pure returns (uint16 fee) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L25

	function getSingleTimepoint(
    Timepoint[UINT16_MODULO] storage self,
    uint32 time,
    uint32 secondsAgo,
    int24 tick,
    uint16 index,
    uint16 oldestIndex,
    uint128 liquidity
  ) internal view returns (Timepoint memory targetTimepoint) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L205

	function getAverages(
    Timepoint[UINT16_MODULO] storage self,
    uint32 time,
    int24 tick,
    uint16 index,
    uint128 liquidity
  ) internal view returns (uint88 volatilityAverage, uint256 volumePerLiqAverage) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L326

	function getNewPriceAfterOutput(
    uint160 price,
    uint128 liquidity,
    uint256 output,
    bool zeroToOne
  ) internal pure returns (uint160 resultPrice) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L36

	function getNewPrice(
    uint160 price,
    uint128 liquidity,
    uint256 amount,
    bool zeroToOne,
    bool fromInput
  ) internal pure returns (uint160 resultPrice) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L45

	function cross(
    mapping(int24 => Tick) storage self,
    int24 tick,
    uint256 totalFeeGrowth0Token,
    uint256 totalFeeGrowth1Token,
    uint160 secondsPerLiquidityCumulative,
    int56 tickCumulative,
    uint32 time
  ) internal returns (int128 liquidityDelta) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L129


## (9) <x> += <y> Costs More Gas Than <x> = <x> + <y> For State Variables

Severity: Gas Optimizations

## Proof Of Concept


	_position.fees0 += fees0;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L257

	_position.fees1 += fees1;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L258

	amountRequired -= (step.input + step.feeAmount).toInt256(); // decrease remaining input amount
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L801

	amountRequired += step.output.toInt256(); // increase remaining output amount (since its negative)
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L804

	step.feeAmount -= delta;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L810

	communityFeeAmount += delta;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L811

	if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L814

	paid0 -= balance0Before;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L922

	totalFeeGrowth0Token += FullMath.mulDiv(paid0 - fees0, Constants.Q128, _liquidity);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L931

	paid1 -= balance1Before;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L936

	totalFeeGrowth1Token += FullMath.mulDiv(paid1 - fees1, Constants.Q128, _liquidity);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L945

	res += xLowestDegree * gHighestDegree;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L84

	res += (xLowestDegree * gHighestDegree) / 2;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L88

	res += (xLowestDegree * gHighestDegree) / 6;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L92

	res += (xLowestDegree * gHighestDegree) / 24;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L96

	res += (xLowestDegree * gHighestDegree) / 120;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L100

	res += (xLowestDegree * gHighestDegree) / 720;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L104

	res += (xLowestDegree * g) / 5040 + (xLowestDegree * x) / (40320);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L107

	last.tickCumulative += int56(tick) * delta;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L79

	last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L80

	last.volatilityCumulative += uint88(_volatilityOnRange(delta, prevTick, tick, last.averageTick, averageTick)); // always fits 88 bits
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L81

	last.volumePerLiquidityCumulative += volumePerLiquidity;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L83

	index -= 1; // considering underflow
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L119

	beforeOrAt.tickCumulative += ((atOrAfter.tickCumulative - beforeOrAt.tickCumulative) / timepointTimeDelta) * targetDelta;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L251

	beforeOrAt.secondsPerLiquidityCumulative += uint160(
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L252

	beforeOrAt.volatilityCumulative += ((atOrAfter.volatilityCumulative - beforeOrAt.volatilityCumulative) / timepointTimeDelta) * targetDelta;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L255

	beforeOrAt.volumePerLiquidityCumulative +=
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L256

	innerFeeGrowth0Token -= upper.outerFeeGrowth0Token;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L58

	innerFeeGrowth1Token -= upper.outerFeeGrowth1Token;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L59

	tick -= int24(255 - getMostSignificantBit(_row));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L92

	tick -= int24(bitNumber);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L95

	tick += 1;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L100

	tick += int24(getSingleSignificantBit(-_row & _row)); // least significant bit
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L112

	tick += int24(255 - bitNumber);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L115





## (10) Using Private Rather Than Public For Constants, Saves Gas

Severity: Gas Optimizations

If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table

## Proof Of Concept



    int32 public constant WINDOW = 1 days;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L12



## Recommended Mitigation Steps

Set variable to private.



## (11) Require() Should Be Used Instead Of Assert()

Severity: Gas Optimizations

## Proof Of Concept


	assert(false);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L190





## (12) Splitting Require() Statements That Use && Saves Gas

Severity: Gas Optimizations

See https://github.com/code-423n4/2022-01-xdefi-findings/issues/128 which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper

## Proof Of Concept

	require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L110

	require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L739

	require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L743

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





## (13) Use of non-uint64 state variable is less efficient than uint256

Severity: Gas Optimizations

Lower than uint256 size storage variables are less gas efficient.
Using uint64 does not give any efficiency, actually, it is the opposite as EVM operates on default of 256-bit values so uint64 is more expensive in this case as it needs a conversion. It only gives improvements in cases where you can pack variables together, e.g. structs.

## Proof Of Concept


	uint128 liquidity;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L42

	uint32 lastLiquidityAddTimestamp;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L43

	uint128 fees0;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L46

	uint128 fees1;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L47

	uint160 outerSecondPerLiquidity;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L98

	uint32 outerSecondsSpent;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L99

	uint32 globalTime = _blockTimestamp();
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L148

	uint32 timestamp = _blockTimestamp();
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L198

	uint32 _liquidityCooldown = liquidityCooldown;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L227

	uint128 liquidityNext = LiquidityMath.addDelta(currentLiquidity, liquidityDelta);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L234

	uint160 price;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L263

	uint16 timepointIndex;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L265

	uint32 time = _blockTimestamp();
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L296

	uint128 liquidityBefore = liquidity;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L354

	uint16 newTimepointIndex = _writeTimepoint(cache.timepointIndex, _blockTimestamp(), cache.tick, liquidityBefore, volumePerLiquidityInBlock);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L355

	uint128 liquidityForRA1 = uint128(FullMath.mulDiv(uint256(liquidityActual), receivedAmount1, amount1));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L463

	uint160 currentPrice;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L596

	uint128 currentLiquidity;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L598

	uint128 volumePerLiquidityInBlock;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L677

	uint160 secondsPerLiquidityCumulative;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L679

	uint16 fee;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L687

	uint160 stepSqrtPrice;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L693

	uint160 nextTickPrice;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L696

	uint32 blockTimestamp;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L718

	uint128 _liquidity = liquidity;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L897

	uint16 _fee = globalState.fee;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L900

	uint8 _communityFeeToken0 = globalState.communityFeeToken0;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L925

	uint8 _communityFeeToken1 = globalState.communityFeeToken1;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L939

	uint128 constant MAX_VOLUME_PER_LIQUIDITY = 100000 << 64;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L16

	uint16 oldestIndex;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L71

	uint16 nextIndex = index + 1;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L73

	uint160 price;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol#L9

	uint16 fee;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol#L11

	uint16 timepointIndex;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol#L12

	uint8 communityFeeToken0;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol#L13

	uint8 communityFeeToken1;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol#L14

	uint128 public override liquidity;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol#L26

	uint128 internal volumePerLiquidityInBlock;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol#L27

	uint32 public override liquidityCooldown;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol#L30

	uint16 alpha1;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L11

	uint16 alpha2;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L12

	uint32 beta1;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L13

	uint32 beta2;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L14

	uint16 gamma1;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L15

	uint16 gamma2;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L16

	uint32 volumeBeta;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L17

	uint16 volumeGamma;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L18

	uint16 baseFee;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L19

	uint8 internal constant RESOLUTION = 96;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/Constants.sol#L5

	uint16 internal constant BASE_FEE = 100;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/Constants.sol#L9

	uint128 internal constant MAX_LIQUIDITY_PER_TICK = 11505743598341114571880798222544994;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/Constants.sol#L13

	uint32 internal constant MAX_LIQUIDITY_COOLDOWN = 1 days;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/Constants.sol#L15

	uint8 internal constant MAX_COMMUNITY_FEE = 250;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/Constants.sol#L16

	uint32 public constant WINDOW = 1 days;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L12

	uint32 blockTimestamp;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L16

	uint160 secondsPerLiquidityCumulative;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L18

	uint88 volatilityCumulative; // the volatility accumulator;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L19

	uint144 volumePerLiquidityCumulative;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L21

	uint32 delta = blockTimestamp - last.blockTimestamp;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L75

	uint32 oldestTimestamp = self[oldestIndex].blockTimestamp;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L114

	uint32 target = time - secondsAgo;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L214

	uint32 timepointTimeDelta = atOrAfter.blockTimestamp - beforeOrAt.blockTimestamp;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L247

	uint32 targetDelta = target - beforeOrAt.blockTimestamp;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L248

	uint16 oldestIndex;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L299

	uint16 nextIndex = index + 1;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L301

	uint32 oldestTimestamp = oldest.blockTimestamp;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L343

	uint88 _oldestVolatilityCumulative = oldest.volatilityCumulative;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L351

	uint144 _oldestVolumePerLiquidityCumulative = oldest.volumePerLiquidityCumulative;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L352

	uint32 _prevLastBlockTimestamp = _prevLast.blockTimestamp;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L412

	uint128 liquidityTotal;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L18

	uint160 outerSecondsPerLiquidity;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L25

	uint32 outerSecondsSpent;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L26

	uint128 liquidityTotalBefore = data.liquidityTotal;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L93

	uint128 liquidityTotalAfter = LiquidityMath.addDelta(liquidityTotalBefore, liquidityDelta);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L95

	uint8 bitNumber;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L18






## (14) ++i/i++ Should Be Unchecked{++i}/unchecked{i++} When It Is Not Possible For Them To Overflow, As Is The Case When Used In For- And While-loops

Severity: Gas Optimizations

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas PER LOOP

## Proof Of Concept


	for (uint256 i = 0; i < secondsAgos.length; i++) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L307





## (15) Use assembly to check for address(0)

Severity: Gas Optimizations

Save 6 gas per instance if using assembly to check for address(0)

e.g.
assembly {
	if iszero(_addr) {
		mstore(0x00, "AddressZero")
		revert(0x00, 0x20)
	}
}

## Proof Of Concept


	function setFactory(address _factory) external override onlyOwner {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol#L36






