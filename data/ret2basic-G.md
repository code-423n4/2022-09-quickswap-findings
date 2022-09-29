# QuickSwap and StellaSwap Contest Gas Optimization Report

## Summary

The following gas optimization issues were found during the code audit:

1. Cache `<array>.length` (1 instance)
2. Use `unchecked{}` to suppress overflow/underflow check (1 instance)
3. Using `bool`s for storage incurs overhead (21 instances)
4. Use `!= 0` instead of `> 0` when comparing uint (35 instances)
5. Don't initialize variables with default value (2 instances)
6. Use `++i`/`--i` instead of `i++`/`i--` (1 instance)
7. Split `require(xxx && yyy)` to `require(xxx)` and `require(yyy)` (8 instances)
8. Use `abi.encodePacked()` instead of `abi.encode()` (2 instances)
9. Use `private` instead of `public` for constants (1 instance)
10. Use shift right/left instead of division/multiplication if possible (1 instance)

Total 73 instances of 10 issues.

## 1. Cache `<array>.length` (1 instance)

If `<array>.length` is used as for loop termination condition, then the `.length` method will be called in each iteration. Caching it in a local variable can save gas.

```solidity
src/core/contracts/libraries/DataStorage.sol::307 => for (uint256 i = 0; i < secondsAgos.length; i++) {
```

## 2. Use `unchecked{}` to suppress overflow/underflow check (1 instance)

Starting from version 0.8.0, Solidity does overflow/underflow checks by default. It is a good feature to prevent vulnerabilities but it has a significant overhead, especially when used in for loop. When using uint256/int256, it is extremely hard to trigger overflow, so it makes sense to skip these checks. To suppress the overflow/underflow checks, use `unchecked {}`. For increment situations, just use `unchecked {}` directly; for decrement situations, add a `require()` statement before decrementing to prevent underflow.

```solidity
src/core/contracts/libraries/DataStorage.sol::307 => for (uint256 i = 0; i < secondsAgos.length; i++) {
```

## 3. Using `bool`s for storage incurs overhead (21 instances)

Use `uint256(1)` and `uint256(2)` for true/false. Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.

```solidity
src/core/contracts/libraries/DataStorage.sol::15 => bool initialized; // whether or not the timepoint is initialized

src/core/contracts/libraries/PriceMovementMath.sol::24 => bool zeroToOne

src/core/contracts/libraries/PriceMovementMath.sol::40 => bool zeroToOne

src/core/contracts/libraries/PriceMovementMath.sol::49 => bool zeroToOne,

src/core/contracts/libraries/PriceMovementMath.sol::50 => bool fromInput

src/core/contracts/libraries/PriceMovementMath.sol::137 => bool zeroToOne,

src/core/contracts/libraries/TickManager.sol::27 => bool initialized; // these 8 bits are set to prevent fresh sstores when crossing newly initialized ticks

src/core/contracts/libraries/TickManager.sol::88 => bool upper

src/core/contracts/libraries/TickTable.sol::71 => bool lte

src/core/contracts/libraries/TokenDeltaMath.sol::27 => bool roundUp

src/core/contracts/libraries/TokenDeltaMath.sol::49 => bool roundUp

src/core/contracts/AlgebraPool.sol::84 => bool initialized,

src/core/contracts/AlgebraPool.sol::293 => bool toggledBottom;

src/core/contracts/AlgebraPool.sol::294 => bool toggledTop;

src/core/contracts/AlgebraPool.sol::591 => bool zeroToOne,

src/core/contracts/AlgebraPool.sol::629 => bool zeroToOne,

src/core/contracts/AlgebraPool.sol::680 => bool computedLatestTimepoint; //  if we have already fetched _tickCumulative_ and _secondPerLiquidity_ from the DataOperator

src/core/contracts/AlgebraPool.sol::686 => bool exactInput; // Whether the exact input or output is specified

src/core/contracts/AlgebraPool.sol::695 => bool initialized; // True if the _nextTick is initialized

src/core/contracts/AlgebraPool.sol::704 => bool zeroToOne,

src/core/contracts/AlgebraPool.sol::728 => bool unlocked = globalState.unlocked;
```

## 4. Use `!= 0` instead of `> 0` when comparing uint (35 instances)

When dealing with unsigned integer types, comparisons with `!= 0` are cheaper then with `> 0`.

```solidity
src/core/contracts/libraries/DataStorage.sol::80 => last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0

src/core/contracts/libraries/FullMath.sol::114 => require(denominator > 0);

src/core/contracts/libraries/FullMath.sol::120 => if (mulmod(a, b, denominator) > 0) {

src/core/contracts/libraries/PriceMovementMath.sol::52 => require(price > 0);

src/core/contracts/libraries/PriceMovementMath.sol::53 => require(liquidity > 0);

src/core/contracts/libraries/TickMath.sol::52 => if (tick > 0) ratio = type(uint256).max / ratio;

src/core/contracts/AlgebraFactory.sol::110 => require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');

src/core/contracts/AlgebraPool.sol::224 => require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges

src/core/contracts/AlgebraPool.sol::228 => if (_liquidityCooldown > 0) {

src/core/contracts/AlgebraPool.sol::237 => liquidityNext > 0 ? (liquidityDelta > 0 ? _blockTimestamp() : lastLiquidityAddTimestamp) : 0

src/core/contracts/AlgebraPool.sol::434 => require(liquidityDesired > 0, 'IL');

src/core/contracts/AlgebraPool.sol::451 => if (amount0 > 0) receivedAmount0 = balanceToken0();

src/core/contracts/AlgebraPool.sol::452 => if (amount1 > 0) receivedAmount1 = balanceToken1();

src/core/contracts/AlgebraPool.sol::454 => if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');

src/core/contracts/AlgebraPool.sol::455 => if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');

src/core/contracts/AlgebraPool.sol::469 => require(liquidityActual > 0, 'IIL2');

src/core/contracts/AlgebraPool.sol::505 => if (amount0 > 0) TransferHelper.safeTransfer(token0, recipient, amount0);

src/core/contracts/AlgebraPool.sol::506 => if (amount1 > 0) TransferHelper.safeTransfer(token1, recipient, amount1);

src/core/contracts/AlgebraPool.sol::617 => if (communityFee > 0) {

src/core/contracts/AlgebraPool.sol::641 => require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');

src/core/contracts/AlgebraPool.sol::645 => require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');

src/core/contracts/AlgebraPool.sol::667 => if (communityFee > 0) {

src/core/contracts/AlgebraPool.sol::734 => (cache.amountRequiredInitial, cache.exactInput) = (amountRequired, amountRequired > 0);

src/core/contracts/AlgebraPool.sol::808 => if (cache.communityFee > 0) {

src/core/contracts/AlgebraPool.sol::814 => if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);

src/core/contracts/AlgebraPool.sol::898 => require(_liquidity > 0, 'L');

src/core/contracts/AlgebraPool.sol::904 => if (amount0 > 0) {

src/core/contracts/AlgebraPool.sol::911 => if (amount1 > 0) {

src/core/contracts/AlgebraPool.sol::924 => if (paid0 > 0) {

src/core/contracts/AlgebraPool.sol::927 => if (_communityFeeToken0 > 0) {

src/core/contracts/AlgebraPool.sol::938 => if (paid1 > 0) {

src/core/contracts/AlgebraPool.sol::941 => if (_communityFeeToken1 > 0) {

src/core/contracts/DataStorageOperator.sol::46 => require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');

src/core/contracts/DataStorageOperator.sol::138 => if (volume >= 2**192) volumeShifted = (type(uint256).max) / (liquidity > 0 ? liquidity : 1);

src/core/contracts/DataStorageOperator.sol::139 => else volumeShifted = (volume << 64) / (liquidity > 0 ? liquidity : 1);
```

## 5. Don't initialize variables with default value (2 instances)

Uninitialized variables are assigned with the types default value. Explicitly initializing a variable with it's default value costs unnecesary gas.

```solidity
src/core/contracts/libraries/DataStorage.sol::307 => for (uint256 i = 0; i < secondsAgos.length; i++) {

src/core/contracts/libraries/TickMath.sol::71 => uint256 msb = 0;
```

## 6. Use `++i`/`--i` instead of `i++`/`i--` (1 instance)

Using `++i`/`--i` saves 6 gas per loop.

```solidity
src/core/contracts/libraries/DataStorage.sol::307 => for (uint256 i = 0; i < secondsAgos.length; i++) {
```

## 7. Split `require(xxx && yyy)` to `require(xxx)` and `require(yyy)` (8 instances)

Instead of using operator && on single require check, using double `require()` checks can save more gas.

```solidity
src/core/contracts/libraries/TickMath.sol::67 => require(price >= MIN_SQRT_RATIO && price < MAX_SQRT_RATIO, 'R');

src/core/contracts/libraries/TransferHelper.sol::22 => require(success && (data.length == 0 || abi.decode(data, (bool))), 'TF');

src/core/contracts/AlgebraFactory.sol::110 => require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');

src/core/contracts/AlgebraPool.sol::739 => require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');

src/core/contracts/AlgebraPool.sol::743 => require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');

src/core/contracts/AlgebraPool.sol::953 => require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));

src/core/contracts/AlgebraPool.sol::968 => require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);

src/core/contracts/DataStorageOperator.sol::46 => require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
```

## 8. Use `abi.encodePacked()` instead of `abi.encode()` (2 instances)

`abi.encodePacked()` is more efficient than `abi.encode()`.

```solidity
src/core/contracts/AlgebraFactory.sol::123 => pool = address(uint256(keccak256(abi.encodePacked(hex'ff', poolDeployer, keccak256(abi.encode(token0, token1)), POOL_INIT_CODE_HASH))));

src/core/contracts/AlgebraPoolDeployer.sol::51 => pool = address(new AlgebraPool{salt: keccak256(abi.encode(token0, token1))}());
```

## 9. Use `private` instead of `public` for constants (1 instance)

If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table.

```solidity
src/core/contracts/libraries/DataStorage.sol::12 => uint32 public constant WINDOW = 1 days;
```

## 10. Use shift right/left instead of division/multiplication if possible (1 instance)

A division/multiplication by any number `x` being a power of 2 can be calculated by shifting `log2(x)` to the right/left. While the `DIV` opcode uses 5 gas, the `SHR` opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

```solidity
src/core/contracts/libraries/AdaptiveFee.sol::88 => res += (xLowestDegree * gHighestDegree) / 2;
```
