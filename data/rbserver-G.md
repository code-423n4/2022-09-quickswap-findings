## [G-01] STATE VARIABLES THAT ARE ACCESSED FOR MULTIPLE TIMES CAN BE CACHED IN MEMORY
A state variable that is accessed for multiple times can be cached in memory, and the cached variable value can then be used. This can replace multiple `sload` operations with one `sload`, one `mstore`, and multiple `mload` operations to save gas.

In the following `swap` function, `token0` and `token1` can be cached, and the cached values can then be used.
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L589-L623
```solidity
  function swap(
    address recipient,
    bool zeroToOne,
    int256 amountRequired,
    uint160 limitSqrtPrice,
    bytes calldata data
  ) external override returns (int256 amount0, int256 amount1) {
    ...

    if (zeroToOne) {
      if (amount1 < 0) TransferHelper.safeTransfer(token1, recipient, uint256(-amount1)); // transfer to recipient

      uint256 balance0Before = balanceToken0();
      _swapCallback(amount0, amount1, data); // callback to get tokens from the caller
      require(balance0Before.add(uint256(amount0)) <= balanceToken0(), 'IIA');
    } else {
      if (amount0 < 0) TransferHelper.safeTransfer(token0, recipient, uint256(-amount0)); // transfer to recipient

      uint256 balance1Before = balanceToken1();
      _swapCallback(amount0, amount1, data); // callback to get tokens from the caller
      require(balance1Before.add(uint256(amount1)) <= balanceToken1(), 'IIA');
    }

    if (communityFee > 0) {
      _payCommunityFee(zeroToOne ? token0 : token1, communityFee);
    }

    ...
  }
```

In the following `swapSupportingFeeOnInputTokens` function, `token0` and `token1` can be cached, and the cached values can then be used.
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L626-L673
```solidity
function swapSupportingFeeOnInputTokens(
    address sender,
    address recipient,
    bool zeroToOne,
    int256 amountRequired,
    uint160 limitSqrtPrice,
    bytes calldata data
  ) external override returns (int256 amount0, int256 amount1) {
    ...

    // only transfer to the recipient
    if (zeroToOne) {
      if (amount1 < 0) TransferHelper.safeTransfer(token1, recipient, uint256(-amount1));
      // return the leftovers
      if (amount0 < amountRequired) TransferHelper.safeTransfer(token0, sender, uint256(amountRequired.sub(amount0)));
    } else {
      if (amount0 < 0) TransferHelper.safeTransfer(token0, recipient, uint256(-amount0));
      // return the leftovers
      if (amount1 < amountRequired) TransferHelper.safeTransfer(token1, sender, uint256(amountRequired.sub(amount1)));
    }

    if (communityFee > 0) {
      _payCommunityFee(zeroToOne ? token0 : token1, communityFee);
    }

    ...
  }
```

In the following `_calculateSwapAndLock` function, `activeIncentive` can be cached, and the cached value can then be used.
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L703-L888
```solidity
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
    ...
    {
      ...

      if (activeIncentive != address(0)) {
        IAlgebraVirtualPool.Status _status = IAlgebraVirtualPool(activeIncentive).increaseCumulative(blockTimestamp);
        ...
      }

      ...
    }

    ...
  }
```

In the following `flash` function, `token0` and `token1` can be cached, and the cached values can then be used.
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L891-L949
```solidity
  function flash(
    address recipient,
    uint256 amount0,
    uint256 amount1,
    bytes calldata data
  ) external override lock {
    ...
    if (amount0 > 0) {
      fee0 = FullMath.mulDivRoundingUp(amount0, _fee, 1e6);
      TransferHelper.safeTransfer(token0, recipient, amount0);
    }

    uint256 fee1;
    uint256 balance1Before = balanceToken1();
    if (amount1 > 0) {
      fee1 = FullMath.mulDivRoundingUp(amount1, _fee, 1e6);
      TransferHelper.safeTransfer(token1, recipient, amount1);
    }

    ...

    if (paid0 > 0) {
      uint8 _communityFeeToken0 = globalState.communityFeeToken0;
      uint256 fees0;
      if (_communityFeeToken0 > 0) {
        fees0 = (paid0 * _communityFeeToken0) / Constants.COMMUNITY_FEE_DENOMINATOR;
        TransferHelper.safeTransfer(token0, vault, fees0);
      }
      totalFeeGrowth0Token += FullMath.mulDiv(paid0 - fees0, Constants.Q128, _liquidity);
    }

    uint256 paid1 = balanceToken1();
    require(balance1Before.add(fee1) <= paid1, 'F1');
    paid1 -= balance1Before;

    if (paid1 > 0) {
      uint8 _communityFeeToken1 = globalState.communityFeeToken1;
      uint256 fees1;
      if (_communityFeeToken1 > 0) {
        fees1 = (paid1 * _communityFeeToken1) / Constants.COMMUNITY_FEE_DENOMINATOR;
        TransferHelper.safeTransfer(token1, vault, fees1);
      }
      totalFeeGrowth1Token += FullMath.mulDiv(paid1 - fees1, Constants.Q128, _liquidity);
    }

    ...
  }
```

## [G-02] UNUSED NAMED RETURNS COST UNNECESSARY DEPLOYMENT GAS
When a function has unused named returns, unnecessary deployment gas is used. Please consider removing the named return variables in the following functions.

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L551-L559
```solidity
  function _writeTimepoint(
    uint16 timepointIndex,
    uint32 blockTimestamp,
    int24 tick,
    uint128 liquidity,
    uint128 volumePerLiquidityInBlock
  ) private returns (uint16 newTimepointIndex) {
    return IDataStorageOperator(dataStorageOperator).write(timepointIndex, blockTimestamp, tick, liquidity, volumePerLiquidityInBlock);
  }
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L561-L578
```solidity
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
    return IDataStorageOperator(dataStorageOperator).getSingleTimepoint(blockTimestamp, secondsAgo, startTick, timepointIndex, liquidityStart);
  }
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L205-L263
```solidity
  function getSingleTimepoint(
    Timepoint[UINT16_MODULO] storage self,
    uint32 time,
    uint32 secondsAgo,
    int24 tick,
    uint16 index,
    uint16 oldestIndex,
    uint128 liquidity
  ) internal view returns (Timepoint memory targetTimepoint) {
    ...
    (Timepoint memory beforeOrAt, Timepoint memory atOrAfter) = binarySearch(self, time, target, index, oldestIndex);

    if (target == atOrAfter.blockTimestamp) {
      return atOrAfter; // we're at the right boundary
    }

    if (target != beforeOrAt.blockTimestamp) {
      // we're in the middle
      uint32 timepointTimeDelta = atOrAfter.blockTimestamp - beforeOrAt.blockTimestamp;
      uint32 targetDelta = target - beforeOrAt.blockTimestamp;

      // For gas savings the resulting point is written to beforeAt
      beforeOrAt.tickCumulative += ((atOrAfter.tickCumulative - beforeOrAt.tickCumulative) / timepointTimeDelta) * targetDelta;
      beforeOrAt.secondsPerLiquidityCumulative += uint160(
        (uint256(atOrAfter.secondsPerLiquidityCumulative - beforeOrAt.secondsPerLiquidityCumulative) * targetDelta) / timepointTimeDelta
      );
      beforeOrAt.volatilityCumulative += ((atOrAfter.volatilityCumulative - beforeOrAt.volatilityCumulative) / timepointTimeDelta) * targetDelta;
      beforeOrAt.volumePerLiquidityCumulative +=
        ((atOrAfter.volumePerLiquidityCumulative - beforeOrAt.volumePerLiquidityCumulative) / timepointTimeDelta) *
        targetDelta;
    }

    // we're at the left boundary or at the middle
    return beforeOrAt;
  }
```

## [G-03] ARRAY LENGTH CAN BE CACHED OUTSIDE OF LOOP
Caching the array length outside of the loop and using the cached length in the loop costs less gas than reading the array length for each iteration. For example, `secondsAgos.length` in the following code can be cached outside of the loop like `uint256 secondsAgosLength = secondsAgos.length`, and `i < secondsAgosLength` can be used for each iteration.

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307-L315
```solidity
    for (uint256 i = 0; i < secondsAgos.length; i++) {
      current = getSingleTimepoint(self, time, secondsAgos[i], tick, index, oldestIndex, liquidity);
      (tickCumulatives[i], secondsPerLiquidityCumulatives[i], volatilityCumulatives[i], volumePerAvgLiquiditys[i]) = (
        current.tickCumulative,
        current.secondsPerLiquidityCumulative,
        current.volatilityCumulative,
        current.volumePerLiquidityCumulative
      );
    }
```

## [G-04] VARIABLE DOES NOT NEED TO BE INITIALIZED TO ITS DEFAULT VALUE
Explicitly initializing a variable with its default value costs more gas than uninitializing it. For example, `uint256 i` can be used instead of `uint256 i = 0` in the following code.

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307-L315
```solidity
    for (uint256 i = 0; i < secondsAgos.length; i++) {
      current = getSingleTimepoint(self, time, secondsAgos[i], tick, index, oldestIndex, liquidity);
      (tickCumulatives[i], secondsPerLiquidityCumulatives[i], volatilityCumulatives[i], volumePerAvgLiquiditys[i]) = (
        current.tickCumulative,
        current.secondsPerLiquidityCumulative,
        current.volatilityCumulative,
        current.volumePerLiquidityCumulative
      );
    }
```

## [G-05] ++VARIABLE CAN BE USED INSTEAD OF VARIABLE++
++variable costs less gas than variable++. For example, `i++` can be changed to `++i` in the following code.

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307-L315
```solidity
    for (uint256 i = 0; i < secondsAgos.length; i++) {
      current = getSingleTimepoint(self, time, secondsAgos[i], tick, index, oldestIndex, liquidity);
      (tickCumulatives[i], secondsPerLiquidityCumulatives[i], volatilityCumulatives[i], volumePerAvgLiquiditys[i]) = (
        current.tickCumulative,
        current.secondsPerLiquidityCumulative,
        current.volatilityCumulative,
        current.volumePerLiquidityCumulative
      );
    }
```

## [G-06] X = X + Y OR X = X - Y CAN BE USED INSTEAD OF X += Y OR X -= Y
x = x + y or x = x - y costs less gas than x += y or x -= y. For example, `tick += 1` can be changed to `tick = tick + 1` in the following code.

```solidity
core\contracts\AlgebraPool.sol
  257: _position.fees0 += fees0;
  258: _position.fees1 += fees1;
  801: amountRequired -= (step.input + step.feeAmount).toInt256(); // decrease remaining input amount
  804: amountRequired += step.output.toInt256(); // increase remaining output amount (since its negative)
  810: step.feeAmount -= delta;
  811: communityFeeAmount += delta;
  814: if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);
  922: paid0 -= balance0Before;
  931: totalFeeGrowth0Token += FullMath.mulDiv(paid0 - fees0, Constants.Q128, _liquidity);
  936: paid1 -= balance1Before;
  945: totalFeeGrowth1Token += FullMath.mulDiv(paid1 - fees1, Constants.Q128, _liquidity);

core\contracts\libraries\AdaptiveFee.sol
  84: res += xLowestDegree * gHighestDegree;
  88: res += (xLowestDegree * gHighestDegree) / 2;
  92: res += (xLowestDegree * gHighestDegree) / 6;
  96: res += (xLowestDegree * gHighestDegree) / 24;
  100: res += (xLowestDegree * gHighestDegree) / 120;
  104: res += (xLowestDegree * gHighestDegree) / 720;
  107: res += (xLowestDegree * g) / 5040 + (xLowestDegree * x) / (40320);

core\contracts\libraries\DataStorage.sol
  79: last.tickCumulative += int56(tick) * delta;
  80: last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0
  81: last.volatilityCumulative += uint88(_volatilityOnRange(delta, prevTick, tick, last.averageTick, averageTick)); // always fits 88 bits
  83: last.volumePerLiquidityCumulative += volumePerLiquidity;
  119: index -= 1; // considering underflow
  251: beforeOrAt.tickCumulative += ((atOrAfter.tickCumulative - beforeOrAt.tickCumulative) / timepointTimeDelta) * targetDelta;
  252: beforeOrAt.secondsPerLiquidityCumulative += uint160(
  255: beforeOrAt.volatilityCumulative += ((atOrAfter.volatilityCumulative - beforeOrAt.volatilityCumulative) / timepointTimeDelta) * targetDelta;
  256: beforeOrAt.volumePerLiquidityCumulative +=

core\contracts\libraries\TickManager.sol
  58: innerFeeGrowth0Token -= upper.outerFeeGrowth0Token;
  59: innerFeeGrowth1Token -= upper.outerFeeGrowth1Token;

core\contracts\libraries\TickTable.sol
  92: tick -= int24(255 - getMostSignificantBit(_row));
  95: tick -= int24(bitNumber);
  100: tick += 1;
  112: tick += int24(getSingleSignificantBit(-_row & _row)); // least significant bit
  115: tick += int24(255 - bitNumber);
```

## [G-07] NEWER VERSION OF SOLIDITY CAN BE USED
The protocol can benefit from more gas-efficient features, such as custom errors starting from Solidity v0.8.4, by using a newer version of Solidity. For the following contracts, please consider using a newer Solidity version.

```solidity
core\contracts\AlgebraFactory.sol
  2: pragma solidity =0.7.6; 

core\contracts\AlgebraPool.sol
  2: pragma solidity =0.7.6; 

core\contracts\AlgebraPoolDeployer.sol
  2: pragma solidity =0.7.6; 

core\contracts\DataStorageOperator.sol
  2: pragma solidity =0.7.6; 

core\contracts\base\PoolImmutables.sol
  2: pragma solidity =0.7.6; 

core\contracts\base\PoolState.sol
  2: pragma solidity =0.7.6; 
```