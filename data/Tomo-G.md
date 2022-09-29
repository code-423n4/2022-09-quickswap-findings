# Gas Optimization


## ✅ G-1: Don't Initialize Variables with Default Value

### 📝 Description
Uninitialized variables are assigned with the types default value. Explicitly initializing a variable with it's default value costs unnecesary gas. Not overwriting the default for [stack variables](https://gist.github.com/IllIllI000/e075d189c1b23dce256cd166e28f3397) saves 8 gas. Storage and memory variables have larger savings

### 💡 Recommendation
Delete useless variable declarations to save gas.

### 🔍 Findings:
```2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307``` [for (uint256 i = 0; i < secondsAgos.length; i++) {](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307 )


## ✅ G-2: Cache Array Length Outside of Loop

### 📝 Description
Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.
You save 3 gas by not reading `array.length` - 3 gas per instance - 27 gas saved

### 💡 Recommendation


### 🔍 Findings:
```2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307``` [for (uint256 i = 0; i < secondsAgos.length; i++) {](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307 )


## ✅ G-3: Use != 0 instead of > 0 for Unsigned Integer Comparison

### 📝 Description
Use != 0 when comparing uint variables to zero, which cannot hold values below zero

### 💡 Recommendation
You should change from `> 0` to  `!=0`.

### 🔍 Findings:
```2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L111``` [require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L111 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L224``` [require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L224 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L228``` [if (_liquidityCooldown > 0) {](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L228 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L237``` [liquidityNext > 0 ? (liquidityDelta > 0 ? _blockTimestamp() : lastLiquidityAddTimestamp) : 0](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L237 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L434``` [require(liquidityDesired > 0, 'IL');](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L434 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L451``` [if (amount0 > 0) receivedAmount0 = balanceToken0();](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L451 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L452``` [if (amount1 > 0) receivedAmount1 = balanceToken1();](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L452 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L454``` [if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L454 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L455``` [if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L455 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L469``` [require(liquidityActual > 0, 'IIL2');](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L469 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L505``` [if (amount0 > 0) TransferHelper.safeTransfer(token0, recipient, amount0);](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L505 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L506``` [if (amount1 > 0) TransferHelper.safeTransfer(token1, recipient, amount1);](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L506 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L617``` [if (communityFee > 0) {](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L617 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L641``` [require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L641 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L645``` [require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L645 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L667``` [if (communityFee > 0) {](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L667 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L734``` [(cache.amountRequiredInitial, cache.exactInput) = (amountRequired, amountRequired > 0);](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L734 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L808``` [if (cache.communityFee > 0) {](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L808 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L814``` [if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L814 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L898``` [require(_liquidity > 0, 'L');](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L898 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L904``` [if (amount0 > 0) {](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L904 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L911``` [if (amount1 > 0) {](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L911 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L924``` [if (paid0 > 0) {](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L924 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L927``` [if (_communityFeeToken0 > 0) {](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L927 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L938``` [if (paid1 > 0) {](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L938 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L941``` [if (_communityFeeToken1 > 0) {](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L941 )

```2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L46``` [require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L46 )

```2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L138``` [if (volume >= 2**192) volumeShifted = (type(uint256).max) / (liquidity > 0 ? liquidity : 1);](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L138 )

```2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L139``` [else volumeShifted = (volume << 64) / (liquidity > 0 ? liquidity : 1);](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L139 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L80``` [last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L80 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L52``` [require(price > 0);](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L52 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L53``` [require(liquidity > 0);](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L53 )

## ✅ G-4: Use assembly to hash

### 📝 Description
See [this issue](https://github.com/code-423n4/2022-06-putty-findings/issues/59)
You can save about 5000 gas per instance in deploying contracrt.
You can save about 80 gas per instance if using assembly to execute the function.

### 💡 Recommendation
Please follow [this link](https://gist.github.com/Tomosuke0930/72beb469f08c954b232e58b8da03ef28) to make corrections

### 🔍 Findings:
```2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L124``` [pool = address(uint256(keccak256(abi.encodePacked(hex'ff', poolDeployer, keccak256(abi.encode(token0, token1)), POOL_INIT_CODE_HASH))));](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L124 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L51``` [pool = address(new AlgebraPool{salt: keccak256(abi.encode(token0, token1))}());](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L51 )

## ✅ G-5: Solidity Version should be the latest

### 📝 Description
Use a solidity version of at least 0.8.0 to get overflow protection without SafeMath
Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value 


### 💡 Recommendation
You should consider with your team members whether you should choose the latest version.
The latest version has its advantages, but also it has disadvantages, such as the possibility of unknown bugs.

### 🔍 Findings:
```2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/base/PoolImmutables.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolImmutables.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/base/PoolState.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolState.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/Constants.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/Constants.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L2 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/TokenDeltaMath.sol#L2``` [pragma solidity =0.7.6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TokenDeltaMath.sol#L2 )

## ✅ G-6: Splitting require() statements that use && saves gas

### 📝 Description
See [this issue](https://github.com/code-423n4/2022-01-xdefi-findings/issues/128) which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper

### 💡 Recommendation
You should change one require which has `&&` to two simple require() statements to save gas

### 🔍 Findings:
```2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L111``` [require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L111 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L739``` [require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L739 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L743``` [require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L743 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L953``` [require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L953 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L968``` [require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L968 )

```2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L46``` [require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L46 )


## ✅ G-7: Use `++i` instead of `i++`

### 📝 Description
You can get cheaper for loops (at least 25 gas, however can be up to 80 gas under certain conditions), by rewriting
The post-increment operation (i++) boils down to saving the original value of i, incrementing it and saving that to a temporary place in memory, and then returning the original value; only after that value is returned is the value of i actually updated (4 operations).On the other hand, in pre-increment doesn't need to store temporary(2 operations) so, the pre-increment operation uses less opcodes and is thus more gas efficient.

### 💡 Recommendation
You should change from i++ to ++i.

### 🔍 Findings:
```2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307``` [for (uint256 i = 0; i < secondsAgos.length; i++) {](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307 )


## ✅ G-8: Use x=x+y instad of x+=y

### 📝 Description
You can save about 35 gas per instance if you change from `x+=y**`** to `x=x+y`
This works equally well for subtraction, multiplication and division.

### 💡 Recommendation
You should change from `x+=y` to `x=x+y`.

### 🔍 Findings:
```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L257``` [_position.fees0 += fees0;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L257 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L258``` [_position.fees1 += fees1;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L258 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L801``` [amountRequired -= (step.input + step.feeAmount).toInt256(); // decrease remaining input amount](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L801 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L804``` [amountRequired += step.output.toInt256(); // increase remaining output amount (since its negative)](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L804 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L810``` [step.feeAmount -= delta;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L810 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L811``` [communityFeeAmount += delta;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L811 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L814``` [if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L814 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L922``` [paid0 -= balance0Before;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L922 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L931``` [totalFeeGrowth0Token += FullMath.mulDiv(paid0 - fees0, Constants.Q128, _liquidity);](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L931 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L936``` [paid1 -= balance1Before;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L936 )

```2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L945``` [totalFeeGrowth1Token += FullMath.mulDiv(paid1 - fees1, Constants.Q128, _liquidity);](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L945 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L83``` [gHighestDegree /= g; // g**7](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L83 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L84``` [res += xLowestDegree * gHighestDegree;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L84 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L86``` [gHighestDegree /= g; // g**6](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L86 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L87``` [xLowestDegree *= x; // x**2](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L87 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L88``` [res += (xLowestDegree * gHighestDegree) / 2;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L88 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L90``` [gHighestDegree /= g; // g**5](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L90 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L91``` [xLowestDegree *= x; // x**3](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L91 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L92``` [res += (xLowestDegree * gHighestDegree) / 6;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L92 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L94``` [gHighestDegree /= g; // g**4](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L94 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L95``` [xLowestDegree *= x; // x**4](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L95 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L96``` [res += (xLowestDegree * gHighestDegree) / 24;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L96 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L98``` [gHighestDegree /= g; // g**3](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L98 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L99``` [xLowestDegree *= x; // x**5](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L99 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L100``` [res += (xLowestDegree * gHighestDegree) / 120;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L100 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L102``` [gHighestDegree /= g; // g**2](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L102 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L103``` [xLowestDegree *= x; // x**6](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L103 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L104``` [res += (xLowestDegree * gHighestDegree) / 720;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L104 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L106``` [xLowestDegree *= x; // x**7](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L106 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L107``` [res += (xLowestDegree * g) / 5040 + (xLowestDegree * x) / (40320);](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L107 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L79``` [last.tickCumulative += int56(tick) * delta;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L79 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L80``` [last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L80 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L81``` [last.volatilityCumulative += uint88(_volatilityOnRange(delta, prevTick, tick, last.averageTick, averageTick)); // always fits 88 bits](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L81 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L83``` [last.volumePerLiquidityCumulative += volumePerLiquidity;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L83 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L119``` [index -= 1; // considering underflow](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L119 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L251``` [beforeOrAt.tickCumulative += ((atOrAfter.tickCumulative - beforeOrAt.tickCumulative) / timepointTimeDelta) * targetDelta;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L251 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L252``` [beforeOrAt.secondsPerLiquidityCumulative += uint160(](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L252 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L255``` [beforeOrAt.volatilityCumulative += ((atOrAfter.volatilityCumulative - beforeOrAt.volatilityCumulative) / timepointTimeDelta) * targetDelta;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L255 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L256``` [beforeOrAt.volumePerLiquidityCumulative +=](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L256 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L58``` [innerFeeGrowth0Token -= upper.outerFeeGrowth0Token;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L58 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L59``` [innerFeeGrowth1Token -= upper.outerFeeGrowth1Token;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L59 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L16``` [tick /= Constants.TICK_SPACING; // compress tick](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L16 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L92``` [tick -= int24(255 - getMostSignificantBit(_row));](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L92 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L95``` [tick -= int24(bitNumber);](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L95 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L100``` [tick += 1;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L100 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L112``` [tick += int24(getSingleSignificantBit(-_row & _row)); // least significant bit](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L112 )

```2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L115``` [tick += int24(255 - bitNumber);](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L115 )

