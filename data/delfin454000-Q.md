### Named return variables are not used when a function returns
The existence of a named return variable that is never used (here, `mostBitPos`) is potentially perplexing

[TickTable.sol: L46](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickTable.sol#L46)
```solidity
  function getMostSignificantBit(uint256 word) internal pure returns (uint8 mostBitPos) {
```
___
___

### Missing `@param` statement

[PriceMovementMath.sol: L125-143](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L125-L143)
```solidity
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
```
`@param` statement missing for `zeroToOne`
___
___


### Typos
___
The same typo (`an`) occurs in all four lines referenced below. I normally wouldn't flag "an" versus "a", but `an timestamp` comes across as awkward and should be corrected.

[DataStorage.sol: L193](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L193)

[DataStorage.sol: L199](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L199)

[DataStorage.sol: L269](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L269)

[DataStorage.sol: L393](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L393)

*Example ([DataStorage.sol: L193](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L193)):*
```solidity
  /// @dev Reverts if an timepoint at or before the desired timepoint timestamp does not exist.
```
Change `an` to `a` in each case
___
[TickManager.sol: L27](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickManager.sol#L27)
```solidity
    bool initialized; // these 8 bits are set to prevent fresh sstores when crossing newly initialized ticks
```
Change `sstores` to `stores`
___
___

### Long single line comments 
In theory, comments over 79 characters should wrap using multi-line comment syntax. Even if somewhat longer comments are acceptable and a scroll bar is provided, very long comments can interfere with readability. Below are five examples of code with lines that could benefit from wrapping, as shown:
___
[AlgebraPool.sol: L345](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L345)
```solidity
      // if liquidityDelta is negative and the tick was toggled, it means that it should not be initialized anymore, so we delete it
```
Suggestion:
```solidity
      // If liquidityDelta is negative and the tick was toggled, it means that
      //   it should not be initialized anymore, so we delete it.
```
___
[DataStorage.sol: L360](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L360)
```solidity
  /// @notice Initialize the dataStorage array by writing the first slot. Called once for the lifecycle of the timepoints array
```
Suggestion:
```solidity
  /// @notice Initialize the dataStorage array by writing the first slot.
  ///   Called once for the lifecycle of the timepoints array.
```
___
[DataStorage.sol: L265-276](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L265-L276)
```solidity
  /// @notice Returns the accumulator values as of each time seconds ago from the given time in the array of `secondsAgos`
  /// @dev Reverts if `secondsAgos` > oldest timepoint
  /// @param self The stored dataStorage array
  /// @param time The current block.timestamp
  /// @param secondsAgos Each amount of time to look back, in seconds, at which point to return an timepoint
  /// @param tick The current tick
  /// @param index The index of the timepoint that was most recently written to the timepoints array
  /// @param liquidity The current in-range pool liquidity
  /// @return tickCumulatives The tick * time elapsed since the pool was first initialized, as of each `secondsAgo`
  /// @return secondsPerLiquidityCumulatives The cumulative seconds / max(1, liquidity) since the pool was first initialized, as of each `secondsAgo`
  /// @return volatilityCumulatives The cumulative volatility values since the pool was first initialized, as of each `secondsAgo`
  /// @return volumePerAvgLiquiditys The cumulative volume per liquidity values since the pool was first initialized, as of each `secondsAgo`
```
Suggestion:
```solidity
  /// @notice Returns the accumulator values as of each time seconds ago
  ///   from the given time in the array of `secondsAgos`.
  /// @dev Reverts if `secondsAgos` > oldest timepoint
  /// @param self The stored dataStorage array
  /// @param time The current block.timestamp
  /// @param secondsAgos Each amount of time to look back, in seconds,
  ///   at which point to return an timepoint.
  /// @param tick The current tick
  /// @param index The index of the timepoint that was most recently
  ///   written to the timepoints array.
  /// @param liquidity The current in-range pool liquidity
  /// @return tickCumulatives The tick * time elapsed since the pool was 
  ///   first initialized, as of each `secondsAgo`.
  /// @return secondsPerLiquidityCumulatives The cumulative seconds / max(1, liquidity) 
  ///   since the pool was first initialized, as of each `secondsAgo`.
  /// @return volatilityCumulatives The cumulative volatility values since the
  ///   pool was first initialized, as of each `secondsAgo`.
  /// @return volumePerAvgLiquiditys The cumulative volume per liquidity values
  ///   since the pool was first initialized, as of each `secondsAgo`.
```
___
[TickTable.sol: L9](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickTable.sol#L9)
```solidity
/// @dev The mapping uses int16 for keys since ticks are represented as int24 and there are 256 (2^8) values per word.
```
Suggestion:
```solidity
/// @dev The mapping uses int16 for keys since ticks are represented as int24
///   and there are 256 (2^8) values per word
```
___
[TokenDeltaMath.sol: L22](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TokenDeltaMath.sol#L22)
```solidity
  /// @return token0Delta Amount of token0 required to cover a position of size liquidity between the two passed prices
```
Suggestion:
```solidity
  /// @return token0Delta Amount of token0 required to cover a position of
  ///   size liquidity between the two passed prices.
```
___
___

### Missing `require` revert string
A `require` message should be included to enable users to understand the reason for failure

[AlgebraFactory.sol: L43](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L43)
```solidity
    require(msg.sender == owner);
```
Recommendation: Add a brief (<= 32 char) explanatory message. 

Similarly for each of the 26 `requires` referenced below. Also, consider using custom error codes (which would be cheaper than revert strings).

[AlgebraFactory.sol: L60](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L60)

[AlgebraFactory.sol: L62](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L62)

[AlgebraFactory.sol: L63](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L63)

[AlgebraFactory.sol: L78](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L78)

[AlgebraFactory.sol: L85](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L85)

[AlgebraFactory.sol: L92](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L92)

[AlgebraPool.sol: L55](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L55)

[AlgebraPool.sol: L122](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L122)

[AlgebraPool.sol: L134](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L134)

[AlgebraPool.sol: L229](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L229)

[AlgebraPool.sol: L953](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L953)

[AlgebraPool.sol: L960](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L960)

[AlgebraPool.sol: L968](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L968)

[AlgebraPoolDeployer.sol: L22](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L22)

[AlgebraPoolDeployer.sol: L27](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L27)

[AlgebraPoolDeployer.sol: L37](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L37)

[AlgebraPoolDeployer.sol: L38](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L38)

[DataStorageOperator.sol: L43](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L43)

[DataStorage.sol: L369](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L369)

[PriceMovementMath.sol: L52](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L52)

[PriceMovementMath.sol: L53](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L53)

[PriceMovementMath.sol: L70](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L70)

[PriceMovementMath.sol: L71](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L71)

[PriceMovementMath.sol: L87](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L87)

[TokenDeltaMath.sol: L30](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TokenDeltaMath.sol#L30)

[TokenDeltaMath.sol: L51](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TokenDeltaMath.sol#L51)
___
___

### `Require` message is inadequate
A revert string should provide enough information for users to understand the reason for failure

[AlgebraPool.sol: L60](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L60)
```solidity
    require(topTick < TickMath.MAX_TICK + 1, 'TUM');
```
Recommendation: Add a brief (<= 32 char) explanatory message instead of using an acronym that not all users may understand. 

Similarly for each of the 23 `requires` referenced below. Again, consider using custom error codes (which would be cheaper than revert strings).

[AlgebraPool.sol: L61](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L61)

[AlgebraPool.sol: L62](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L62)

[AlgebraPool.sol: L194](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L194)

[AlgebraPool.sol: L224](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L224)

[AlgebraPool.sol: L434](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L434)

[AlgebraPool.sol: L469](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L469)

[AlgebraPool.sol: L474](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L474)

[AlgebraPool.sol: L475](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L475)

[AlgebraPool.sol: L608](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L608)

[AlgebraPool.sol: L614](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L614)

[AlgebraPool.sol: L636](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L636)

[AlgebraPool.sol: L641](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L641)

[AlgebraPool.sol: L645](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L645)

[AlgebraPool.sol: L731](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L731)

[AlgebraPool.sol: L733](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L733)

[AlgebraPool.sol: L739](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L739)

[AlgebraPool.sol: L743](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L743)

[AlgebraPool.sol: L898](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L898)

[AlgebraPool.sol: L921](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L921)

[AlgebraPool.sol: L935](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L935)

[DataStorage.sol: L238](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L238)

[TickManager.sol: L96](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickManager.sol#L96)

[PoolState.sol: L41](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolState.sol#L41)
___
___


