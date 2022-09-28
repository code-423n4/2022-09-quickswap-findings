## [G-01] Don't Initialize Variables with Default Value

Uninitialized variables are assigned with the types default value. Explicitly initializing a variable with it's default value costs unnecesary gas.

```
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::307 => for (uint256 i = 0; i < secondsAgos.length; i++) {
```

## [G-02] Cache Array Length Outside of Loop

Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.

```
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::307 => for (uint256 i = 0; i < secondsAgos.length; i++) {
```

## [G-03] Using > 0 costs more gas than != 0 when used on a uint in a require() statement

When dealing with unsigned integer types, comparisons with != 0 are cheaper then with > 0. This change saves 6 gas per instance

```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::110 => require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::224 => require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::434 => require(liquidityDesired > 0, 'IL');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::454 => if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::455 => if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::469 => require(liquidityActual > 0, 'IIL2');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::641 => require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::645 => require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::898 => require(_liquidity > 0, 'L');
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::46 => require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::52 => require(price > 0);
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::53 => require(liquidity > 0);
```

## [G-04] Use Shift Right/Left instead of Division/Multiplication if possible

A division/multiplication by any number x being a power of 2 can be calculated by shifting log2(x) to the right/left.

While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

```
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::53 => uint256 g8 = uint256(g)**8; // < 128 bits (8*16)
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::60 => uint256 g8 = uint256(g)**8; // < 128 bits (8*16)
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::81 => res = gHighestDegree; // g**8
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::87 => xLowestDegree *= x; // x**2
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::88 => res += (xLowestDegree * gHighestDegree) / 2;
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::94 => gHighestDegree /= g; // g**4
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::95 => xLowestDegree *= x; // x**4
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::96 => res += (xLowestDegree * gHighestDegree) / 24;
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::102 => gHighestDegree /= g; // g**2
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::52 => int256 sumOfSequence = (dt * (dt + 1)); // sumOfSequence * 2
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::53 => volatility = uint256((K**2 * sumOfSquares + 6 * B * K * sumOfSequence + 6 * dt * B**2) / (6 * dt**2));
```

## [G-05] Using private rather than public for constants, saves gas

If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table

```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::20 => address public immutable override poolDeployer;
```

## [G-06] Functions guaranteed to revert when called by normal users can be marked payable

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::77 => function setOwner(address _owner) external override onlyOwner {
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::84 => function setFarmingAddress(address _farmingAddress) external override onlyOwner {
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::91 => function setVaultAddress(address _vaultAddress) external override onlyOwner {
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::952 => function setCommunityFee(uint8 communityFee0, uint8 communityFee1) external override lock onlyFactoryOwner {
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::967 => function setLiquidityCooldown(uint32 newLiquidityCooldown) external override onlyFactoryOwner {
2022-09-quickswap/src/core/contracts/AlgebraPoolDeployer.sol::36 => function setFactory(address _factory) external override onlyOwner {
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::37 => function initialize(uint32 time, int24 tick) external override onlyPool {
```

## [G-07] Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

```
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::42 => uint128 liquidity; // The amount of liquidity concentrated in the range
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::43 => uint32 lastLiquidityAddTimestamp; // Timestamp of last adding of liquidity
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::46 => uint128 fees0; // The amount of token0 owed to a LP
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::47 => uint128 fees1; // The amount of token1 owed to a LP
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::98 => uint160 outerSecondPerLiquidity;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::99 => uint32 outerSecondsSpent;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::148 => uint32 globalTime = _blockTimestamp();
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::198 => uint32 timestamp = _blockTimestamp();
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::227 => uint32 _liquidityCooldown = liquidityCooldown;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::234 => uint128 liquidityNext = LiquidityMath.addDelta(currentLiquidity, liquidityDelta);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::244 => uint128 fees0;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::249 => uint128 fees1;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::263 => uint160 price; // The square root of the current price in Q64.96 format
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::265 => uint16 timepointIndex; // The index of the last written timepoint
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::351 => int128 globalLiquidityDelta;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::596 => uint160 currentPrice;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::598 => uint128 currentLiquidity;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::649 => uint160 currentPrice;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::651 => uint128 currentLiquidity;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::677 => uint128 volumePerLiquidityInBlock;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::679 => uint160 secondsPerLiquidityCumulative; // The global secondPerLiquidity at the moment
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::687 => uint16 fee; // The current dynamic fee
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::689 => uint16 timepointIndex; // The index of last written timepoint
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::693 => uint160 stepSqrtPrice; // The Q64.96 sqrt of the price at the start of the step
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::696 => uint160 nextTickPrice; // The Q64.96 sqrt of the price calculated from the _nextTick
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::718 => uint32 blockTimestamp;
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::16 => uint128 constant MAX_VOLUME_PER_LIQUIDITY = 100000 << 64; // maximum meaningful ratio of volume to liquidity
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::71 => uint16 oldestIndex;
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::73 => uint16 nextIndex = index + 1; // considering overflow
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::11 => uint16 alpha1; // max value of the first sigmoid
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::12 => uint16 alpha2; // max value of the second sigmoid
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::13 => uint32 beta1; // shift along the x-axis for the first sigmoid
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::14 => uint32 beta2; // shift along the x-axis for the second sigmoid
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::15 => uint16 gamma1; // horizontal stretch factor for the first sigmoid
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::16 => uint16 gamma2; // horizontal stretch factor for the second sigmoid
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::17 => uint32 volumeBeta; // shift along the x-axis for the outer volume-sigmoid
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::18 => uint16 volumeGamma; // horizontal stretch factor the outer volume-sigmoid
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::19 => uint16 baseFee; // minimum possible fee
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::5 => uint8 internal constant RESOLUTION = 96;
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::9 => uint16 internal constant BASE_FEE = 100;
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::13 => uint128 internal constant MAX_LIQUIDITY_PER_TICK = 11505743598341114571880798222544994;
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::15 => uint32 internal constant MAX_LIQUIDITY_COOLDOWN = 1 days;
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::16 => uint8 internal constant MAX_COMMUNITY_FEE = 250;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::12 => uint32 public constant WINDOW = 1 days;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::16 => uint32 blockTimestamp; // the block timestamp of the timepoint
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::18 => uint160 secondsPerLiquidityCumulative; // the seconds per liquidity since the pool was first initialized
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::19 => uint88 volatilityCumulative; // the volatility accumulator; overflow after ~34800 years is desired :)
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::75 => uint32 delta = blockTimestamp - last.blockTimestamp;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::114 => uint32 oldestTimestamp = self[oldestIndex].blockTimestamp;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::214 => uint32 target = time - secondsAgo;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::247 => uint32 timepointTimeDelta = atOrAfter.blockTimestamp - beforeOrAt.blockTimestamp;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::248 => uint32 targetDelta = target - beforeOrAt.blockTimestamp;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::299 => uint16 oldestIndex;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::301 => uint16 nextIndex = index + 1; // considering overflow
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::333 => uint16 oldestIndex;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::335 => uint16 nextIndex = index + 1; // considering overflow
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::343 => uint32 oldestTimestamp = oldest.blockTimestamp;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::351 => uint88 _oldestVolatilityCumulative = oldest.volatilityCumulative;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::402 => uint16 oldestIndex;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::412 => uint32 _prevLastBlockTimestamp = _prevLast.blockTimestamp;
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::18 => uint128 liquidityTotal; // the total position liquidity that references this tick
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::19 => int128 liquidityDelta; // amount of net liquidity added (subtracted) when tick is crossed left-right (right-left),
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::25 => uint160 outerSecondsPerLiquidity; // the seconds per unit of liquidity on the _other_ side of current tick, (relative meaning)
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::26 => uint32 outerSecondsSpent; // the seconds spent on the other side of the current tick, only has relative meaning
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::92 => int128 liquidityDeltaBefore = data.liquidityDelta;
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::93 => uint128 liquidityTotalBefore = data.liquidityTotal;
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::17 => int16 rowNumber;
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::18 => uint8 bitNumber;
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::83 => int16 rowNumber;
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::84 => uint8 bitNumber;
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::101 => int16 rowNumber;
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::102 => uint8 bitNumber;
```

## [G-08] ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, for example when used in for- and while-loops

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop

```
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::307 => for (uint256 i = 0; i < secondsAgos.length; i++) {
```

## [G-09] <x> += <y> costs more gas than <x> = <x> + <y> for state variables

use <x> = <x> + <y> or <x> = <x> - <y> instead to save gas

```
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::257 => _position.fees0 += fees0;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::258 => _position.fees1 += fees1;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::801 => amountRequired -= (step.input + step.feeAmount).toInt256(); // decrease remaining input amount
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::804 => amountRequired += step.output.toInt256(); // increase remaining output amount (since its negative)
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::810 => step.feeAmount -= delta;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::811 => communityFeeAmount += delta;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::814 => if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::922 => paid0 -= balance0Before;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::931 => totalFeeGrowth0Token += FullMath.mulDiv(paid0 - fees0, Constants.Q128, _liquidity);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::936 => paid1 -= balance1Before;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::945 => totalFeeGrowth1Token += FullMath.mulDiv(paid1 - fees1, Constants.Q128, _liquidity);
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::84 => res += xLowestDegree * gHighestDegree;
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::88 => res += (xLowestDegree * gHighestDegree) / 2;
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::92 => res += (xLowestDegree * gHighestDegree) / 6;
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::96 => res += (xLowestDegree * gHighestDegree) / 24;
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::100 => res += (xLowestDegree * gHighestDegree) / 120;
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::104 => res += (xLowestDegree * gHighestDegree) / 720;
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::107 => res += (xLowestDegree * g) / 5040 + (xLowestDegree * x) / (40320);
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::79 => last.tickCumulative += int56(tick) * delta;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::80 => last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::81 => last.volatilityCumulative += uint88(_volatilityOnRange(delta, prevTick, tick, last.averageTick, averageTick)); // always fits 88 bits
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::83 => last.volumePerLiquidityCumulative += volumePerLiquidity;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::119 => index -= 1; // considering underflow
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::251 => beforeOrAt.tickCumulative += ((atOrAfter.tickCumulative - beforeOrAt.tickCumulative) / timepointTimeDelta) * targetDelta;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::252 => beforeOrAt.secondsPerLiquidityCumulative += uint160(
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::255 => beforeOrAt.volatilityCumulative += ((atOrAfter.volatilityCumulative - beforeOrAt.volatilityCumulative) / timepointTimeDelta) * targetDelta;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::256 => beforeOrAt.volumePerLiquidityCumulative +=
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::58 => innerFeeGrowth0Token -= upper.outerFeeGrowth0Token;
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::59 => innerFeeGrowth1Token -= upper.outerFeeGrowth1Token;
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::92 => tick -= int24(255 - getMostSignificantBit(_row));
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::95 => tick -= int24(bitNumber);
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::100 => tick += 1;
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::112 => tick += int24(getSingleSignificantBit(-_row & _row)); // least significant bit
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::115 => tick += int24(255 - bitNumber);
```

## [G-10] abi.encode() is less efficient than abi.encodePacked()

use abi.encodePacked() where possible to save gas

```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::123 => pool = address(uint256(keccak256(abi.encodePacked(hex'ff', poolDeployer, keccak256(abi.encode(token0, token1)), POOL_INIT_CODE_HASH))));
2022-09-quickswap/src/core/contracts/AlgebraPoolDeployer.sol::51 => pool = address(new AlgebraPool{salt: keccak256(abi.encode(token0, token1))}());
```

## [G-11] Use a more recent version of solidity

Use a solidity version of at least 0.8.0 to get overflow protection without SafeMath
Use a solidity version of at least 0.8.2 to get compiler automatic inlining
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/AlgebraPoolDeployer.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::2 => pragma solidity =0.7.6;
```

## [G-12] Prefix increments cheaper than Postfix increments

++i costs less gas than i++, especially when it's used in for-loops (--i/i-- too)
Saves 5 gas PER LOOP

```
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::307 => for (uint256 i = 0; i < secondsAgos.length; i++) {
```

## [G-13] Splitting require() statements that use && saves gas

Saves 16 gas per instance.
If you're using the Optimizer at 200, instead of using the && operator in a single require statement to check multiple conditions, multiple require statements with 1 condition per require statement should be used to save gas:

```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::110 => require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::739 => require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::743 => require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::953 => require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::968 => require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::46 => require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
```

## [G-14] Usage of assert() instead of require()

Between solc 0.4.10 and 0.8.0, require() used REVERT (0xfd) opcode which refunded remaining gas on failure while assert() used INVALID (0xfe) opcode which consumed all the supplied gas.
require() should be used for checking error conditions on inputs and return values while assert() should be used for invariant checking (properly functioning code should never reach a failing assert statement, unless there is a bug in your contract you should fix).

```
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::190 => assert(false);
```

## [G-15] Not using the named return variables when a function returns, wastes deployment gas

It is not necessary to have both a named return and a return statement.

```
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::407 => ) private view returns (Position storage) {
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::557 => ) private returns (uint16 newTimepointIndex) {
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::115 => ) external view override onlyPool returns (uint112 TWVolatilityAverage, uint256 TWVolumePerLiqAverage) {
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::126 => ) external override onlyPool returns (uint16 indexUpdated) {
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::135 => ) external pure override returns (uint128 volumePerLiquidity) {
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::155 => ) external view override onlyPool returns (uint16 fee) {
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::29 => ) internal pure returns (uint16 fee) {
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::49 => ) internal pure returns (uint256 res) {
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::154 => ) private view returns (Timepoint storage beforeOrAt, Timepoint storage atOrAfter) {
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::213 => ) internal view returns (Timepoint memory targetTimepoint) {
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::391 => ) internal returns (uint16 indexUpdated) {
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::25 => ) internal pure returns (uint160 resultPrice) {
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::41 => ) internal pure returns (uint160 resultPrice) {
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::51 => ) internal pure returns (uint160 resultPrice) {
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::137 => ) internal returns (int128 liquidityDelta) {
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::46 => function getMostSignificantBit(uint256 word) internal pure returns (uint8 mostBitPos) {
```

## [G-16] Use assembly to check for address(0)

Saves 6 gas per instance if using assembly to check for address(0)

e.g.
```
assembly {
 if iszero(_addr) {
  mstore(0x00, "zero address")
  revert(0x00, 0x20)
 }
}
```

instances:

```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::62 => require(token0 != address(0));
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::752 => if (activeIncentive != address(0)) {
2022-09-quickswap/src/core/contracts/AlgebraPoolDeployer.sol::37 => require(_factory != address(0));
```

## [G-17] internal functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::122 => function computeAddress(address token0, address token1) internal view returns (address pool) {
```

## [G-18] internal functions not called by the contract should be removed to save deployment gas

If the functions are required by an interface, the contract should inherit from that interface and use the override keyword

```
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::14 => function toggleTick(mapping(int16 => uint256) storage self, int24 tick) internal {
```