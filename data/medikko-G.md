### [G-01] Use defualt value rather than overwriite variable with their default value.

Overriting varibles with defualt values with their default value will waste only gas and not necessary.

_There are **1** instances of this issue:_

File: DataStorage.sol line affected 307

```
for (uint256 i = 0; i < secondsAgos.length; i++) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L307

### [G-02] In for loop use outside variable for array length

For loop written like this`for (uint256 i; i < array.length; ++i) {` will cost more gas than `for (uint256 i; i < _lengthOfArray; ++i) {` because for every iteration we use mload and memory_offset
that will cost about 6 gas

_There are **1** instances of this issue:_

File: DataStorage.sol line affected 307

```
for (uint256 i = 0; i < secondsAgos.length; i++) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L307

### [G-03] `!=` cost 6 gas less than `>` in require

_There have **8** istance of this issues:_

File: AlgebraPool.sol lines affected 110, 224, 434, 469, 641, 645, 898

```
110 => require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L110

```
224 => require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L224

```
434 => require(liquidityDesired > 0, 'IL');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L434

```
469 => require(liquidityActual > 0, 'IIL2');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L469

```
641 => require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L641

```
645 => require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L645

```
898 => require(_liquidity > 0, 'L');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L898

File: DataStorageOperator.sol line affected 46

```
require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L46

File: PriceMovementMath.sol lines affected 52, 53

```
require(price > 0);
require(liquidity > 0);
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L52

### [G-04] Recomend to use one operator for comparison where can be used

If you use `>=` or `<=` this will cost more gas because in the EVM there is no implementation of Opcodes for `>=` and `<= and two operations are done.

_There have **14** istance of this issues:_

File: AlgebraPool line affected 229

```
require((_blockTimestamp() - lastLiquidityAddTimestamp) >= _liquidityCooldown);
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L229

File: DataStorageOperator.sol lines affected 138, 140

```
if (volume >= 2**192) volumeShifted = (type(uint256).max) / (liquidity > 0 ? liquidity : 1);
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L138

```
if (volumeShifted >= MAX_VOLUME_PER_LIQUIDITY) return MAX_VOLUME_PER_LIQUIDITY;
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L140

File: AdaptiveFee.sol lines affected 52, 59

```
if (x >= 6 * uint256(g)) return alpha; // so x < 19 bits
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L52

```
if (x >= 6 * uint256(g)) return 0; // so x < 19 bits
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L59

File: DataStorage.sol line affected 156

```
uint256 right = lastIndex >= oldestIndex ? lastIndex : lastIndex + UINT16_MODULO; // newest
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L156

File: PriceMovementMath lines affected 64, 155, 159, 180

```
if (denominator >= liquidityShifted) return uint160(FullMath.mulDivRoundingUp
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L64

```
if (amountAvailable >= 0) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L155

```
if (amountAvailableAfterFee >= input) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L159

```
if (uint256(amountAvailable) >= output) resultPrice = targetPrice;
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L180

File: TickManager.sol line affected 51

```
if (currentTick >= bottomTick) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L51

File: TokenDeltaMath.sol lines affected 51, 66, 81

```
require(priceUpper >= priceLower);
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TokenDeltaMath.sol#L51

```
token0Delta = liquidity >= 0
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TokenDeltaMath.sol#L66

```
token1Delta = liquidity >= 0
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TokenDeltaMath.sol#L81

### [G-05] Recomend to use `uint256(1)`/`uint256(2)` for true and false

If you don't use boolean for storage you will avoid Gwarmaccess 100 gas. Also boolean from true to false cost 20000 gas rather than uint256(2) to uint256(1) that cost less.

_There have **5** istance of this issues:_

File: AlgebraPool.sol lines affected 680, 686, 695

```
bool computedLatestTimepoint; //  if we have already fetched _tickCumulative_ and
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L680

```
bool exactInput; // Whether the exact input or output is specified
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L686

```
bool initialized; // True if the _nextTick is initialized
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L695

File: DataStorage.sol line affected 15

```
bool initialized; // whether or not the timepoint is initialized
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L15

File: TickManager.sol line affected 27

```
bool initialized; // these 8 bits are set to prevent fresh sstores when crossing newly
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L27

### [G-06] i++, i += 1 and i = i + 1 IN FOR LOOPS COSTS MORE GAS COMPARED TO ++i

++i will save about 5 gas for each iteration

_There are **1** instances of this issue:_

File: DataStorage.sol line affected 307

```
for (uint256 i = 0; i < secondsAgos.length; i++) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L307

### [G-07] Use `uncheck()` in for loops whenere overflow and undeflow is not possible

Using of `uncheck(i++)`/`uncheck(++i)` will save about 30 gas per iteration because compiler not save check everytime. This feature come from 0.8

_There are **1** instances of this issue:_

File: DataStorage.sol line affected 307

```
for (uint256 i = 0; i < secondsAgos.length; i++) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L307

### [G-08] Using of custom errors than are came from 0.8.4 instead of `require()`/`revert()` strings will save deployment gas

Using of custom errors will save about 50 gas each time when they are executed.

_There have **58** istance of this issues:_

File: AlgebraFactory.sol lines affected 43, 60, 62, 63, 78, 85, 92, 109, 110

```
require(msg.sender == owner);
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L43

```
require(tokenA != tokenB);
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L60

```
require(token0 != address(0));
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L62

```
require(poolByPair[token0][token1] == address(0));
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L63

```
require(owner != _owner);
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L78

```
require(farmingAddress != _farmingAddress);
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L85

```
require(vaultAddress != _vaultAddress);
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L92

```
require(uint256(alpha1) + uint256(alpha2) + uint256(baseFee) <= type(uint16).max, 'Max fee exceeded');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L109

```
require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L110

File: AlgebraPool.sol lines affected 55, 60, 61, 62, 122, 134, 194, 224, 229, 434, 454, 455, 469, 474, 475, 608, 614, 636, 641, 645, 731, 733, 739, 743, 898, 921, 935, 953, 960, 968

```
require(msg.sender == IAlgebraFactory(factory).owner());
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L55

```
require(topTick < TickMath.MAX_TICK + 1, 'TUM');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L55

```
require(topTick > bottomTick, 'TLU');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L61

```
require(bottomTick > TickMath.MIN_TICK - 1, 'TLM');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L62

```
require(_lower.initialized);
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L122

```
require(_upper.initialized);
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L134

```
require(globalState.price == 0, 'AI');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L194

```
require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L224

```
require((_blockTimestamp() - lastLiquidityAddTimestamp) >= _liquidityCooldown);
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L229

```
require(liquidityDesired > 0, 'IL');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L434

```
if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0,
'IIAM');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L454

```
if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L455

```
require(liquidityActual > 0, 'IIL2');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L469

```
require((amount0 = uint256(amount0Int)) <= receivedAmount0, 'IIAM2');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L474

```
require((amount1 = uint256(amount1Int)) <= receivedAmount1, 'IIAM2');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L475

```
require(balance0Before.add(uint256(amount0)) <= balanceToken0(), 'IIA');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L608

```
require(balance1Before.add(uint256(amount1)) <= balanceToken1(), 'IIA');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L614

```
require(globalState.unlocked, 'LOK');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L636

```
require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L641

```
require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L645

```
require(unlocked, 'LOK');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L731

```
require(amountRequired != 0, 'AS');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L733

```
require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L739

```
require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L743

```
require(\_liquidity > 0, 'L');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L898

```
require(balance0Before.add(fee0) <= paid0, 'F0');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L921

```
require(balance1Before.add(fee1) <= paid1, 'F1');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L935

```
require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L953

```
require(msg.sender == IAlgebraFactory(factory).farmingAddress());
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L960

```
require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L968

File: AlgebraPoolDeployer.sol lines affected 22, 27, 37, 38

```
require(msg.sender == factory);
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L22

```
require(msg.sender == owner);
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L27

```
require(\_factory != address(0));
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L37

```
require(factory == address(0));
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L38

File: DataStorageOperator.sol lines affected 27, 43, 45, 46,

```
require(msg.sender == pool, 'only pool can call this');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L27

```
require(msg.sender == factory || msg.sender == IAlgebraFactory(factory).owner());
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L43

```
require(uint256(\_feeConfig.alpha1) + uint256(\_feeConfig.alpha2) + uint256(\_feeConfig.baseFee) <= type(uint16).max, 'Max fee exceeded');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L45

```
require(\_feeConfig.gamma1 != 0 && \_feeConfig.gamma2 != 0 && \_feeConfig.volumeGamma != 0, 'Gammas must be > 0');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L46

File: DataStorage.sol lines affected 238, 369

```
require(lteConsideringOverflow(self[oldestIndex].blockTimestamp, target, time), 'OLD');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L238

```
require(!self[0].initialized);
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L369

File: PriceMovementMath.sol lines affected 52, 53, 70, 71, 87, 96

```
require(price > 0);
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L52

```
require(liquidity > 0);
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L53

```
require((product = amount \* price) / amount == price); // if the product overflows, we know the denominator underflows
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L70

```
require(liquidityShifted > product); // in addition, we must check that the denominator does not underflow
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L71

```
require(price > quotient);
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L87

File: TickManager.sol line affected 96

```
require(liquidityTotalAfter < Constants.MAX_LIQUIDITY_PER_TICK + 1, 'LO');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickManager.sol#L96

File: TickTable.sol line affected 15

```
require(tick % Constants.TICK_SPACING == 0, 'tick is not spaced'); // ensure that the tick is spaced
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickManager.sol#L96

File: TokenDeltaMath.sol lines affected 30, 51

```
require(priceDelta < priceUpper); // forbids underflow and 0 priceLower
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TokenDeltaMath.sol#L30

```
require(priceUpper >= priceLower);
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TokenDeltaMath.sol#L51

### [G-09] Recomend to declare constant with private modifier

Values can be read from the verified contract source code if you need it.

_There are **1** instances of this issue:_

File: DataStorage.sol line affected 12

```
uint32 public constant WINDOW = 1 days;
```

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L12

### [G-10] Recomending to not use constructor parameters where is possible because of their cost.

If you not use constructor parameters, deployment will be cheaper. I recomend instead of them to hard code them and change in time if needed with custome function for that purpose.

_There are **3** instances of this issue:_

```
poolDeployer = _poolDeployer;
```

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L54

```
vaultAddress = _vaultAddress;
```

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L55

```
pool = _pool;
```

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L33
