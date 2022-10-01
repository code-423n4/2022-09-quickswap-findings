## Summary



### Low Risk Issues
| |Issue|Instances|
|-|:-|:-:|
| [L&#x2011;01] | There will be no liquidity cooldowns for the first few blocks in ~84 years | 1 |
| [L&#x2011;02] | TWAP answers will be wrong in ~84 years | 1 |
| [L&#x2011;03] | `require()` should be used instead of `assert()` | 1 |
| [L&#x2011;04] | Missing checks for `address(0x0)` when assigning values to `address` state variables | 7 |

Total: 10 instances over 4 issues

### Non-critical Issues
| |Issue|Instances|
|-|:-|:-:|
| [N&#x2011;01] | `require()`/`revert()` statements should have descriptive reason strings | 27 |
| [N&#x2011;02] | `2**<n> - 1` should be re-written as `type(uint<n>).max` | 1 |
| [N&#x2011;03] | `constant`s should be defined rather than using magic numbers | 41 |
| [N&#x2011;04] | Large multiples of ten should use scientific notation (e.g. `1e6`) rather than decimal literals (e.g. `1000000`), for readability | 1 |
| [N&#x2011;05] | Use bit shifts in an imutable variable rather than long bit masks of a single bit, for readability | 2 |
| [N&#x2011;06] | Use a more recent version of solidity | 5 |
| [N&#x2011;07] | Use a more recent version of solidity | 1 |
| [N&#x2011;08] | Lines are too long | 1 |
| [N&#x2011;09] | Variable names that consist of all capital letters should be reserved for `constant`/`immutable` variables | 2 |
| [N&#x2011;10] | File is missing NatSpec | 4 |
| [N&#x2011;11] | NatSpec is incomplete | 7 |
| [N&#x2011;12] | Not using the named return variables anywhere in the function is confusing | 33 |

Total: 125 instances over 12 issues




## Low Risk Issues

### [L&#x2011;01]  There will be no liquidity cooldowns for the first few blocks in ~84 years
Since `_blockTimestamp()` truncates `block.timestamp` to `uint32`, once the current timestamp passes `type(uint32).max`, the subtraction below will underflow, causing the `require()` statement to pass, regardless of the time since the last liquidity addition

*There is 1 instance of this issue:*
```solidity
File: /src/core/contracts/AlgebraPool.sol

229:           require((_blockTimestamp() - lastLiquidityAddTimestamp) >= _liquidityCooldown);

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L229

### [L&#x2011;02]  TWAP answers will be wrong in ~84 years
Like the issue above, the timestamp in the calculation below will wrap, leading to answers about the TWAP value to be wrong

*There is 1 instance of this issue:*
```solidity
File: /src/core/contracts/libraries/DataStorage.sol

133:       avgTick = (lastTimestamp == oldestTimestamp) ? tick : (lastTickCumulative - oldestTickCumulative) / (lastTimestamp - oldestTimestamp);

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L133

### [L&#x2011;03]  `require()` should be used instead of `assert()`
Prior to solidity version 0.8.0, hitting an assert consumes the **remainder of the transaction's available gas** rather than returning it, as `require()`/`revert()` do. `assert()` should be avoided even past solidity version 0.8.0 as its [documentation](https://docs.soliditylang.org/en/v0.8.14/control-structures.html#panic-via-assert-and-error-via-require) states that "The assert function creates an error of type Panic(uint256). ... Properly functioning code should never create a Panic, not even on invalid external input. If this happens, then there is a bug in your contract which you should fix".

*There is 1 instance of this issue:*
```solidity
File: src/core/contracts/libraries/DataStorage.sol

190:      assert(false);

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L190

### [L&#x2011;04]  Missing checks for `address(0x0)` when assigning values to `address` state variables

*There are 7 instances of this issue:*
```solidity
File: src/core/contracts/AlgebraFactory.sol

54:       poolDeployer = _poolDeployer;

55:       vaultAddress = _vaultAddress;

80:       owner = _owner;

87:       farmingAddress = _farmingAddress;

94:       vaultAddress = _vaultAddress;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L54

```solidity
File: src/core/contracts/AlgebraPool.sol

961:      activeIncentive = virtualPoolAddress;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L961

```solidity
File: src/core/contracts/DataStorageOperator.sol

33:       pool = _pool;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L33

## Non-critical Issues

### [N&#x2011;01]  `require()`/`revert()` statements should have descriptive reason strings

*There are 27 instances of this issue:*
```solidity
File: src/core/contracts/AlgebraFactory.sol

43:       require(msg.sender == owner);

60:       require(tokenA != tokenB);

62:       require(token0 != address(0));

63:       require(poolByPair[token0][token1] == address(0));

78:       require(owner != _owner);

85:       require(farmingAddress != _farmingAddress);

92:       require(vaultAddress != _vaultAddress);

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L43

```solidity
File: src/core/contracts/AlgebraPoolDeployer.sol

22:       require(msg.sender == factory);

27:       require(msg.sender == owner);

37:       require(_factory != address(0));

38:       require(factory == address(0));

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L22

```solidity
File: src/core/contracts/AlgebraPool.sol

55:       require(msg.sender == IAlgebraFactory(factory).owner());

122:        require(_lower.initialized);

134:        require(_upper.initialized);

229:            require((_blockTimestamp() - lastLiquidityAddTimestamp) >= _liquidityCooldown);

953:      require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));

960:      require(msg.sender == IAlgebraFactory(factory).farmingAddress());

968:      require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L55

```solidity
File: src/core/contracts/DataStorageOperator.sol

43:       require(msg.sender == factory || msg.sender == IAlgebraFactory(factory).owner());

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L43

```solidity
File: src/core/contracts/libraries/DataStorage.sol

369:      require(!self[0].initialized);

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L369

```solidity
File: src/core/contracts/libraries/PriceMovementMath.sol

52:       require(price > 0);

53:       require(liquidity > 0);

70:           require((product = amount * price) / amount == price); // if the product overflows, we know the denominator underflows

71:           require(liquidityShifted > product); // in addition, we must check that the denominator does not underflow

87:           require(price > quotient);

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L52

```solidity
File: src/core/contracts/libraries/TokenDeltaMath.sol

30:       require(priceDelta < priceUpper); // forbids underflow and 0 priceLower

51:       require(priceUpper >= priceLower);

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TokenDeltaMath.sol#L30

### [N&#x2011;02]  `2**<n> - 1` should be re-written as `type(uint<n>).max`
Earlier versions of solidity can use `uint<n>(-1)` instead. Expressions not including the `- 1` can often be re-written to accomodate the change (e.g. by using a `>` rather than a `>=`, which will also save some gas)

*There is 1 instance of this issue:*
```solidity
File: src/core/contracts/DataStorageOperator.sol

138:      if (volume >= 2**192) volumeShifted = (type(uint256).max) / (liquidity > 0 ? liquidity : 1);

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L138

### [N&#x2011;03]  `constant`s should be defined rather than using magic numbers
Even [assembly](https://github.com/code-423n4/2022-05-opensea-seaport/blob/9d7ce4d08bf3c3010304a0476a785c70c0e90ae7/contracts/lib/TokenTransferrer.sol#L35-L39) can benefit from using readable constants instead of hex/numeric literals

*There are 41 instances of this issue:*
```solidity
File: src/core/contracts/AlgebraPool.sol

/// @audit 0xFFFFFF
/// @audit 0xFFFFFF
410:        key := or(shl(24, or(shl(24, owner), and(bottomTick, 0xFFFFFF))), and(topTick, 0xFFFFFF))

/// @audit 1e6
905:        fee0 = FullMath.mulDivRoundingUp(amount0, _fee, 1e6);

/// @audit 1e6
912:        fee1 = FullMath.mulDivRoundingUp(amount1, _fee, 1e6);

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L410

```solidity
File: src/core/contracts/DataStorageOperator.sol

/// @audit 192
138:      if (volume >= 2**192) volumeShifted = (type(uint256).max) / (liquidity > 0 ? liquidity : 1);

/// @audit 64
139:      else volumeShifted = (volume << 64) / (liquidity > 0 ? liquidity : 1);

/// @audit 15
158:      return AdaptiveFee.getFee(volatilityAverage / 15, volumePerLiqAverage, feeConfig);

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L138

```solidity
File: src/core/contracts/libraries/AdaptiveFee.sol

/// @audit 6
52:         if (x >= 6 * uint256(g)) return alpha; // so x < 19 bits

/// @audit 8
53:         uint256 g8 = uint256(g)**8; // < 128 bits (8*16)

/// @audit 6
59:         if (x >= 6 * uint256(g)) return 0; // so x < 19 bits

/// @audit 8
60:         uint256 g8 = uint256(g)**8; // < 128 bits (8*16)

/// @audit 6
92:       res += (xLowestDegree * gHighestDegree) / 6;

/// @audit 24
96:       res += (xLowestDegree * gHighestDegree) / 24;

/// @audit 120
100:      res += (xLowestDegree * gHighestDegree) / 120;

/// @audit 720
104:      res += (xLowestDegree * gHighestDegree) / 720;

/// @audit 5040
107:      res += (xLowestDegree * g) / 5040 + (xLowestDegree * x) / (40320);

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L52

```solidity
File: src/core/contracts/libraries/DataStorage.sol

/// @audit 6
/// @audit 6
/// @audit 6
53:       volatility = uint256((K**2 * sumOfSquares + 6 * B * K * sumOfSequence + 6 * dt * B**2) / (6 * dt**2));

/// @audit 128
80:       last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0

/// @audit 57
348:          uint256(endOfWindow.volumePerLiquidityCumulative - startOfWindow.volumePerLiquidityCumulative) >> 57

/// @audit 57
355:          uint256(endOfWindow.volumePerLiquidityCumulative - _oldestVolumePerLiquidityCumulative) >> 57

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L53

```solidity
File: src/core/contracts/libraries/PriceMovementMath.sol

/// @audit 1e6
/// @audit 1e6
157:        uint256 amountAvailableAfterFee = FullMath.mulDiv(uint256(amountAvailable), 1e6 - fee, 1e6);

/// @audit 1e6
161:          feeAmount = FullMath.mulDivRoundingUp(input, fee, 1e6 - fee);

/// @audit 1e6
170:            feeAmount = FullMath.mulDivRoundingUp(input, fee, 1e6 - fee);

/// @audit 1e6
195:        feeAmount = FullMath.mulDivRoundingUp(input, fee, 1e6 - fee);

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L157

```solidity
File: src/core/contracts/libraries/TickTable.sol

/// @audit 0xFF
21:         bitNumber := and(tick, 0xFF)

/// @audit 0x5555555555555555555555555555555555555555555555555555555555555555
32:         singleBitPos := iszero(and(word, 0x5555555555555555555555555555555555555555555555555555555555555555))

/// @audit 0x00000000000000000000000000000000FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF
33:         singleBitPos := or(singleBitPos, shl(7, iszero(and(word, 0x00000000000000000000000000000000FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF))))

/// @audit 0x0000000000000000FFFFFFFFFFFFFFFF0000000000000000FFFFFFFFFFFFFFFF
34:         singleBitPos := or(singleBitPos, shl(6, iszero(and(word, 0x0000000000000000FFFFFFFFFFFFFFFF0000000000000000FFFFFFFFFFFFFFFF))))

/// @audit 0x00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF
35:         singleBitPos := or(singleBitPos, shl(5, iszero(and(word, 0x00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF00000000FFFFFFFF))))

/// @audit 0x0000FFFF0000FFFF0000FFFF0000FFFF0000FFFF0000FFFF0000FFFF0000FFFF
36:         singleBitPos := or(singleBitPos, shl(4, iszero(and(word, 0x0000FFFF0000FFFF0000FFFF0000FFFF0000FFFF0000FFFF0000FFFF0000FFFF))))

/// @audit 0x00FF00FF00FF00FF00FF00FF00FF00FF00FF00FF00FF00FF00FF00FF00FF00FF
37:         singleBitPos := or(singleBitPos, shl(3, iszero(and(word, 0x00FF00FF00FF00FF00FF00FF00FF00FF00FF00FF00FF00FF00FF00FF00FF00FF))))

/// @audit 0x0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F
38:         singleBitPos := or(singleBitPos, shl(2, iszero(and(word, 0x0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F0F))))

/// @audit 0x3333333333333333333333333333333333333333333333333333333333333333
39:         singleBitPos := or(singleBitPos, shl(1, iszero(and(word, 0x3333333333333333333333333333333333333333333333333333333333333333))))

/// @audit 0xFF
86:           bitNumber := and(tick, 0xFF)

/// @audit 255
89:         uint256 _row = self[rowNumber] << (255 - bitNumber); // all the 1s at or to the right of the current bitNumber

/// @audit 255
92:           tick -= int24(255 - getMostSignificantBit(_row));

/// @audit 0xFF
104:          bitNumber := and(tick, 0xFF)

/// @audit 255
115:          tick += int24(255 - bitNumber);

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickTable.sol#L21

### [N&#x2011;04]  Large multiples of ten should use scientific notation (e.g. `1e6`) rather than decimal literals (e.g. `1000000`), for readability

*There is 1 instance of this issue:*
```solidity
File: src/core/contracts/DataStorageOperator.sol

16:     uint128 constant MAX_VOLUME_PER_LIQUIDITY = 100000 << 64; // maximum meaningful ratio of volume to liquidity

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L16

### [N&#x2011;05]  Use bit shifts in an imutable variable rather than long bit masks of a single bit, for readability

*There are 2 instances of this issue:*
```solidity
File: src/core/contracts/libraries/Constants.sol

6:      uint256 internal constant Q96 = 0x1000000000000000000000000;

7:      uint256 internal constant Q128 = 0x100000000000000000000000000000000;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/Constants.sol#L6

### [N&#x2011;06]  Use a more recent version of solidity
Use a solidity version of at least 0.8.13 to get the ability to use `using for` with a list of free functions

*There are 5 instances of this issue:*
```solidity
File: src/core/contracts/AlgebraPool.sol

2:    pragma solidity =0.7.6;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L2

```solidity
File: src/core/contracts/DataStorageOperator.sol

2:    pragma solidity =0.7.6;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L2

```solidity
File: src/core/contracts/libraries/PriceMovementMath.sol

2:    pragma solidity =0.7.6;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L2

```solidity
File: src/core/contracts/libraries/TickManager.sol

2:    pragma solidity =0.7.6;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickManager.sol#L2

```solidity
File: src/core/contracts/libraries/TokenDeltaMath.sol

2:    pragma solidity =0.7.6;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TokenDeltaMath.sol#L2

### [N&#x2011;07]  Use a more recent version of solidity
Use a solidity version of at least 0.8.4 to get `bytes.concat()` instead of `abi.encodePacked(<bytes>,<bytes>)`
Use a solidity version of at least 0.8.12 to get `string.concat()` instead of `abi.encodePacked(<str>,<str>)`

*There is 1 instance of this issue:*
```solidity
File: src/core/contracts/AlgebraFactory.sol

2:    pragma solidity =0.7.6;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L2

### [N&#x2011;08]  Lines are too long
Usually lines in source code are limited to [80](https://softwareengineering.stackexchange.com/questions/148677/why-is-80-characters-the-standard-limit-for-code-width) characters. Today's screens are much larger so it's reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over [164](https://github.com/aizatto/character-length) characters, the lines below should be split when they reach that length

*There is 1 instance of this issue:*
```solidity
File: src/core/contracts/libraries/AdaptiveFee.sol

38:       return uint16(config.baseFee + sigmoid(volumePerLiquidity, config.volumeGamma, uint16(sumOfSigmoids), config.volumeBeta)); // safe since alpha1 + alpha2 + baseFee _must_ be <= type(uint16).max

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L38

### [N&#x2011;09]  Variable names that consist of all capital letters should be reserved for `constant`/`immutable` variables
If the variable needs to be different based on which class it comes from, a `view`/`pure` _function_ should be used instead (e.g. like [this](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/76eee35971c2541585e05cbf258510dda7b2fbc6/contracts/token/ERC20/extensions/draft-IERC20Permit.sol#L59)).

*There are 2 instances of this issue:*
```solidity
File: src/core/contracts/libraries/DataStorage.sol

49:       int256 K = (tick1 - tick0) - (avgTick1 - avgTick0); // (k - p)*dt

50:       int256 B = (tick0 - avgTick0) * dt; // (b - q)*dt

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L49

### [N&#x2011;10]  File is missing NatSpec

*There are 4 instances of this issue:*
```solidity
File: src/core/contracts/AlgebraPoolDeployer.sol


```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol

```solidity
File: src/core/contracts/base/PoolImmutables.sol


```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolImmutables.sol

```solidity
File: src/core/contracts/DataStorageOperator.sol


```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol

```solidity
File: src/core/contracts/libraries/Constants.sol


```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/Constants.sol

### [N&#x2011;11]  NatSpec is incomplete

*There are 7 instances of this issue:*
```solidity
File: src/core/contracts/AlgebraPool.sol

/// @audit Missing: '@param owner'
/// @audit Missing: '@param bottomTick'
/// @audit Missing: '@param topTick'
/// @audit Missing: '@param liquidityDelta'
268     /**
269      * @dev Updates position's ticks and its fees
270      * @return position The Position object to operate with
271      * @return amount0 The amount of token0 the caller needs to send, negative if the pool needs to send it
272      * @return amount1 The amount of token1 the caller needs to send, negative if the pool needs to send it
273      */
274     function _updatePositionTicksAndFees(
275       address owner,
276       int24 bottomTick,
277       int24 topTick,
278       int128 liquidityDelta
279     )
280       private
281       returns (
282         Position storage position,
283         int256 amount0,
284:        int256 amount1

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L268-L284

```solidity
File: src/core/contracts/libraries/PriceMovementMath.sol

/// @audit Missing: '@param zeroToOne'
125     /// @notice Computes the result of swapping some amount in, or amount out, given the parameters of the swap
126     /// @dev The fee, plus the amount in, will never exceed the amount remaining if the swap's `amountSpecified` is positive
127     /// @param currentPrice The current Q64.96 sqrt price of the pool
128     /// @param targetPrice The Q64.96 sqrt price that cannot be exceeded, from which the direction of the swap is inferred
129     /// @param liquidity The usable liquidity
130     /// @param amountAvailable How much input or output amount is remaining to be swapped in/out
131     /// @param fee The fee taken from the input amount, expressed in hundredths of a bip
132     /// @return resultPrice The Q64.96 sqrt price after swapping the amount in/out, not to exceed the price target
133     /// @return input The amount to be swapped in, of either token0 or token1, based on the direction of the swap
134     /// @return output The amount to be received, of either token0 or token1, based on the direction of the swap
135     /// @return feeAmount The amount of input that will be taken as a fee
136     function movePriceTowardsTarget(
137       bool zeroToOne,
138       uint160 currentPrice,
139       uint160 targetPrice,
140       uint128 liquidity,
141       int256 amountAvailable,
142       uint16 fee
143     )
144       internal
145       pure
146       returns (
147         uint160 resultPrice,
148         uint256 input,
149         uint256 output,
150:        uint256 feeAmount

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L125-L150

```solidity
File: src/core/contracts/libraries/TickTable.sol

/// @audit Missing: '@return'
28      /// @dev it is assumed that word contains exactly one 1-bit, otherwise the result will be incorrect
29      /// @param word The word containing only one 1-bit
30:     function getSingleSignificantBit(uint256 word) internal pure returns (uint8 singleBitPos) {

/// @audit Missing: '@return'
44      /// @dev it is assumed that before the call, a check will be made that the argument (word) is not equal to zero
45      /// @param word The word containing at least one 1-bit
46:     function getMostSignificantBit(uint256 word) internal pure returns (uint8 mostBitPos) {

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickTable.sol#L28-L30

### [N&#x2011;12]  Not using the named return variables anywhere in the function is confusing
Consider changing the variable to be an unnamed one

*There are 33 instances of this issue:*
```solidity
File: src/core/contracts/AlgebraPool.sol

/// @audit initialized
/// @audit blockTimestamp
/// @audit tickCumulative
/// @audit secondsPerLiquidityCumulative
/// @audit volatilityCumulative
/// @audit averageTick
/// @audit volumePerLiquidityCumulative
79      function timepoints(uint256 index)
80        external
81        view
82        override
83        returns (
84          bool initialized,
85          uint32 blockTimestamp,
86          int56 tickCumulative,
87          uint160 secondsPerLiquidityCumulative,
88          uint88 volatilityCumulative,
89          int24 averageTick,
90:         uint144 volumePerLiquidityCumulative

/// @audit innerTickCumulative
/// @audit innerSecondsSpentPerLiquidity
/// @audit innerSecondsSpent
103     function getInnerCumulatives(int24 bottomTick, int24 topTick)
104       external
105       view
106       override
107       onlyValidTicks(bottomTick, topTick)
108       returns (
109         int56 innerTickCumulative,
110         uint160 innerSecondsSpentPerLiquidity,
111:        uint32 innerSecondsSpent

/// @audit tickCumulatives
/// @audit secondsPerLiquidityCumulatives
/// @audit volatilityCumulatives
/// @audit volumePerAvgLiquiditys
171     function getTimepoints(uint32[] calldata secondsAgos)
172       external
173       view
174       override
175       returns (
176         int56[] memory tickCumulatives,
177         uint160[] memory secondsPerLiquidityCumulatives,
178         uint112[] memory volatilityCumulatives,
179:        uint256[] memory volumePerAvgLiquiditys

/// @audit newTimepointIndex
551     function _writeTimepoint(
552       uint16 timepointIndex,
553       uint32 blockTimestamp,
554       int24 tick,
555       uint128 liquidity,
556       uint128 volumePerLiquidityInBlock
557:    ) private returns (uint16 newTimepointIndex) {

/// @audit tickCumulative
/// @audit secondsPerLiquidityCumulative
/// @audit volatilityCumulative
/// @audit volumePerAvgLiquidity
561     function _getSingleTimepoint(
562       uint32 blockTimestamp,
563       uint32 secondsAgo,
564       int24 startTick,
565       uint16 timepointIndex,
566       uint128 liquidityStart
567     )
568       private
569       view
570       returns (
571         int56 tickCumulative,
572         uint160 secondsPerLiquidityCumulative,
573         uint112 volatilityCumulative,
574:        uint256 volumePerAvgLiquidity

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L79-L90

```solidity
File: src/core/contracts/DataStorageOperator.sol

/// @audit tickCumulatives
/// @audit secondsPerLiquidityCumulatives
/// @audit volatilityCumulatives
/// @audit volumePerAvgLiquiditys
88      function getTimepoints(
89        uint32 time,
90        uint32[] memory secondsAgos,
91        int24 tick,
92        uint16 index,
93        uint128 liquidity
94      )
95        external
96        view
97        override
98        onlyPool
99        returns (
100         int56[] memory tickCumulatives,
101         uint160[] memory secondsPerLiquidityCumulatives,
102         uint112[] memory volatilityCumulatives,
103:        uint256[] memory volumePerAvgLiquiditys

/// @audit TWVolatilityAverage
/// @audit TWVolumePerLiqAverage
110     function getAverages(
111       uint32 time,
112       int24 tick,
113       uint16 index,
114       uint128 liquidity
115:    ) external view override onlyPool returns (uint112 TWVolatilityAverage, uint256 TWVolumePerLiqAverage) {

/// @audit indexUpdated
120     function write(
121       uint16 index,
122       uint32 blockTimestamp,
123       int24 tick,
124       uint128 liquidity,
125       uint128 volumePerLiquidity
126:    ) external override onlyPool returns (uint16 indexUpdated) {

/// @audit fee
150     function getFee(
151       uint32 _time,
152       int24 _tick,
153       uint16 _index,
154       uint128 _liquidity
155:    ) external view override onlyPool returns (uint16 fee) {

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L88-L103

```solidity
File: src/core/contracts/libraries/AdaptiveFee.sol

/// @audit fee
25      function getFee(
26        uint88 volatility,
27        uint256 volumePerLiquidity,
28        Configuration memory config
29:     ) internal pure returns (uint16 fee) {

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L25-L29

```solidity
File: src/core/contracts/libraries/DataStorage.sol

/// @audit targetTimepoint
205     function getSingleTimepoint(
206       Timepoint[UINT16_MODULO] storage self,
207       uint32 time,
208       uint32 secondsAgo,
209       int24 tick,
210       uint16 index,
211       uint16 oldestIndex,
212       uint128 liquidity
213:    ) internal view returns (Timepoint memory targetTimepoint) {

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L205-L213

```solidity
File: src/core/contracts/libraries/PriceMovementMath.sol

/// @audit resultPrice
20      function getNewPriceAfterInput(
21        uint160 price,
22        uint128 liquidity,
23        uint256 input,
24        bool zeroToOne
25:     ) internal pure returns (uint160 resultPrice) {

/// @audit resultPrice
36      function getNewPriceAfterOutput(
37        uint160 price,
38        uint128 liquidity,
39        uint256 output,
40        bool zeroToOne
41:     ) internal pure returns (uint160 resultPrice) {

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L20-L25

```solidity
File: src/core/contracts/libraries/TickManager.sol

/// @audit liquidityDelta
129     function cross(
130       mapping(int24 => Tick) storage self,
131       int24 tick,
132       uint256 totalFeeGrowth0Token,
133       uint256 totalFeeGrowth1Token,
134       uint160 secondsPerLiquidityCumulative,
135       int56 tickCumulative,
136       uint32 time
137:    ) internal returns (int128 liquidityDelta) {

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickManager.sol#L129-L137

```solidity
File: src/core/contracts/libraries/TickTable.sol

/// @audit mostBitPos
46:     function getMostSignificantBit(uint256 word) internal pure returns (uint8 mostBitPos) {

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickTable.sol#L46



