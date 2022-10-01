# **Gas Optimization**

Serial No. | Issue Name | Instances
--- | --- | ---
G-1 | Pre increment costs less gas as compared to Post increment | 3
G-2 | Increments can be unchecked | 2
G-3 | Using > 0 costs more gas than != 0 when used on a uint in a require() statement | 11
G-4 | array.length should not be looked up in every loop of a for-loop | 1
G-5 | Splitting require() statements that use && saves gas | 6
G-6 | Use custom errors rather than revert()/require() strings to save deployment gas | 31
G-7 | Usage of assert() instead of require() | 1
G-8 | Strict inequalities (>) are more expensive than non-strict ones (>=) | 5
G-9 | x += y costs more gas than x = x + y for state variables | 31
G-10 | abi.encode() is less efficient than abi.encodePacked() | 2
G-11 | Using bools for storage incurs overhead | 6
G-12 | Use a more recent version of solidity | 12
G-13 | Use Assembly to check for address(0) | 3
G-14 | Functions guaranteed to revert when called by normal users can be marked payable | 8
G-15 | Use Shift Right/Left instead of Division/Multiplication 2X | 1
G-16 | Using private rather than public for constants / immutable, saves gas | 6
G-17 | Using `calldata` instead of `memory` for read-only arguments in `external` functions saves gas | 1

----
***Total instances found in this contest: 160 | Among all files in scope***

## 1. Pre-increment costs less gas as compared to Post-increment :

++i costs less gas as compared to i++ for unsigned integer, as per-increment is cheaper(its about 5 gas per iteration cheaper)

i++ increments i and returns initial value of i. Which means

```
uint i =  1;
i++; // ==1 but i ==2
```

But ++i returns the actual incremented value:

```
uint i = 1;
++i; // ==2 and i ==2 , no need for temporary variable here
```

In the first case, the compiler has create a temporary variable (when used) for returning 1 instead of 2.

### Instances:
[DataStorage.sol:L307](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L307)
[TickTable.sol:L100](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L100)
[DataStorage.sol:L119](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L119)

```
core/contracts/libraries/DataStorage.sol:307:    for (uint256 i = 0; i < secondsAgos.length; i++) {
core/contracts/libraries/TickTable.sol:100:      tick += 1;
core/contracts/libraries/DataStorage.sol:119:        index -= 1; // considering underflow
``` 

### Recommendations:
Change post-increment to pre-increment.

-----

## 2. ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, as is the case when used in for- and while-loops

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas [PER LOOP](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked)

### Instances:
[DataStorage.sol:L307](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L307)
[DataStorage.sol:L307](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L307)

```
core/contracts/libraries/DataStorage.sol:307:    for (uint256 i = 0; i < secondsAgos.length; i++) {
core/contracts/libraries/DataStorage.sol:307:    for (uint256 i = 0; i < secondsAgos.length; i++) {
``` 

### Reference:

[https://code4rena.com/reports/2022-05-cally#g-11-ii-should-be-uncheckediuncheckedi-when-it-is-not-possible-for-them-to-overflow-as-is-the-case-when-used-in-for--and-while-loops](https://code4rena.com/reports/2022-05-cally#g-11-ii-should-be-uncheckediuncheckedi-when-it-is-not-possible-for-them-to-overflow-as-is-the-case-when-used-in-for--and-while-loops)

-----

## 3. Using > 0 costs more gas than != 0 when used on a uint in a require() statement

> 0 is less efficient than != 0 for unsigned integers (with proof)
!= 0 costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled (6 gas) Proof: While it may seem that > 0 is cheaper than !=, this is only true without the optimizer enabled and outside a require statement. If you enable the optimizer at 10k AND you’re in a require statement, this will save gas. You can see this tweet for more proof: https://twitter.com/gzeon/status/1485428085885640706

### Instances
[AlgebraFactory.sol:L110](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L110)
[PriceMovementMath.sol:L52](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L52)
[PriceMovementMath.sol:L53](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L53)
[DataStorageOperator.sol:L46](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L46)
[AlgebraPool.sol:L224](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L224)
[AlgebraPool.sol:L434](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L434)
[AlgebraPool.sol:L454](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L454)
[AlgebraPool.sol:L455](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L455)
[AlgebraPool.sol:L469](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L469)
[AlgebraPool.sol:L641](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L641)
[AlgebraPool.sol:L645](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L645)
[AlgebraPool.sol:L898](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L898)

```
core/contracts/AlgebraFactory.sol:110:    require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
core/contracts/libraries/PriceMovementMath.sol:52:    require(price > 0);
core/contracts/libraries/PriceMovementMath.sol:53:    require(liquidity > 0);
core/contracts/DataStorageOperator.sol:46:    require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
core/contracts/AlgebraPool.sol:224:      require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges
core/contracts/AlgebraPool.sol:434:    require(liquidityDesired > 0, 'IL');
core/contracts/AlgebraPool.sol:454:      if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');
core/contracts/AlgebraPool.sol:455:      if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');
core/contracts/AlgebraPool.sol:469:    require(liquidityActual > 0, 'IIL2');
core/contracts/AlgebraPool.sol:641:      require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');
core/contracts/AlgebraPool.sol:645:      require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');
core/contracts/AlgebraPool.sol:898:    require(_liquidity > 0, 'L');
``` 

### Reference:

[https://twitter.com/gzeon/status/1485428085885640706](https://twitter.com/gzeon/status/1485428085885640706)

### Remediation:

I suggest changing > 0 with != 0. Also, please enable the Optimizer.

-----

## 4. An array's length should be cached to save gas in for-loops

Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack.
Caching the array length in the stack saves around 3 gas per iteration.

### Instances:
[DataStorage.sol:L307](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L307)

```
core/contracts/libraries/DataStorage.sol:307:    for (uint256 i = 0; i < secondsAgos.length; i++) {
``` 


### Remediation:
Here, I suggest storing the array's length in a variable before the for-loop, and use it instead.

-----

## 5. Splitting require() statements that use && saves gas

Require statements including conditions with the && operator can be broken down in multiple require statements to save gas.

### Instances
[AlgebraFactory.sol:L110](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L110)
[DataStorageOperator.sol:L46](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L46)
[AlgebraPool.sol:L739](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L739)
[AlgebraPool.sol:L743](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L743)
[AlgebraPool.sol:L953](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L953)
[AlgebraPool.sol:L968](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L968)


```
core/contracts/AlgebraFactory.sol:110:    require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
core/contracts/DataStorageOperator.sol:46:    require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
core/contracts/AlgebraPool.sol:739:        require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
core/contracts/AlgebraPool.sol:743:        require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
core/contracts/AlgebraPool.sol:953:    require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));
core/contracts/AlgebraPool.sol:968:    require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
``` 

### Mitigation:
Breakdown each condition in a separate require statement (though require statements should be replaced with custom errors)

-----

## 6. Custom Errors instead of Revert Strings to save Gas

Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met). Starting from Solidity v0.8.4,there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., revert("Insufficient funds.");),but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.
Custom errors are defined using the error statement, which can be used inside and outside of contracts (including interfaces and libraries).

### Instances

```
core/contracts/AlgebraFactory.sol:109:    require(uint256(alpha1) + uint256(alpha2) + uint256(baseFee) <= type(uint16).max, 'Max fee exceeded');
core/contracts/AlgebraFactory.sol:110:    require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
core/contracts/libraries/TickTable.sol:15:    require(tick % Constants.TICK_SPACING == 0, 'tick is not spaced'); // ensure that the tick is spaced
core/contracts/libraries/DataStorage.sol:238:    require(lteConsideringOverflow(self[oldestIndex].blockTimestamp, target, time), 'OLD');
core/contracts/DataStorageOperator.sol:27:    require(msg.sender == pool, 'only pool can call this');
core/contracts/DataStorageOperator.sol:45:    require(uint256(_feeConfig.alpha1) + uint256(_feeConfig.alpha2) + uint256(_feeConfig.baseFee) <= type(uint16).max, 'Max fee exceeded');
core/contracts/DataStorageOperator.sol:46:    require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
core/contracts/AlgebraPool.sol:60:    require(topTick < TickMath.MAX_TICK + 1, 'TUM');
core/contracts/AlgebraPool.sol:61:    require(topTick > bottomTick, 'TLU');
core/contracts/AlgebraPool.sol:62:    require(bottomTick > TickMath.MIN_TICK - 1, 'TLM');
core/contracts/AlgebraPool.sol:194:    require(globalState.price == 0, 'AI');
core/contracts/AlgebraPool.sol:224:      require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges
core/contracts/AlgebraPool.sol:434:    require(liquidityDesired > 0, 'IL');
core/contracts/AlgebraPool.sol:454:      if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');
core/contracts/AlgebraPool.sol:455:      if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');
core/contracts/AlgebraPool.sol:469:    require(liquidityActual > 0, 'IIL2');
core/contracts/AlgebraPool.sol:474:      require((amount0 = uint256(amount0Int)) <= receivedAmount0, 'IIAM2');
core/contracts/AlgebraPool.sol:475:      require((amount1 = uint256(amount1Int)) <= receivedAmount1, 'IIAM2');
core/contracts/AlgebraPool.sol:608:      require(balance0Before.add(uint256(amount0)) <= balanceToken0(), 'IIA');
core/contracts/AlgebraPool.sol:614:      require(balance1Before.add(uint256(amount1)) <= balanceToken1(), 'IIA');
core/contracts/AlgebraPool.sol:636:    require(globalState.unlocked, 'LOK');
core/contracts/AlgebraPool.sol:641:      require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');
core/contracts/AlgebraPool.sol:645:      require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');
core/contracts/AlgebraPool.sol:731:      require(unlocked, 'LOK');
core/contracts/AlgebraPool.sol:733:      require(amountRequired != 0, 'AS');
core/contracts/AlgebraPool.sol:739:        require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
core/contracts/AlgebraPool.sol:743:        require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
core/contracts/AlgebraPool.sol:898:    require(_liquidity > 0, 'L');
core/contracts/AlgebraPool.sol:921:    require(balance0Before.add(fee0) <= paid0, 'F0');
core/contracts/AlgebraPool.sol:935:    require(balance1Before.add(fee1) <= paid1, 'F1');
core/contracts/base/PoolState.sol:41:    require(globalState.unlocked, 'LOK');
``` 


### Remediation:
I suggest replacing revert strings with custom errors.

-----

## 7. Usage of assert() instead of require()

Between solc 0.4.10 and 0.8.0, require() used REVERT (0xfd) opcode which refunded the remaining gas on failure while assert() used INVALID (0xfe) opcode which consumed all the supplied gas. (see [https://docs.soliditylang.org/en/v0.8.1/control-structures.html#error-handling-assert-require-revert-and-exceptions](https://docs.soliditylang.org/en/v0.8.1/control-structures.html#error-handling-assert-require-revert-and-exceptions)).
require() should be used for checking error conditions on inputs and return values while assert() should be used for invariant checking (properly functioning code should never reach a failing assert statement, unless there is a bug in your contract you should fix).
From the current usage of assert, my guess is that they can be replaced with require unless a Panic really is intended.

### Instances:

[DataStorage.sol:L190](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L190)

```
core/contracts/libraries/DataStorage.sol:190:    assert(false);
``` 

### References:

[https://code4rena.com/reports/2022-05-bunker/#g-08-usage-of-assert-instead-of-require](https://code4rena.com/reports/2022-05-bunker/#g-08-usage-of-assert-instead-of-require)

-----



## 8. Strict inequalities (>) are more expensive than non-strict ones (>=)

Strict inequalities (>) are more expensive than non-strict ones (>=). This is due to some supplementary checks (ISZERO, 3 gas. I suggest using >= instead of > to avoid some opcodes here:

### Instances:
[DataStorage.sol:L99](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L99)
[AlgebraPool.sol:L237](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L237)
[AlgebraPool.sol:L498](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L498)
[AlgebraPool.sol:L499](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L499)
[AlgebraPool.sol:L734](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L734)


```
core/contracts/libraries/DataStorage.sol:99:    res = a > currentTime;
core/contracts/AlgebraPool.sol:237:        liquidityNext > 0 ? (liquidityDelta > 0 ? _blockTimestamp() : lastLiquidityAddTimestamp) : 0
core/contracts/AlgebraPool.sol:498:    amount0 = amount0Requested > positionFees0 ? positionFees0 : amount0Requested;
core/contracts/AlgebraPool.sol:499:    amount1 = amount1Requested > positionFees1 ? positionFees1 : amount1Requested;
core/contracts/AlgebraPool.sol:734:      (cache.amountRequiredInitial, cache.exactInput) = (amountRequired, amountRequired > 0);
``` 
### References:

[https://code4rena.com/reports/2022-04-badger-citadel/#g-31--is-cheaper-than](https://code4rena.com/reports/2022-04-badger-citadel/#g-31--is-cheaper-than)


-----

## 9. x += y costs more gas than x = x + y for state variables

### Instances:
[TickTable.sol:L100](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L100)
[TickTable.sol:L112](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L112)
[TickTable.sol:L115](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L115)
```
core/contracts/libraries/TickTable.sol:100:      tick += 1;
core/contracts/libraries/TickTable.sol:112:        tick += int24(getSingleSignificantBit(-_row & _row)); // least significant bit
core/contracts/libraries/TickTable.sol:115:        tick += int24(255 - bitNumber);
```
[DataStorage.sol:L79](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L79)
[DataStorage.sol:L80](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L80)
[DataStorage.sol:L81](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L81)
[DataStorage.sol:L83](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L83)
[DataStorage.sol:L251](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L251)
[DataStorage.sol:L252](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L252)
[DataStorage.sol:L255](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L255)
[DataStorage.sol:L256](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L256)
```
core/contracts/libraries/DataStorage.sol:79:    last.tickCumulative += int56(tick) * delta;
core/contracts/libraries/DataStorage.sol:80:    last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0
core/contracts/libraries/DataStorage.sol:81:    last.volatilityCumulative += uint88(_volatilityOnRange(delta, prevTick, tick, last.averageTick, averageTick)); // always fits 88 bits
core/contracts/libraries/DataStorage.sol:83:    last.volumePerLiquidityCumulative += volumePerLiquidity;
core/contracts/libraries/DataStorage.sol:251:      beforeOrAt.tickCumulative += ((atOrAfter.tickCumulative - beforeOrAt.tickCumulative) / timepointTimeDelta) * targetDelta;
core/contracts/libraries/DataStorage.sol:252:      beforeOrAt.secondsPerLiquidityCumulative += uint160(
core/contracts/libraries/DataStorage.sol:255:      beforeOrAt.volatilityCumulative += ((atOrAfter.volatilityCumulative - beforeOrAt.volatilityCumulative) / timepointTimeDelta) * targetDelta;
core/contracts/libraries/DataStorage.sol:256:      beforeOrAt.volumePerLiquidityCumulative +=
```
[AdaptiveFee.sol:L84](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L84)
[AdaptiveFee.sol:L88](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L88)
[AdaptiveFee.sol:L92](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L92)
[AdaptiveFee.sol:L96](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L96)
[AdaptiveFee.sol:L100](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L100)
[AdaptiveFee.sol:L104](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L104)
[AdaptiveFee.sol:L107](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L107)
```
core/contracts/libraries/AdaptiveFee.sol:84:    res += xLowestDegree * gHighestDegree;
core/contracts/libraries/AdaptiveFee.sol:88:    res += (xLowestDegree * gHighestDegree) / 2;
core/contracts/libraries/AdaptiveFee.sol:92:    res += (xLowestDegree * gHighestDegree) / 6;
core/contracts/libraries/AdaptiveFee.sol:96:    res += (xLowestDegree * gHighestDegree) / 24;
core/contracts/libraries/AdaptiveFee.sol:100:    res += (xLowestDegree * gHighestDegree) / 120;
core/contracts/libraries/AdaptiveFee.sol:104:    res += (xLowestDegree * gHighestDegree) / 720;
core/contracts/libraries/AdaptiveFee.sol:107:    res += (xLowestDegree * g) / 5040 + (xLowestDegree * x) / (40320);
```

[AlgebraPool.sol:L257](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L257)
[AlgebraPool.sol:L258](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L258)
[AlgebraPool.sol:L804](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L804)
[AlgebraPool.sol:L811](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L811)
[AlgebraPool.sol:L814](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L814)
[AlgebraPool.sol:L931](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L931)
[AlgebraPool.sol:L945](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L945)
```
core/contracts/AlgebraPool.sol:257:      _position.fees0 += fees0;
core/contracts/AlgebraPool.sol:258:      _position.fees1 += fees1;
core/contracts/AlgebraPool.sol:804:        amountRequired += step.output.toInt256(); // increase remaining output amount (since its negative)
core/contracts/AlgebraPool.sol:811:        communityFeeAmount += delta;
core/contracts/AlgebraPool.sol:814:      if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);
core/contracts/AlgebraPool.sol:931:      totalFeeGrowth0Token += FullMath.mulDiv(paid0 - fees0, Constants.Q128, _liquidity);
core/contracts/AlgebraPool.sol:945:      totalFeeGrowth1Token += FullMath.mulDiv(paid1 - fees1, Constants.Q128, _liquidity);
```
[TickTable.sol:L92](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L92)
[TickTable.sol:L95](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L95)

```
core/contracts/libraries/TickTable.sol:92:        tick -= int24(255 - getMostSignificantBit(_row));
core/contracts/libraries/TickTable.sol:95:        tick -= int24(bitNumber);
```
[DataStorage.sol:L119](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L119)
```
core/contracts/libraries/DataStorage.sol:119:        index -= 1; // considering underflow
```
[AlgebraPool.sol:L801](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L801)
[AlgebraPool.sol:L810](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L810)
[AlgebraPool.sol:L922](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L922)
[AlgebraPool.sol:L936](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L936)
```
core/contracts/AlgebraPool.sol:801:        amountRequired -= (step.input + step.feeAmount).toInt256(); // decrease remaining input amount
core/contracts/AlgebraPool.sol:810:        step.feeAmount -= delta;
core/contracts/AlgebraPool.sol:922:    paid0 -= balance0Before;
core/contracts/AlgebraPool.sol:936:    paid1 -= balance1Before;
``` 



similarly `<x> *= <y>` costs more gas as compared to `<x> = <x> * <y>`
### Links to github files
[AdaptiveFee.sol:L87](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L87)
[AdaptiveFee.sol:L91](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L91)
[AdaptiveFee.sol:L95](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L95)
[AdaptiveFee.sol:L99](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L99)
[AdaptiveFee.sol:L103](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L103)
[AdaptiveFee.sol:L106](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L106)
### Instances
```
src/core/contracts/libraries/AdaptiveFee.sol:87:    xLowestDegree *= x; // x**2
src/core/contracts/libraries/AdaptiveFee.sol:91:    xLowestDegree *= x; // x**3
src/core/contracts/libraries/AdaptiveFee.sol:95:    xLowestDegree *= x; // x**4
src/core/contracts/libraries/AdaptiveFee.sol:99:    xLowestDegree *= x; // x**5
src/core/contracts/libraries/AdaptiveFee.sol:103:    xLowestDegree *= x; // x**6
src/core/contracts/libraries/AdaptiveFee.sol:106:    xLowestDegree *= x; // x**7
```
similarly `<x> /= <y>` costs more gas as compared to `<x> = <x> / <y>`
### Links to github files
[AdaptiveFee.sol:L83](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L83)
[AdaptiveFee.sol:L86](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L86)
[AdaptiveFee.sol:L90](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L90)
[AdaptiveFee.sol:L94](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L94)
[AdaptiveFee.sol:L98](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L98)
[AdaptiveFee.sol:L102](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L102)
[TickTable.sol:L16](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickTable.sol#L16)
### Instances
```
src/core/contracts/libraries/AdaptiveFee.sol:83:    gHighestDegree /= g; // g**7
src/core/contracts/libraries/AdaptiveFee.sol:86:    gHighestDegree /= g; // g**6
src/core/contracts/libraries/AdaptiveFee.sol:90:    gHighestDegree /= g; // g**5
src/core/contracts/libraries/AdaptiveFee.sol:94:    gHighestDegree /= g; // g**4
src/core/contracts/libraries/AdaptiveFee.sol:98:    gHighestDegree /= g; // g**3
src/core/contracts/libraries/AdaptiveFee.sol:102:    gHighestDegree /= g; // g**2
src/core/contracts/libraries/TickTable.sol:16:    tick /= Constants.TICK_SPACING; // compress tick
```



### References:

[https://github.com/code-423n4/2022-05-backd-findings/issues/108](https://github.com/code-423n4/2022-05-backd-findings/issues/108)


-----



## 10. abi.encode() is less efficient than abi.encodepacked()

Use abi.encodepacked() instead of abi.encode()

### Instances:
[AlgebraPoolDeployer.sol:L51](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol#L51)
```
core/contracts/AlgebraPoolDeployer.sol:51:    pool = address(new AlgebraPool{salt: keccak256(abi.encode(token0, token1))}());
```
[AlgebraPoolDeployer.sol:L51](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol#L51)
```
core/contracts/AlgebraPoolDeployer.sol:51:    pool = address(new AlgebraPool{salt: keccak256(abi.encode(token0, token1))}());
```


### Reference:

[https://code4rena.com/reports/2022-06-notional-coop#15-abiencode-is-less-efficient-than-abiencodepacked](https://code4rena.com/reports/2022-06-notional-coop#15-abiencode-is-less-efficient-than-abiencodepacked)


-----



## 11. Using bools for storage incurs overhead

```
// Booleans are more expensive than uint256 or any type that takes up a full
// word because each write operation emits an extra SLOAD to first read the
// slot's contents, replace the bits taken up by the boolean, and then write
// back. This is the compiler's defense against contract upgrades and
// pointer aliasing, and it cannot be disabled.
```

[Refer Here](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27) Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past

### Instances:
[DataStorage.sol:L15](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L15)
```
core/contracts/libraries/DataStorage.sol:15:    bool initialized; // whether or not the timepoint is initialized
```
[AlgebraPool.sol:L293](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L293)
```
core/contracts/AlgebraPool.sol:293:    bool toggledBottom;
```
[AlgebraPool.sol:L294](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L294)
```
core/contracts/AlgebraPool.sol:294:    bool toggledTop;
```
[AlgebraPool.sol:L680](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L680)
```
core/contracts/AlgebraPool.sol:680:    bool computedLatestTimepoint;
```
[AlgebraPool.sol:L686](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L686)
```
core/contracts/AlgebraPool.sol:686:    bool exactInput; // Whether the exact input or output is specified
```
[AlgebraPool.sol:L695](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L695)
```
core/contracts/AlgebraPool.sol:695:    bool initialized; // True if the _nextTick is initialized
```
[PoolState.sol:L15](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol#L15)
```
core/contracts/base/PoolState.sol:15:    bool unlocked; // True if the contract is unlocked, otherwise - false
```
### References:

[https://code4rena.com/reports/2022-06-notional-coop#8-using-bools-for-storage-incurs-overhead](https://code4rena.com/reports/2022-06-notional-coop#8-using-bools-for-storage-incurs-overhead)


-----


## 12. Use a more recent version of solidity

Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

### Instances:
[AlgebraFactory.sol:L2](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L2)
```
core/contracts/AlgebraFactory.sol:2:pragma solidity =0.7.6;
```
[AlgebraPoolDeployer.sol:L2](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol#L2)
```
core/contracts/AlgebraPoolDeployer.sol:2:pragma solidity =0.7.6;
```
[TickTable.sol:L2](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L2)
```
core/contracts/libraries/TickTable.sol:2:pragma solidity =0.7.6;
```
[PriceMovementMath.sol:L2](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L2)
```
core/contracts/libraries/PriceMovementMath.sol:2:pragma solidity =0.7.6;
```
[Constants.sol:L2](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/Constants.sol#L2)
```
core/contracts/libraries/Constants.sol:2:pragma solidity =0.7.6;
```
[DataStorage.sol:L2](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L2)
```
core/contracts/libraries/DataStorage.sol:2:pragma solidity =0.7.6;
```
[TokenDeltaMath.sol:L2](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TokenDeltaMath.sol#L2)
```
core/contracts/libraries/TokenDeltaMath.sol:2:pragma solidity =0.7.6;
```
[AdaptiveFee.sol:L2](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L2)
```
core/contracts/libraries/AdaptiveFee.sol:2:pragma solidity =0.7.6;
```
[DataStorageOperator.sol:L2](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L2)
```
core/contracts/DataStorageOperator.sol:2:pragma solidity =0.7.6;
```
[AlgebraPool.sol:L2](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L2)
```
core/contracts/AlgebraPool.sol:2:pragma solidity =0.7.6;
```
[PoolImmutables.sol:L2](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolImmutables.sol#L2)
```
core/contracts/base/PoolImmutables.sol:2:pragma solidity =0.7.6;
```
[PoolState.sol:L2](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol#L2)
```
core/contracts/base/PoolState.sol:2:pragma solidity =0.7.6;
```

### References:

[https://code4rena.com/reports/2022-06-notional-coop/#10-use-a-more-recent-version-of-solidity](https://code4rena.com/reports/2022-06-notional-coop/#10-use-a-more-recent-version-of-solidity)


-----

## 13. Use assembly to check for address(0)

Saves 6 gas per instance if using assembly to check for address(0)

e.g.
```
assembly {
 if iszero(_addr) {
  mstore(0x00, 'zero address')
  revert(0x00, 0x20)
 }
}
```
### Links to Files
[AlgebraFactory.sol:L62](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L62)
```
core/contracts/AlgebraFactory.sol:62:    require(token0 != address(0));
```
[AlgebraPoolDeployer.sol:L37](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol#L37)
```
core/contracts/AlgebraPoolDeployer.sol:37:    require(_factory != address(0));
```
[AlgebraPool.sol:L752](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L752)

```
core/contracts/AlgebraPool.sol:752:      if (activeIncentive != address(0)) {
```

-----

## 14. Functions guaranteed to revert when called by normal users can be marked payable

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable
 will lower the gas cost for legitimate callers because the compiler 
will not include checks for whether a payment was provided. The extra 
opcodes avoided areCALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

### Instances:
```
core/contracts/AlgebraFactory.sol:77:  function setOwner(address _owner) external override onlyOwner {
core/contracts/AlgebraFactory.sol:84:  function setFarmingAddress(address _farmingAddress) external override onlyOwner {
core/contracts/AlgebraFactory.sol:91:  function setVaultAddress(address _vaultAddress) external override onlyOwner {
core/contracts/AlgebraPoolDeployer.sol:36:  function setFactory(address _factory) external override onlyOwner {
core/contracts/libraries/TickTable.sol:67:  /// @return initialized Whether the next tick is initialized, as the function only searches within up to 256 ticks
core/contracts/DataStorageOperator.sol:37:  function initialize(uint32 time, int24 tick) external override onlyPool {
core/contracts/AlgebraPool.sol:952:  function setCommunityFee(uint8 communityFee0, uint8 communityFee1) external override lock onlyFactoryOwner {
core/contracts/AlgebraPool.sol:967:  function setLiquidityCooldown(uint32 newLiquidityCooldown) external override onlyFactoryOwner {

```
### References:

[https://code4rena.com/reports/2022-04-pooltogether/#g-08-functions-guaranteed-to-revert-when-called-by-normal-users-can-be-marked-payable](https://code4rena.com/reports/2022-04-pooltogether/#g-08-functions-guaranteed-to-revert-when-called-by-normal-users-can-be-marked-payable)

----
# 15. Use Shift Right/Left instead of Division/Multiplication 2X
A division/multiplication by any number `x` being a power of 2 can be calculated by shifting `log2(x)` to the right/left.

While the `DIV` opcode uses 5 gas, the `SHR` opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.
### Links to github files
[AdaptiveFee.sol:L88](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L88)
### Instances
```
src/core/contracts/libraries/AdaptiveFee.sol:88:    res += (xLowestDegree * gHighestDegree) / 2;
```
### Recommended Mitigation Steps:
You should change multiplication and division by powers of two to bit shift.

----

# 16. Using private rather than public for constants / immutable, saves gas
### Description
If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that returns a tuple of the values of all currently-public constants. Saves **3406-3606** gas in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it’s used, and not adding another entry to the method ID table
### Links to github files
[AlgebraFactory.sol:L20](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L20)
[PoolImmutables.sol:L10](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolImmutables.sol#L10)
[PoolImmutables.sol:L13](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolImmutables.sol#L13)
[PoolImmutables.sol:L15](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolImmutables.sol#L15)
[PoolImmutables.sol:L17](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolImmutables.sol#L17)
[DataStorage.sol:L12](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L12)
### Instances
```
src/core/contracts/AlgebraFactory.sol:20:  address public immutable override poolDeployer;
src/core/contracts/base/PoolImmutables.sol:10:  address public immutable override dataStorageOperator;
src/core/contracts/base/PoolImmutables.sol:13:  address public immutable override factory;
src/core/contracts/base/PoolImmutables.sol:15:  address public immutable override token0;
src/core/contracts/base/PoolImmutables.sol:17:  address public immutable override token1;
src/core/contracts/libraries/DataStorage.sol:12:  uint32 public constant WINDOW = 1 days;
```

----
# 17. Using `calldata` instead of `memory` for read-only arguments in `external` functions saves gas
### Description
When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the `memory` index. **Each iteration of this for-loop costs at least 60 gas** (i.e. `60 * <mem_array>.length`). Using `calldata` directly, obliviates the need for such a loop in the contract code and runtime execution.

If the array is passed to an `internal` function which passes the array to another internal function where the array is modified and therefore `memory` is used in the `external` call, it’s still more gass-efficient to use `calldata` when the `external` function uses modifiers, since the modifiers may prevent the internal functions from being called. Structs have the same overhead as an array of length one
### Links to github files
[AlgebraPool.sol:L171](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L171)
### Instances
```
src/core/contracts/AlgebraPool.sol:171:  function getTimepoints(uint32[] calldata secondsAgos)
```
### Recommended Mitigation Steps
use `calldata` instead of `memory`
