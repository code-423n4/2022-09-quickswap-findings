## (1) Consider Uniform SPDX License

Not all source files specify the same license.

***

// SPDX-License-Identifier: BUSL-1.1
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L1

// SPDX-License-Identifier: GPL-2.0-or-later
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/Constants.sol#L1

## (2) Solidity Version

Consider using the most recent or a more recent version of solidity.
0.8.17 is already released and the project still uses version 0.7.6.

## (3) Avoid Magic Numbers

Checking against magic numbers should be avoided.
Consider using a properly named constant instead.

Use _ for better number readability.
Large multiples of ten should use scientific notation.

***

if (volume >= 2**192) volumeShifted = (type(uint256).max) / (liquidity > 0 ? liquidity : 1);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L138

else volumeShifted = (volume << 64) / (liquidity > 0 ? liquidity : 1);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L139

if (x >= 6 * uint256(g)) return alpha;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L52

if (x >= 6 * uint256(g)) return 0;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L59

last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L80

uint256(endOfWindow.volumePerLiquidityCumulative - startOfWindow.volumePerLiquidityCumulative) >> 57
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L348

uint256(endOfWindow.volumePerLiquidityCumulative - _oldestVolumePerLiquidityCumulative) >> 57
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L355

## (4) Revert and Require Should Have Descriptive Reason Strings

require(msg.sender == owner);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L43

require(tokenA != tokenB);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L60

require(token0 != address(0));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L62

require(poolByPair[token0][token1] == address(0));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L63

require(owner != _owner);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L78

require(farmingAddress != _farmingAddress);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L85

require(vaultAddress != _vaultAddress);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L92

require(msg.sender == IAlgebraFactory(factory).owner());
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L55

require(_lower.initialized);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L122

require(_upper.initialized);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L134

require((_blockTimestamp() - lastLiquidityAddTimestamp) >= _liquidityCooldown);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L229

require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L953

require(msg.sender == IAlgebraFactory(factory).farmingAddress());
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L960

require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L968

require(msg.sender == factory);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol#L22

require(msg.sender == owner);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol#L27

require(_factory != address(0));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol#L37

require(factory == address(0));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol#L38

require(msg.sender == factory || msg.sender == IAlgebraFactory(factory).owner());
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L43

require(!self[0].initialized);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L369

require(price > 0);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L52

require(liquidity > 0);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L53

require((product = amount * price) / amount == price); // if the product overflows, we know the denominator underflows
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L70

require(liquidityShifted > product); // in addition, we must check that the denominator does not underflow
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L71

require(price > quotient);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L87

require(priceDelta < priceUpper); // forbids underflow and 0 priceLower
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TokenDeltaMath.sol#L30

require(priceUpper >= priceLower);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TokenDeltaMath.sol#L51

## (5) Avoid Overlong Lines

Limit line length to 164 characters.
Longer lines will need scrollbars in github and are hard to read anyways.

***

    return uint16(config.baseFee + sigmoid(volumePerLiquidity, config.volumeGamma, uint16(sumOfSigmoids), config.volumeBeta)); // safe since alpha1 + alpha2 + baseFee _must_ be <= type(uint16).max
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L38

## (6) Missing NatSpec

Every function should be documented, every parameter should be documented, and every return value should be documented.

***

Undocumented function:

function balanceToken0() private view returns (uint256) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L70-L70

***

Undocumented function:

function balanceToken1() private view returns (uint256) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L74-L74

***

Undocumented parameter: owner
Undocumented parameter: bottomTick
Undocumented parameter: topTick
Undocumented parameter: liquidityDelta

/**
* @dev Updates position's ticks and its fees
* @return position The Position object to operate with
* @return amount0 The amount of token0 the caller needs to send, negative if the pool needs to send it
* @return amount1 The amount of token1 the caller needs to send, negative if the pool needs to send it
*/
function _updatePositionTicksAndFees(
address owner,
int24 bottomTick,
int24 topTick,
int128 liquidityDelta
)
private
returns (
Position storage position,
int256 amount0,
int256 amount1
)
{
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L267-L286

***

Undocumented function:

function _getAmountsForLiquidity(
int24 bottomTick,
int24 topTick,
int128 liquidityDelta,
int24 currentTick,
uint160 currentPrice
)
private
pure
returns (
int256 amount0,
int256 amount1,
int128 globalLiquidityDelta
)
{
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L366-L380

***

Undocumented function:

function _payCommunityFee(address token, uint256 amount) private {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L546-L546

***

Undocumented function:

function _writeTimepoint(
uint16 timepointIndex,
uint32 blockTimestamp,
int24 tick,
uint128 liquidity,
uint128 volumePerLiquidityInBlock
) private returns (uint16 newTimepointIndex) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L551-L557

***

Undocumented function:

function _getSingleTimepoint(
uint32 blockTimestamp,
uint32 secondsAgo,
int24 startTick,
uint16 timepointIndex,
uint128 liquidityStart
)
private
view
returns (
int56 tickCumulative,
uint160 secondsPerLiquidityCumulative,
uint112 volatilityCumulative,
uint256 volumePerAvgLiquidity
)
{
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L561-L576

***

Undocumented function:

function _swapCallback(
int256 amount0,
int256 amount1,
bytes calldata data
) private {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L580-L584

***

Undocumented parameter: zeroToOne
Undocumented parameter: amountRequired
Undocumented parameter: limitSqrtPrice
Undocumented return value: amount0
Undocumented return value: amount1
Undocumented return value: currentPrice
Undocumented return value: currentTick
Undocumented return value: currentLiquidity
Undocumented return value: communityFeeAmount

/// @notice For gas optimization, locks 'globalState.unlocked' and does not release.
function _calculateSwapAndLock(
bool zeroToOne,
int256 amountRequired,
uint160 limitSqrtPrice
)
private
returns (
int256 amount0,
int256 amount1,
uint160 currentPrice,
int24 currentTick,
uint128 currentLiquidity,
uint256 communityFeeAmount
)
{
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L701-L717

***

Undocumented parameter: volatility
Undocumented parameter: volumePerLiquidity
Undocumented parameter: config
Undocumented return value: fee

/// @notice Calculates fee based on formula:
/// baseFee + sigmoidVolume(sigmoid1(volatility, volumePerLiquidity) + sigmoid2(volatility, volumePerLiquidity))
/// maximum value capped by baseFee + alpha1 + alpha2
function getFee(
uint88 volatility,
uint256 volumePerLiquidity,
Configuration memory config
) internal pure returns (uint16 fee) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L21-L29

***

Undocumented parameter: self
Undocumented parameter: time
Undocumented parameter: tick
Undocumented parameter: index
Undocumented parameter: oldestIndex
Undocumented parameter: lastTimestamp
Undocumented parameter: lastTickCumulative
Undocumented return value: avgTick

/// @dev guaranteed that the result is within the bounds of int24
/// returns int256 for fuzzy tests
function _getAverageTick(
Timepoint[UINT16_MODULO] storage self,
uint32 time,
int24 tick,
uint16 index,
uint16 oldestIndex,
uint32 lastTimestamp,
int56 lastTickCumulative
) internal view returns (int256 avgTick) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L102-L113

***

Undocumented function:

function getNewPrice(
uint160 price,
uint128 liquidity,
uint256 amount,
bool zeroToOne,
bool fromInput
) internal pure returns (uint160 resultPrice) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L45-L51

***

Undocumented function:

function getTokenADelta01(
uint160 to,
uint160 from,
uint128 liquidity
) internal pure returns (uint256) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L93-L97

***

Undocumented function:

function getTokenADelta10(
uint160 to,
uint160 from,
uint128 liquidity
) internal pure returns (uint256) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L101-L105

***

Undocumented function:

function getTokenBDelta01(
uint160 to,
uint160 from,
uint128 liquidity
) internal pure returns (uint256) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L109-L113

***

Undocumented function:

function getTokenBDelta10(
uint160 to,
uint160 from,
uint128 liquidity
) internal pure returns (uint256) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L117-L121

***

Undocumented parameter: zeroToOne

/// @notice Computes the result of swapping some amount in, or amount out, given the parameters of the swap
/// @dev The fee, plus the amount in, will never exceed the amount remaining if the swap's `amountSpecified` is positive
/// @param currentPrice The current Q64.96 sqrt price of the pool
/// @param targetPrice The Q64.96 sqrt price that cannot be exceeded, from which the direction of the swap is inferred
/// @param liquidity The usable liquidity
/// @param amountAvailable How much input or output amount is remaining to be swapped in/out
/// @param fee The fee taken from the input amount, expressed in hundredths of a bip
/// @return resultPrice The Q64.96 sqrt price after swapping the amount in/out, not to exceed the price target
/// @return input The amount to be swapped in, of either token0 or token1, based on the direction of the swap
/// @return output The amount to be received, of either token0 or token1, based on the direction of the swap
/// @return feeAmount The amount of input that will be taken as a fee
function movePriceTowardsTarget(
bool zeroToOne,
uint160 currentPrice,
uint160 targetPrice,
uint128 liquidity,
int256 amountAvailable,
uint16 fee
)
internal
pure
returns (
uint160 resultPrice,
uint256 input,
uint256 output,
uint256 feeAmount
)
{
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L124-L152

***

Undocumented function:

function uncompressAndBoundTick(int24 tick) private pure returns (int24 boundedTick) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L121-L121



