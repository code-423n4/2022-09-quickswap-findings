# 1. Use a more recent version of solidity 
### Description 
Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than `revert()/require()` strings Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value 
### Links to github files
[AlgebraPool.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L2)
[DataStorageOperator.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L2)
[AlgebraFactory.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L2)
[PoolImmutables.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolImmutables.sol#L2)
[PoolState.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolState.sol#L2)
[AlgebraPoolDeployer.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L2)
[TokenDeltaMath.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TokenDeltaMath.sol#L2)
[Constants.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/Constants.sol#L2)
[DataStorage.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L2)
[AdaptiveFee.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L2)
[TickManager.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickManager.sol#L2)
[TickTable.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickTable.sol#L2)
[PriceMovementMath.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L2)
### Instances 
``` 
src/core/contracts/AlgebraPool.sol:2:pragma solidity =0.7.6;
src/core/contracts/DataStorageOperator.sol:2:pragma solidity =0.7.6;
src/core/contracts/AlgebraFactory.sol:2:pragma solidity =0.7.6;
src/core/contracts/base/PoolImmutables.sol:2:pragma solidity =0.7.6;
src/core/contracts/base/PoolState.sol:2:pragma solidity =0.7.6;
src/core/contracts/AlgebraPoolDeployer.sol:2:pragma solidity =0.7.6;
src/core/contracts/libraries/TokenDeltaMath.sol:2:pragma solidity =0.7.6;
src/core/contracts/libraries/Constants.sol:2:pragma solidity =0.7.6;
src/core/contracts/libraries/DataStorage.sol:2:pragma solidity =0.7.6;
src/core/contracts/libraries/AdaptiveFee.sol:2:pragma solidity =0.7.6;
src/core/contracts/libraries/TickManager.sol:2:pragma solidity =0.7.6;
src/core/contracts/libraries/TickTable.sol:2:pragma solidity =0.7.6;
src/core/contracts/libraries/PriceMovementMath.sol:2:pragma solidity =0.7.6;
``` 
---- 
# 2. Using > 0 costs more gas than != 0 when used on a uint in a require() statement 
### Description 
0 is less efficient than != 0 for unsigned integers (with proof) 
!= 0 costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled (6 gas) Proof: While it may seem that > 0 is cheaper than !=, this is only true without the optimizer enabled and outside a require statement. If you enable the optimizer at 10k AND you’re in a require statement, this will save gas. You can see this tweet for more proof: https://twitter.com/gzeon/status/1485428085885640706 
### Links to github file 
[AlgebraPool.sol:L224](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L224)
[AlgebraPool.sol:L434](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L434)
[AlgebraPool.sol:L469](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L469)
[AlgebraPool.sol:L641](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L641)
[AlgebraPool.sol:L645](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L645)
[AlgebraPool.sol:L898](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L898)
[PriceMovementMath.sol:L52](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L52)
[PriceMovementMath.sol:L53](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L53)
### Instances 
``` 
src/core/contracts/AlgebraPool.sol:224:      require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges
src/core/contracts/AlgebraPool.sol:434:    require(liquidityDesired > 0, 'IL');
src/core/contracts/AlgebraPool.sol:454:      if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');
src/core/contracts/AlgebraPool.sol:455:      if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');
src/core/contracts/AlgebraPool.sol:469:    require(liquidityActual > 0, 'IIL2');
src/core/contracts/AlgebraPool.sol:641:      require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');
src/core/contracts/AlgebraPool.sol:645:      require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');
src/core/contracts/AlgebraPool.sol:898:    require(_liquidity > 0, 'L');
src/core/contracts/libraries/PriceMovementMath.sol:52:    require(price > 0);
src/core/contracts/libraries/PriceMovementMath.sol:53:    require(liquidity > 0);
``` 
### Recommended Mitigation Steps: 
I suggest changing > 0 with != 0. Also, please enable the Optimizer. 

---- 
# 3. ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, as is the case when used in for- and while-loops
### Description
The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas [PER LOOP](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked)
### Links to github files
[DataStorage.sol:L307](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L307)
### Instances
```
src/core/contracts/libraries/DataStorage.sol:307:    for (uint256 i = 0; i < secondsAgos.length; i++) {
```
----
# 4. IT COSTS MORE GAS TO INITIALIZE VARIABLES WITH THEIR DEFAULT VALUE THAN LETTING THE DEFAULT VALUE BE APPLIED
### Description
If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: for `(uint256 i = 0; i < numIterations; ++i)` { should be replaced with for `(uint256 i; i < numIterations; ++i) {`
### Links to github files
[DataStorage.sol:L307](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L307)
### Instances
```
src/core/contracts/libraries/DataStorage.sol:307:    for (uint256 i = 0; i < secondsAgos.length; i++) {
```
----
# 5. ++I COSTS LESS GAS COMPARED TO I++ OR I += 1
*Pre-increments and pre-decrements are cheaper.*

For a `uint256` i variable, the following is true with the Optimizer enabled at 10k:
Increment:
`i += 1` is the most expensive form
`i++` costs `6` `gas` less than `i += 1`
`++i` costs `5 gas` less than `i++` (11 gas less than i += 1)

Note that post-increments (or post-decrements) return the old value before incrementing or decrementing, hence the name post-increment:
### Links to github files
[TickTable.sol:L100](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickTable.sol#L100)
[DataStorage.sol:L307](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L307)
### Instances
```
src/core/contracts/libraries/TickTable.sol:100:      tick += 1;
src/core/contracts/libraries/DataStorage.sol:307:    for (uint256 i = 0; i < secondsAgos.length; i++) {
```
Similarly
`i -= 1` is the most expensive form `i--`
`i--` is more expennsive than `--i`
### Links to github files
[DataStorage.sol:L119](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L119)
### Instances
```
src/core/contracts/libraries/DataStorage.sol:119:        index -= 1; // considering underflow
```
----
# 6. Usage of assert() instead of require()
### Description
Between solc 0.4.10 and 0.8.0, require() used REVERT (0xfd) opcode which refunded the remaining gas on failure while assert() used INVALID (0xfe) opcode which consumed all the supplied gas. (see [https://docs.soliditylang.org/en/v0.8.1/control-structures.html#error-handling-assert-require-revert-and-exceptions](https://docs.soliditylang.org/en/v0.8.1/control-structures.html#error-handling-assert-require-revert-and-exceptions)).
require() should be used for checking error conditions on inputs and return values while assert() should be used for invariant checking (properly functioning code should never reach a failing assert statement, unless there is a bug in your contract you should fix).
From the current usage of assert, my guess is that they can be replaced with require unless a Panic really is intended.
### Links to github file
[DataStorage.sol:L190](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L190)
### Instance
```
src/core/contracts/libraries/DataStorage.sol:190:    assert(false);
```
----
# 7. Splitting require() statements that use && saves gas
### Description
Require statements including conditions with the && operator can be broken down in multiple require statements to save gas.
### Links to github file
[AlgebraPool.sol:L739](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L739)
[AlgebraPool.sol:L743](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L743)
[AlgebraPool.sol:L953](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L953)
[AlgebraPool.sol:L968](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L968)
[DataStorageOperator.sol:L46](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L46)
[AlgebraFactory.sol:L110](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L110)
### Instance
```
src/core/contracts/AlgebraPool.sol:739:        require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
src/core/contracts/AlgebraPool.sol:743:        require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
src/core/contracts/AlgebraPool.sol:953:    require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));
src/core/contracts/AlgebraPool.sol:968:    require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
src/core/contracts/DataStorageOperator.sol:46:    require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
src/core/contracts/AlgebraFactory.sol:110:    require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
```
### Recommended Mitigation Steps:
Breakdown each condition in a separate require statement (though require statements should be replaced with custom errors)

----
# 8. Use Shift Right/Left instead of Division/Multiplication 2X
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
# 9. `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables
`<x> += <y>` costs more gas as compared to `<x> = <x> + <y>`
### Links to github files
[AlgebraPool.sol:L257](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L257)
[AlgebraPool.sol:L258](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L258)
[AlgebraPool.sol:L804](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L804)
[AlgebraPool.sol:L811](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L811)
[AlgebraPool.sol:L814](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L814)
[AlgebraPool.sol:L931](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L931)
[AlgebraPool.sol:L945](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L945)
[DataStorage.sol:L79](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L79)
[DataStorage.sol:L80](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L80)
[DataStorage.sol:L81](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L81)
[DataStorage.sol:L83](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L83)
[DataStorage.sol:L251](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L251)
[DataStorage.sol:L252](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L252)
[DataStorage.sol:L255](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L255)
[DataStorage.sol:L256](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L256)
[AdaptiveFee.sol:L84](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L84)
[AdaptiveFee.sol:L88](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L88)
[AdaptiveFee.sol:L92](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L92)
[AdaptiveFee.sol:L96](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L96)
[AdaptiveFee.sol:L100](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L100)
[AdaptiveFee.sol:L104](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L104)
[AdaptiveFee.sol:L107](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L107)
[TickTable.sol:L100](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickTable.sol#L100)
[TickTable.sol:L112](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickTable.sol#L112)
[TickTable.sol:L115](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickTable.sol#L115)
### Instances
```
src/core/contracts/AlgebraPool.sol:257:      _position.fees0 += fees0;
src/core/contracts/AlgebraPool.sol:258:      _position.fees1 += fees1;
src/core/contracts/AlgebraPool.sol:804:        amountRequired += step.output.toInt256(); // increase remaining output amount (since its negative)
src/core/contracts/AlgebraPool.sol:811:        communityFeeAmount += delta;
src/core/contracts/AlgebraPool.sol:814:      if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);
src/core/contracts/AlgebraPool.sol:931:      totalFeeGrowth0Token += FullMath.mulDiv(paid0 - fees0, Constants.Q128, _liquidity);
src/core/contracts/AlgebraPool.sol:945:      totalFeeGrowth1Token += FullMath.mulDiv(paid1 - fees1, Constants.Q128, _liquidity);
src/core/contracts/libraries/DataStorage.sol:79:    last.tickCumulative += int56(tick) * delta;
src/core/contracts/libraries/DataStorage.sol:80:    last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0
src/core/contracts/libraries/DataStorage.sol:81:    last.volatilityCumulative += uint88(_volatilityOnRange(delta, prevTick, tick, last.averageTick, averageTick)); // always fits 88 bits
src/core/contracts/libraries/DataStorage.sol:83:    last.volumePerLiquidityCumulative += volumePerLiquidity;
src/core/contracts/libraries/DataStorage.sol:251:      beforeOrAt.tickCumulative += ((atOrAfter.tickCumulative - beforeOrAt.tickCumulative) / timepointTimeDelta) * targetDelta;
src/core/contracts/libraries/DataStorage.sol:252:      beforeOrAt.secondsPerLiquidityCumulative += uint160(
src/core/contracts/libraries/DataStorage.sol:255:      beforeOrAt.volatilityCumulative += ((atOrAfter.volatilityCumulative - beforeOrAt.volatilityCumulative) / timepointTimeDelta) * targetDelta;
src/core/contracts/libraries/DataStorage.sol:256:      beforeOrAt.volumePerLiquidityCumulative +=
src/core/contracts/libraries/AdaptiveFee.sol:84:    res += xLowestDegree * gHighestDegree;
src/core/contracts/libraries/AdaptiveFee.sol:88:    res += (xLowestDegree * gHighestDegree) / 2;
src/core/contracts/libraries/AdaptiveFee.sol:92:    res += (xLowestDegree * gHighestDegree) / 6;
src/core/contracts/libraries/AdaptiveFee.sol:96:    res += (xLowestDegree * gHighestDegree) / 24;
src/core/contracts/libraries/AdaptiveFee.sol:100:    res += (xLowestDegree * gHighestDegree) / 120;
src/core/contracts/libraries/AdaptiveFee.sol:104:    res += (xLowestDegree * gHighestDegree) / 720;
src/core/contracts/libraries/AdaptiveFee.sol:107:    res += (xLowestDegree * g) / 5040 + (xLowestDegree * x) / (40320);
src/core/contracts/libraries/TickTable.sol:100:      tick += 1;
src/core/contracts/libraries/TickTable.sol:112:        tick += int24(getSingleSignificantBit(-_row & _row)); // least significant bit
src/core/contracts/libraries/TickTable.sol:115:        tick += int24(255 - bitNumber);
```
similarly `<x> -= <y>` costs more gas as compared to `<x> = <x> - <y>`
### Links to github files
[AlgebraPool.sol:L801](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L801)
[AlgebraPool.sol:L810](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L810)
[AlgebraPool.sol:L922](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L922)
[AlgebraPool.sol:L936](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L936)
[DataStorage.sol:L119](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L119)
[TickManager.sol:L58](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickManager.sol#L58)
[TickManager.sol:L59](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickManager.sol#L59)
[TickTable.sol:L92](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickTable.sol#L92)
[TickTable.sol:L95](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickTable.sol#L95)
### Instances
```
src/core/contracts/AlgebraPool.sol:801:        amountRequired -= (step.input + step.feeAmount).toInt256(); // decrease remaining input amount
src/core/contracts/AlgebraPool.sol:810:        step.feeAmount -= delta;
src/core/contracts/AlgebraPool.sol:922:    paid0 -= balance0Before;
src/core/contracts/AlgebraPool.sol:936:    paid1 -= balance1Before;
src/core/contracts/libraries/DataStorage.sol:119:        index -= 1; // considering underflow
src/core/contracts/libraries/TickManager.sol:58:      innerFeeGrowth0Token -= upper.outerFeeGrowth0Token;
src/core/contracts/libraries/TickManager.sol:59:      innerFeeGrowth1Token -= upper.outerFeeGrowth1Token;
src/core/contracts/libraries/TickTable.sol:92:        tick -= int24(255 - getMostSignificantBit(_row));
src/core/contracts/libraries/TickTable.sol:95:        tick -= int24(bitNumber);
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
----
# 10.`<ARRAY>.LENGTH` SHOULD NOT BE LOOKED UP IN EVERY LOOP OF A FOR-LOOP
### Description
The overheads outlined below are `PER LOOP`, excluding the first loop
storage arrays incur a Gwarmaccess (100 gas) memory arrays use `MLOAD` (3 gas)
calldata arrays use `CALLDATALOAD` (3 gas) Caching the length changes each of these to a `DUP<N>` (3 gas), and gets rid of the extra `DUP<N>` needed to store the stack offset
### Links to github files
[DataStorage.sol:L307](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L307)
### Instances
```
src/core/contracts/libraries/DataStorage.sol:307:    for (uint256 i = 0; i < secondsAgos.length; i++) {
```
----
# 11. Use `abi.encodePacked()` not `abi.encode()`
Changing `abi.encode` to `abi.encodePacked` can save gas. `abi.encode` pads extra null bytes at the end of the call data which is normally unnecessary. In general, `abi.encodePacked` is more gas-efficient.
### Links to github files
[AlgebraFactory.sol:L123](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L123)
[AlgebraPoolDeployer.sol:L51](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L51)
### Instances
```
src/core/contracts/AlgebraFactory.sol:123:    pool = address(uint256(keccak256(abi.encodePacked(hex'ff', poolDeployer, keccak256(abi.encode(token0, token1)), POOL_INIT_CODE_HASH))));
src/core/contracts/AlgebraPoolDeployer.sol:51:    pool = address(new AlgebraPool{salt: keccak256(abi.encode(token0, token1))}());
```

### Recommended Mitigation Steps
You should change **abi.encode** to **abi.encodePacked**

----
# 12. Using private rather than public for constants / immutable, saves gas
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
# 13. Using `calldata` instead of `memory` for read-only arguments in `external` functions saves gas
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

----
# 14. Custom Errors instead of Revert Strings to save Gas
### Description
Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met)

Starting from Solidity v0.8.4,there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., revert("Insufficient funds.");),but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.

Custom errors are defined using the error statement, which can be used inside and outside of contracts (including interfaces and libraries).
### Links to github files
[AlgebraPool.sol:L60](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L60)
[AlgebraPool.sol:L61](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L61)
[AlgebraPool.sol:L62](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L62)
[AlgebraPool.sol:L194](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L194)
[AlgebraPool.sol:L224](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L224)
[AlgebraPool.sol:L434](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L434)
[AlgebraPool.sol:L454](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L454)
[AlgebraPool.sol:L455](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L455)
[AlgebraPool.sol:L469](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L469)
[AlgebraPool.sol:L474](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L474)
[AlgebraPool.sol:L475](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L475)
[AlgebraPool.sol:L608](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L608)
[AlgebraPool.sol:L614](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L614)
[AlgebraPool.sol:L636](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L636)
[AlgebraPool.sol:L641](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L641)
[AlgebraPool.sol:L645](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L645)
[AlgebraPool.sol:L731](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L731)
[AlgebraPool.sol:L733](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L733)
[AlgebraPool.sol:L739](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L739)
[AlgebraPool.sol:L743](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L743)
[AlgebraPool.sol:L898](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L898)
[AlgebraPool.sol:L921](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L921)
[AlgebraPool.sol:L935](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L935)
[DataStorageOperator.sol:L27](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L27)
[DataStorageOperator.sol:L45](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L45)
[DataStorageOperator.sol:L46](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L46)
[AlgebraFactory.sol:L109](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L109)
[AlgebraFactory.sol:L110](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L110)
[PoolState.sol:L41](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolState.sol#L41)
[DataStorage.sol:L238](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L238)
[TickManager.sol:L96](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickManager.sol#L96)
[TickTable.sol:L15](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickTable.sol#L15)
### Instances
```
src/core/contracts/AlgebraPool.sol:60:    require(topTick < TickMath.MAX_TICK + 1, 'TUM');
src/core/contracts/AlgebraPool.sol:61:    require(topTick > bottomTick, 'TLU');
src/core/contracts/AlgebraPool.sol:62:    require(bottomTick > TickMath.MIN_TICK - 1, 'TLM');
src/core/contracts/AlgebraPool.sol:194:    require(globalState.price == 0, 'AI');
src/core/contracts/AlgebraPool.sol:224:      require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges
src/core/contracts/AlgebraPool.sol:434:    require(liquidityDesired > 0, 'IL');
src/core/contracts/AlgebraPool.sol:454:      if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');
src/core/contracts/AlgebraPool.sol:455:      if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');
src/core/contracts/AlgebraPool.sol:469:    require(liquidityActual > 0, 'IIL2');
src/core/contracts/AlgebraPool.sol:474:      require((amount0 = uint256(amount0Int)) <= receivedAmount0, 'IIAM2');
src/core/contracts/AlgebraPool.sol:475:      require((amount1 = uint256(amount1Int)) <= receivedAmount1, 'IIAM2');
src/core/contracts/AlgebraPool.sol:608:      require(balance0Before.add(uint256(amount0)) <= balanceToken0(), 'IIA');
src/core/contracts/AlgebraPool.sol:614:      require(balance1Before.add(uint256(amount1)) <= balanceToken1(), 'IIA');
src/core/contracts/AlgebraPool.sol:636:    require(globalState.unlocked, 'LOK');
src/core/contracts/AlgebraPool.sol:641:      require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');
src/core/contracts/AlgebraPool.sol:645:      require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');
src/core/contracts/AlgebraPool.sol:731:      require(unlocked, 'LOK');
src/core/contracts/AlgebraPool.sol:733:      require(amountRequired != 0, 'AS');
src/core/contracts/AlgebraPool.sol:739:        require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
src/core/contracts/AlgebraPool.sol:743:        require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
src/core/contracts/AlgebraPool.sol:898:    require(_liquidity > 0, 'L');
src/core/contracts/AlgebraPool.sol:921:    require(balance0Before.add(fee0) <= paid0, 'F0');
src/core/contracts/AlgebraPool.sol:935:    require(balance1Before.add(fee1) <= paid1, 'F1');;
src/core/contracts/DataStorageOperator.sol:27:    require(msg.sender == pool, 'only pool can call this');
src/core/contracts/DataStorageOperator.sol:45:    require(uint256(_feeConfig.alpha1) + uint256(_feeConfig.alpha2) + uint256(_feeConfig.baseFee) <= type(uint16).max, 'Max fee exceeded');
src/core/contracts/DataStorageOperator.sol:46:    require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
src/core/contracts/AlgebraFactory.sol:109:    require(uint256(alpha1) + uint256(alpha2) + uint256(baseFee) <= type(uint16).max, 'Max fee exceeded');
src/core/contracts/AlgebraFactory.sol:110:    require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
src/core/contracts/base/PoolState.sol:41:    require(globalState.unlocked, 'LOK');
src/core/contracts/libraries/DataStorage.sol:238:    require(lteConsideringOverflow(self[oldestIndex].blockTimestamp, target, time), 'OLD');
src/core/contracts/libraries/TickManager.sol:96:    require(liquidityTotalAfter < Constants.MAX_LIQUIDITY_PER_TICK + 1, 'LO');
src/core/contracts/libraries/TickTable.sol:15:    require(tick % Constants.TICK_SPACING == 0, 'tick is not spaced'); // ensure that the tick is spaced
```
----
# 15. Strict inequalities (>) are more expensive than non-strict ones (>=)
### Description
Strict inequalities (>) are more expensive than non-strict ones (>=). This is due to some supplementary checks (ISZERO, 3 gas. I suggest using >= instead of > to avoid some opcodes here:
### Link To Github Files
[AlgebraPool.sol:L61](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L61)
[AlgebraPool.sol:L62](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L62)
[AlgebraPool.sol:L228](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L228)
[AlgebraPool.sol:L237](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L237)
[AlgebraPool.sol:L451](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L451)
[AlgebraPool.sol:L452](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L452)
[AlgebraPool.sol:L454](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L454)
[AlgebraPool.sol:L455](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L455)
[AlgebraPool.sol:L478](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L478)
[AlgebraPool.sol:L481](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L481)
[AlgebraPool.sol:L498](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L498)
[AlgebraPool.sol:L499](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L499)
[AlgebraPool.sol:L505](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L505)
[AlgebraPool.sol:L506](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L506)
[AlgebraPool.sol:L617](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L617)
[AlgebraPool.sol:L667](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L667)
[AlgebraPool.sol:L734](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L734)
[AlgebraPool.sol:L739](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L739)
[AlgebraPool.sol:L743](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L743)
[AlgebraPool.sol:L808](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L808)
[AlgebraPool.sol:L814](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L814)
[AlgebraPool.sol:L904](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L904)
[AlgebraPool.sol:L911](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L911)
[AlgebraPool.sol:L924](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L924)
[AlgebraPool.sol:L927](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L927)
[AlgebraPool.sol:L938](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L938)
[AlgebraPool.sol:L941](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L941)
[DataStorageOperator.sol:L138](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L138)
[DataStorageOperator.sol:L139](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L139)
[DataStorage.sol:L80](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L80)
[DataStorage.sol:L99](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L99)
[DataStorage.sol:L100](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L100)
[AdaptiveFee.sol:L33](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L33)
[AdaptiveFee.sol:L50](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L50)
[TickTable.sol:L125](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickTable.sol#L125)
[PriceMovementMath.sol:L71](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L71)
[PriceMovementMath.sol:L87](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L87)
[PriceMovementMath.sol:L189](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L189)
### Instances
```
src/core/contracts/AlgebraPool.sol:61:    require(topTick > bottomTick, 'TLU');
src/core/contracts/AlgebraPool.sol:62:    require(bottomTick > TickMath.MIN_TICK - 1, 'TLM');
src/core/contracts/AlgebraPool.sol:228:        if (_liquidityCooldown > 0) {
src/core/contracts/AlgebraPool.sol:237:        liquidityNext > 0 ? (liquidityDelta > 0 ? _blockTimestamp() : lastLiquidityAddTimestamp) : 0
src/core/contracts/AlgebraPool.sol:451:      if (amount0 > 0) receivedAmount0 = balanceToken0();
src/core/contracts/AlgebraPool.sol:452:      if (amount1 > 0) receivedAmount1 = balanceToken1();
src/core/contracts/AlgebraPool.sol:454:      if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');
src/core/contracts/AlgebraPool.sol:455:      if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');
src/core/contracts/AlgebraPool.sol:478:    if (receivedAmount0 > amount0) {
src/core/contracts/AlgebraPool.sol:481:    if (receivedAmount1 > amount1) {
src/core/contracts/AlgebraPool.sol:498:    amount0 = amount0Requested > positionFees0 ? positionFees0 : amount0Requested;
src/core/contracts/AlgebraPool.sol:499:    amount1 = amount1Requested > positionFees1 ? positionFees1 : amount1Requested;
src/core/contracts/AlgebraPool.sol:505:      if (amount0 > 0) TransferHelper.safeTransfer(token0, recipient, amount0);
src/core/contracts/AlgebraPool.sol:506:      if (amount1 > 0) TransferHelper.safeTransfer(token1, recipient, amount1);
src/core/contracts/AlgebraPool.sol:617:    if (communityFee > 0) {
src/core/contracts/AlgebraPool.sol:667:    if (communityFee > 0) {
src/core/contracts/AlgebraPool.sol:734:      (cache.amountRequiredInitial, cache.exactInput) = (amountRequired, amountRequired > 0);
src/core/contracts/AlgebraPool.sol:739:        require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
src/core/contracts/AlgebraPool.sol:743:        require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
src/core/contracts/AlgebraPool.sol:808:      if (cache.communityFee > 0) {
src/core/contracts/AlgebraPool.sol:814:      if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);
src/core/contracts/AlgebraPool.sol:904:    if (amount0 > 0) {
src/core/contracts/AlgebraPool.sol:911:    if (amount1 > 0) {
src/core/contracts/AlgebraPool.sol:924:    if (paid0 > 0) {
src/core/contracts/AlgebraPool.sol:927:      if (_communityFeeToken0 > 0) {
src/core/contracts/AlgebraPool.sol:938:    if (paid1 > 0) {
src/core/contracts/AlgebraPool.sol:941:      if (_communityFeeToken1 > 0) {
src/core/contracts/DataStorageOperator.sol:138:    if (volume >= 2**192) volumeShifted = (type(uint256).max) / (liquidity > 0 ? liquidity : 1);
src/core/contracts/DataStorageOperator.sol:139:    else volumeShifted = (volume << 64) / (liquidity > 0 ? liquidity : 1);
src/core/contracts/libraries/DataStorage.sol:80:    last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0
src/core/contracts/libraries/DataStorage.sol:99:    res = a > currentTime;
src/core/contracts/libraries/DataStorage.sol:100:    if (res == b > currentTime) res = a <= b; // if both are on the same side
src/core/contracts/libraries/AdaptiveFee.sol:33:    if (sumOfSigmoids > type(uint16).max) {
src/core/contracts/libraries/AdaptiveFee.sol:50:    if (x > beta) {
src/core/contracts/libraries/TickTable.sol:125:    } else if (boundedTick > TickMath.MAX_TICK) {
src/core/contracts/libraries/PriceMovementMath.sol:71:        require(liquidityShifted > product); // in addition, we must check that the denominator does not underflow
src/core/contracts/libraries/PriceMovementMath.sol:87:        require(price > quotient);
src/core/contracts/libraries/PriceMovementMath.sol:189:        if (output > uint256(amountAvailable)) {
```
----