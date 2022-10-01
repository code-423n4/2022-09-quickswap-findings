# 2022-09-QUICKSWAP

## Gas Optimizations Report

### The usage of `++i` will cost less gas than `i++`. The same change can be applied to `i--` as well.

This change would save up to 6 gas per instance/loop.

_There are **1** instances of this issue:_

```solidity
File: src/core/contracts/libraries/DataStorage.sol

307:  for (uint256 i = 0; i < secondsAgos.length; i++) {
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol

### `internal` and `private` functions that are called only once should be inlined.

The execution of a non-inlined function would cost up to 40 more gas because of two extra `jump`s as well as some other instructions.

_There are **1** instances of this issue:_

```solidity
File: src/core/contracts/libraries/DataStorage.sol

148:  function binarySearch(
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol

### Using `!= 0` on `uints` costs less gas than `> 0`.

This change saves 3 gas per instance/loop

_There are **24** instances of this issue:_

```solidity
File: src/core/contracts/AlgebraFactory.sol

110:  require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol

```solidity
File: src/core/contracts/AlgebraPool.sol

224:  require(currentLiquidity > 0, 'NP');

228:  if (_liquidityCooldown > 0) {

237:  liquidityNext > 0 ? (liquidityDelta > 0 ? _blockTimestamp() : lastLiquidityAddTimestamp) : 0

434:  require(liquidityDesired > 0, 'IL');

469:  require(liquidityActual > 0, 'IIL2');

505:  if (amount0 > 0) TransferHelper.safeTransfer(token0, recipient, amount0);

506:  if (amount1 > 0) TransferHelper.safeTransfer(token1, recipient, amount1);

617:  if (communityFee > 0) {

667:  if (communityFee > 0) {

808:  if (cache.communityFee > 0) {

814:  if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);

898:  require(_liquidity > 0, 'L');

904:  if (amount0 > 0) {

911:  if (amount1 > 0) {

924:  if (paid0 > 0) {

927:  if (_communityFeeToken0 > 0) {

938:  if (paid1 > 0) {

941:  if (_communityFeeToken1 > 0) {
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol

```solidity
File: src/core/contracts/DataStorageOperator.sol

138:  if (volume >= 2**192) volumeShifted = (type(uint256).max) / (liquidity > 0 ? liquidity : 1);

139:  else volumeShifted = (volume << 64) / (liquidity > 0 ? liquidity : 1);
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol

```solidity
File: src/core/contracts/libraries/DataStorage.sol

80:   last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1));
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol

```solidity
File: src/core/contracts/libraries/PriceMovementMath.sol

52:   require(price > 0);

53:   require(liquidity > 0);
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol

### It costs more gas to initialize non-`constant`/non-`immutable` variables to zero than to let the default of zero be applied

Not overwriting the default for stack variables saves 8 gas. Storage and memory variables have larger savings

_There are **1** instances of this issue:_

```solidity
File: src/core/contracts/libraries/DataStorage.sol

307:  for (uint256 i = 0; i < secondsAgos.length; i++) {
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol

### Using `private` rather than `public` for constants, saves gas

If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that returns a tuple of the values of all currently-public constants. Saves 3406-3606 gas in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

_There are **1** instances of this issue:_

```solidity
File: src/core/contracts/libraries/DataStorage.sol

12:   uint32 public constant WINDOW = 1 days;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol

### Functions that are access-restricted from most users may be marked as `payable`

Marking a function as `payable` reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **9** instances of this issue:_

```solidity
File: src/core/contracts/AlgebraFactory.sol

77:   function setOwner(address _owner) external override onlyOwner {

84:   function setFarmingAddress(address _farmingAddress) external override onlyOwner {

91:   function setVaultAddress(address _vaultAddress) external override onlyOwner {
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol

```solidity
File: src/core/contracts/AlgebraPool.sol

952:  function setCommunityFee(uint8 communityFee0, uint8 communityFee1) external override lock onlyFactoryOwner {

959:  function setIncentive(address virtualPoolAddress) external override {

967:  function setLiquidityCooldown(uint32 newLiquidityCooldown) external override onlyFactoryOwner {
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol

```solidity
File: src/core/contracts/AlgebraPoolDeployer.sol

36:   function setFactory(address _factory) external override onlyOwner {
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol

```solidity
File: src/core/contracts/DataStorageOperator.sol

37:   function initialize(uint32 time, int24 tick) external override onlyPool {

42:   function changeFeeConfiguration(AdaptiveFee.Configuration calldata _feeConfig) external override {
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol

### `++i`/`i++` should be `unchecked{++I}`/`unchecked{I++}` in `for`-loops

When an increment or any arithmetic operation is not possible to overflow it should be placed in `unchecked{}` block. \This is because of the default compiler overflow and underflow safety checks since Solidity version 0.8.0. \In for-loops it saves around 30-40 gas **per loop**

_There are **1** instances of this issue:_

```solidity
File: src/core/contracts/libraries/DataStorage.sol

307:  for (uint256 i = 0; i < secondsAgos.length; i++) {
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol

### Usage of `uint`s/`int`s smaller than 32 bytes (256 bits) incurs overhead

'When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.' \ https://docs.soliditylang.org/en/v0.8.15/internals/layout_in_storage.html \ Use a larger size then downcast where needed

_There are **249** instances of this issue:_

```solidity
File: src/core/contracts/AlgebraFactory.sol

99:   uint16 alpha1,

100:  uint16 alpha2,

101:  uint32 beta1,

102:  uint32 beta2,

103:  uint16 gamma1,

104:  uint16 gamma2,

105:  uint32 volumeBeta,

106:  uint16 volumeGamma,

107:  uint16 baseFee
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol

```solidity
File: src/core/contracts/AlgebraPool.sol

42:   uint128 liquidity;

43:   uint32 lastLiquidityAddTimestamp;

46:   uint128 fees0;

47:   uint128 fees1;

85:   uint32 blockTimestamp,

87:   uint160 secondsPerLiquidityCumulative,

98:   uint160 outerSecondPerLiquidity;

99:   uint32 outerSecondsSpent;

110:  uint160 innerSecondsSpentPerLiquidity,

111:  uint32 innerSecondsSpent

137:  (int24 currentTick, uint16 currentTimepointIndex) = (globalState.tick, globalState.timepointIndex);

148:  uint32 globalTime = _blockTimestamp();

149:  (int56 globalTickCumulative, uint160 globalSecondsPerLiquidityCumulative, , ) = _getSingleTimepoint(

193:  function initialize(uint160 initialPrice) external override {

198:  uint32 timestamp = _blockTimestamp();

217:  int128 liquidityDelta,

221:  (uint128 currentLiquidity, uint32 lastLiquidityAddTimestamp) = (_position.liquidity, _position.lastLiquidityAddTimestamp);

227:  uint32 _liquidityCooldown = liquidityCooldown;

234:  uint128 liquidityNext = LiquidityMath.addDelta(currentLiquidity, liquidityDelta);

244:  uint128 fees0;

249:  uint128 fees1;

263:  uint160 price;

265:  uint16 timepointIndex;

278:  int128 liquidityDelta

296:  uint32 time = _blockTimestamp();

297:  (int56 tickCumulative, uint160 secondsPerLiquidityCumulative, , ) = _getSingleTimepoint(time, 0, cache.tick, cache.timepointIndex, liquidity);

351:  int128 globalLiquidityDelta;

354:  uint128 liquidityBefore = liquidity;

355:  uint16 newTimepointIndex = _writeTimepoint(cache.timepointIndex, _blockTimestamp(), cache.tick, liquidityBefore, volumePerLiquidityInBlock);

369:  int128 liquidityDelta,

371:  uint160 currentPrice

378:  int128 globalLiquidityDelta

421:  uint128 liquidityDesired,

431:  uint128 liquidityActual

463:  uint128 liquidityForRA1 = uint128(FullMath.mulDiv(uint256(liquidityActual), receivedAmount1, amount1));

492:  uint128 amount0Requested,

493:  uint128 amount1Requested

494:  ) external override lock returns (uint128 amount0, uint128 amount1) {

496:  (uint128 positionFees0, uint128 positionFees1) = (position.fees0, position.fees1);

516:  uint128 amount

537:  uint32 _time,

539:  uint16 _index,

540:  uint128 _liquidity

541:  ) private returns (uint16 newFee) {

552:  uint16 timepointIndex,

553:  uint32 blockTimestamp,

555:  uint128 liquidity,

556:  uint128 volumePerLiquidityInBlock

557:  ) private returns (uint16 newTimepointIndex) {

562:  uint32 blockTimestamp,

563:  uint32 secondsAgo,

565:  uint16 timepointIndex,

566:  uint128 liquidityStart

572:  uint160 secondsPerLiquidityCumulative,

593:  uint160 limitSqrtPrice,

596:  uint160 currentPrice;

598:  uint128 currentLiquidity;

631:  uint160 limitSqrtPrice,

649:  uint160 currentPrice;

651:  uint128 currentLiquidity;

677:  uint128 volumePerLiquidityInBlock;

679:  uint160 secondsPerLiquidityCumulative;

687:  uint16 fee;

689:  uint16 timepointIndex;

693:  uint160 stepSqrtPrice;

696:  uint160 nextTickPrice;

706:  uint160 limitSqrtPrice

712:  uint160 currentPrice,

714:  uint128 currentLiquidity,

718:  uint32 blockTimestamp;

763:  uint16 newTimepointIndex = _writeTimepoint(

835:  int128 liquidityDelta;

897:  uint128 _liquidity = liquidity;

900:  uint16 _fee = globalState.fee;

925:  uint8 _communityFeeToken0 = globalState.communityFeeToken0;

939:  uint8 _communityFeeToken1 = globalState.communityFeeToken1;

952:  function setCommunityFee(uint8 communityFee0, uint8 communityFee1) external override lock onlyFactoryOwner {

967:  function setLiquidityCooldown(uint32 newLiquidityCooldown) external override onlyFactoryOwner {
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol

```solidity
File: src/core/contracts/base/PoolState.sol

9:    uint160 price;

11:   uint16 fee;

12:   uint16 timepointIndex;

13:   uint8 communityFeeToken0;

14:   uint8 communityFeeToken1;

26:   uint128 public override liquidity;

27:   uint128 internal volumePerLiquidityInBlock;

30:   uint32 public override liquidityCooldown;

37:   mapping(int16 => uint256) public override tickTable;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol

```solidity
File: src/core/contracts/DataStorageOperator.sol

16:   uint128 constant MAX_VOLUME_PER_LIQUIDITY = 100000 << 64;

37:   function initialize(uint32 time, int24 tick) external override onlyPool {

54:   uint32 time,

55:   uint32 secondsAgo,

57:   uint16 index,

58:   uint128 liquidity

66:   uint160 secondsPerLiquidityCumulative,

71:   uint16 oldestIndex;

73:   uint16 nextIndex = index + 1;

89:   uint32 time,

92:   uint16 index,

93:   uint128 liquidity

111:  uint32 time,

113:  uint16 index,

114:  uint128 liquidity

121:  uint16 index,

122:  uint32 blockTimestamp,

124:  uint128 liquidity,

125:  uint128 volumePerLiquidity

126:  ) external override onlyPool returns (uint16 indexUpdated) {

132:  uint128 liquidity,

135:  ) external pure override returns (uint128 volumePerLiquidity) {

151:  uint32 _time,

153:  uint16 _index,

154:  uint128 _liquidity

155:  ) external view override onlyPool returns (uint16 fee) {
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol

```solidity
File: src/core/contracts/libraries/AdaptiveFee.sol

11:   uint16 alpha1;

12:   uint16 alpha2;

13:   uint32 beta1;

14:   uint32 beta2;

15:   uint16 gamma1;

16:   uint16 gamma2;

17:   uint32 volumeBeta;

18:   uint16 volumeGamma;

19:   uint16 baseFee;

29:   ) internal pure returns (uint16 fee) {

46:   uint16 g,

47:   uint16 alpha,

72:   uint16 g,
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol

```solidity
File: src/core/contracts/libraries/Constants.sol

5:    uint8 internal constant RESOLUTION = 96;

9:    uint16 internal constant BASE_FEE = 100;

13:   uint128 internal constant MAX_LIQUIDITY_PER_TICK = 11505743598341114571880798222544994;

15:   uint32 internal constant MAX_LIQUIDITY_COOLDOWN = 1 days;

16:   uint8 internal constant MAX_COMMUNITY_FEE = 250;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/Constants.sol

```solidity
File: src/core/contracts/libraries/DataStorage.sol

12:   uint32 public constant WINDOW = 1 days;

16:   uint32 blockTimestamp;

18:   uint160 secondsPerLiquidityCumulative;

68:   uint32 blockTimestamp,

71:   uint128 liquidity,

73:   uint128 volumePerLiquidity

75:   uint32 delta = blockTimestamp - last.blockTimestamp;

95:   uint32 a,

96:   uint32 b,

97:   uint32 currentTime

107:  uint32 time,

109:  uint16 index,

110:  uint16 oldestIndex,

111:  uint32 lastTimestamp,

114:  uint32 oldestTimestamp = self[oldestIndex].blockTimestamp;

150:  uint32 time,

151:  uint32 target,

152:  uint16 lastIndex,

153:  uint16 oldestIndex

161:  (bool initializedBefore, uint32 timestampBefore) = (beforeOrAt.initialized, beforeOrAt.blockTimestamp);

166:  (bool initializedAfter, uint32 timestampAfter) = (atOrAfter.initialized, atOrAfter.blockTimestamp);

207:  uint32 time,

208:  uint32 secondsAgo,

210:  uint16 index,

211:  uint16 oldestIndex,

212:  uint128 liquidity

214:  uint32 target = time - secondsAgo;

247:  uint32 timepointTimeDelta = atOrAfter.blockTimestamp - beforeOrAt.blockTimestamp;

248:  uint32 targetDelta = target - beforeOrAt.blockTimestamp;

279:  uint32 time,

282:  uint16 index,

283:  uint128 liquidity

299:  uint16 oldestIndex;

301:  uint16 nextIndex = index + 1;

328:  uint32 time,

330:  uint16 index,

331:  uint128 liquidity

333:  uint16 oldestIndex;

335:  uint16 nextIndex = index + 1;

343:  uint32 oldestTimestamp = oldest.blockTimestamp;

366:  uint32 time,

386:  uint16 index,

387:  uint32 blockTimestamp,

389:  uint128 liquidity,

390:  uint128 volumePerLiquidity

391:  ) internal returns (uint16 indexUpdated) {

402:  uint16 oldestIndex;

412:  uint32 _prevLastBlockTimestamp = _prevLast.blockTimestamp;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol

```solidity
File: src/core/contracts/libraries/PriceMovementMath.sol

21:   uint160 price,

22:   uint128 liquidity,

25:   ) internal pure returns (uint160 resultPrice) {

37:   uint160 price,

38:   uint128 liquidity,

41:   ) internal pure returns (uint160 resultPrice) {

46:   uint160 price,

47:   uint128 liquidity,

51:   ) internal pure returns (uint160 resultPrice) {

94:   uint160 to,

95:   uint160 from,

96:   uint128 liquidity

102:  uint160 to,

103:  uint160 from,

104:  uint128 liquidity

110:  uint160 to,

111:  uint160 from,

112:  uint128 liquidity

118:  uint160 to,

119:  uint160 from,

120:  uint128 liquidity

138:  uint160 currentPrice,

139:  uint160 targetPrice,

140:  uint128 liquidity,

142:  uint16 fee

147:  uint160 resultPrice,
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol

```solidity
File: src/core/contracts/libraries/TickManager.sol

18:   uint128 liquidityTotal;

19:   int128 liquidityDelta;

25:   uint160 outerSecondsPerLiquidity;

26:   uint32 outerSecondsSpent;

82:   int128 liquidityDelta,

85:   uint160 secondsPerLiquidityCumulative,

87:   uint32 time,

92:   int128 liquidityDeltaBefore = data.liquidityDelta;

93:   uint128 liquidityTotalBefore = data.liquidityTotal;

95:   uint128 liquidityTotalAfter = LiquidityMath.addDelta(liquidityTotalBefore, liquidityDelta);

134:  uint160 secondsPerLiquidityCumulative,

136:  uint32 time

137:  ) internal returns (int128 liquidityDelta) {
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol

```solidity
File: src/core/contracts/libraries/TickTable.sol

14:   function toggleTick(mapping(int16 => uint256) storage self, int24 tick) internal {

17:   int16 rowNumber;

18:   uint8 bitNumber;

30:   function getSingleSignificantBit(uint256 word) internal pure returns (uint8 singleBitPos) {

46:   function getMostSignificantBit(uint256 word) internal pure returns (uint8 mostBitPos) {

69:   mapping(int16 => uint256) storage self,

83:   int16 rowNumber;

84:   uint8 bitNumber;

101:  int16 rowNumber;

102:  uint8 bitNumber;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol

```solidity
File: src/core/contracts/libraries/TokenDeltaMath.sol

24:   uint160 priceLower,

25:   uint160 priceUpper,

26:   uint128 liquidity,

46:   uint160 priceLower,

47:   uint160 priceUpper,

48:   uint128 liquidity,

62:   uint160 priceLower,

63:   uint160 priceUpper,

64:   int128 liquidity

77:   uint160 priceLower,

78:   uint160 priceUpper,

79:   int128 liquidity
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TokenDeltaMath.sol

### Multiplication/division by a number thats a power of 2 should use bit shifting

`x * 2` is equivalent to `x << 1` and `x / 2` is the same as `x >> 1`. The `MUL` and `DIV` opcodes cost 5 gas, whereas `SHL` and `SHR` only cost 3 gas

_There are **2** instances of this issue:_

```solidity
File: src/core/contracts/DataStorageOperator.sol

138:  if (volume >= 2**192) volumeShifted = (type(uint256).max) / (liquidity > 0 ? liquidity : 1);
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol

```solidity
File: src/core/contracts/libraries/DataStorage.sol

53:   volatility = uint256((K**2 * sumOfSquares + 6 * B * K * sumOfSequence + 6 * dt * B**2) / (6 * dt**2));
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol

### Splitting `require()` statements that use `&&` saves gas

Instead of using `&&` on single `require` check using two `require` checks can save gas

_There are **6** instances of this issue:_

```solidity
File: src/core/contracts/AlgebraFactory.sol

110:  require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol

```solidity
File: src/core/contracts/AlgebraPool.sol

739:  require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');

743:  require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');

953:  require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));

968:  require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol

```solidity
File: src/core/contracts/DataStorageOperator.sol

46:   require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol

### Use `calldata` instead of `memory` for function parameters

If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory. Try to use calldata as a data location because it will avoid copies and also makes sure that the data cannot be modified.

_There are **4** instances of this issue:_

```solidity
File: src/core/contracts/DataStorageOperator.sol

90:   uint32[] memory secondsAgos,
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol

```solidity
File: src/core/contracts/libraries/AdaptiveFee.sol

28:   Configuration memory config
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol

```solidity
File: src/core/contracts/libraries/DataStorage.sol

67:   Timepoint memory last,

280:  uint32[] memory secondsAgos,
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol

### Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`

In the EVM, there is no opcode for `>=` or `<=`. When using greater than or equal, two operations are performed: `>` and `=`. Using strict comparison operators hence saves gas

_There are **28** instances of this issue:_

```solidity
File: src/core/contracts/AlgebraFactory.sol

109:  require(uint256(alpha1) + uint256(alpha2) + uint256(baseFee) <= type(uint16).max, 'Max fee exceeded');
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol

```solidity
File: src/core/contracts/AlgebraPool.sol

229:  require((_blockTimestamp() - lastLiquidityAddTimestamp) >= _liquidityCooldown);

474:  require((amount0 = uint256(amount0Int)) <= receivedAmount0, 'IIAM2');

475:  require((amount1 = uint256(amount1Int)) <= receivedAmount1, 'IIAM2');

608:  require(balance0Before.add(uint256(amount0)) <= balanceToken0(), 'IIA');

614:  require(balance1Before.add(uint256(amount1)) <= balanceToken1(), 'IIA');

921:  require(balance0Before.add(fee0) <= paid0, 'F0');

935:  require(balance1Before.add(fee1) <= paid1, 'F1');

953:  require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));

968:  require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol

```solidity
File: src/core/contracts/DataStorageOperator.sol

45:   require(uint256(_feeConfig.alpha1) + uint256(_feeConfig.alpha2) + uint256(_feeConfig.baseFee) <= type(uint16).max, 'Max fee exceeded');

138:  if (volume >= 2**192) volumeShifted = (type(uint256).max) / (liquidity > 0 ? liquidity : 1);

140:  if (volumeShifted >= MAX_VOLUME_PER_LIQUIDITY) return MAX_VOLUME_PER_LIQUIDITY;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol

```solidity
File: src/core/contracts/libraries/AdaptiveFee.sol

52:   if (x >= 6 * uint256(g)) return alpha;

59:   if (x >= 6 * uint256(g)) return 0;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol

```solidity
File: src/core/contracts/libraries/DataStorage.sol

100:  if (res == b > currentTime) res = a <= b;

156:  uint256 right = lastIndex >= oldestIndex ? lastIndex : lastIndex + UINT16_MODULO;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol

```solidity
File: src/core/contracts/libraries/PriceMovementMath.sol

64:   if (denominator >= liquidityShifted) return uint160(FullMath.mulDivRoundingUp(liquidityShifted, price, denominator));

80:   .add(amount <= type(uint160).max ? (amount << Constants.RESOLUTION) / liquidity : FullMath.mulDiv(amount, Constants.Q96, liquidity))

83:   uint256 quotient = amount <= type(uint160).max

155:  if (amountAvailable >= 0) {

159:  if (amountAvailableAfterFee >= input) {

180:  if (uint256(amountAvailable) >= output) resultPrice = targetPrice;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol

```solidity
File: src/core/contracts/libraries/TickManager.sol

51:   if (currentTick >= bottomTick) {

109:  if (tick <= currentTick) {
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol

```solidity
File: src/core/contracts/libraries/TokenDeltaMath.sol

51:   require(priceUpper >= priceLower);

66:   token0Delta = liquidity >= 0

81:   token1Delta = liquidity >= 0
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TokenDeltaMath.sol

### Use `immutable` & `constant` for state variables that do not change their value

_There are **9** instances of this issue:_

```solidity
File: src/core/contracts/AlgebraPoolDeployer.sol

19:   address private owner;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol

```solidity
File: src/core/contracts/base/PoolState.sol

19:   uint256 public override totalFeeGrowth0Token;

21:   uint256 public override totalFeeGrowth1Token;

23:   GlobalState public override globalState;

26:   uint128 public override liquidity;

27:   uint128 internal volumePerLiquidityInBlock;

30:   uint32 public override liquidityCooldown;

32:   address public override activeIncentive;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol

```solidity
File: src/core/contracts/DataStorageOperator.sol

20:   DataStorage.Timepoint[UINT16_MODULO] public override timepoints;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol

### Use custom errors rather than `revert()`/`require()` strings to save gas

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they're hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

_There are **32** instances of this issue:_

```solidity
File: src/core/contracts/AlgebraFactory.sol

109:  require(uint256(alpha1) + uint256(alpha2) + uint256(baseFee) <= type(uint16).max, 'Max fee exceeded');

110:  require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol

```solidity
File: src/core/contracts/AlgebraPool.sol

60:   require(topTick < TickMath.MAX_TICK + 1, 'TUM');

61:   require(topTick > bottomTick, 'TLU');

62:   require(bottomTick > TickMath.MIN_TICK - 1, 'TLM');

194:  require(globalState.price == 0, 'AI');

224:  require(currentLiquidity > 0, 'NP');

434:  require(liquidityDesired > 0, 'IL');

454:  if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');

455:  if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');

469:  require(liquidityActual > 0, 'IIL2');

474:  require((amount0 = uint256(amount0Int)) <= receivedAmount0, 'IIAM2');

475:  require((amount1 = uint256(amount1Int)) <= receivedAmount1, 'IIAM2');

608:  require(balance0Before.add(uint256(amount0)) <= balanceToken0(), 'IIA');

614:  require(balance1Before.add(uint256(amount1)) <= balanceToken1(), 'IIA');

636:  require(globalState.unlocked, 'LOK');

641:  require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');

645:  require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');

731:  require(unlocked, 'LOK');

733:  require(amountRequired != 0, 'AS');

739:  require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');

743:  require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');

898:  require(_liquidity > 0, 'L');

921:  require(balance0Before.add(fee0) <= paid0, 'F0');

935:  require(balance1Before.add(fee1) <= paid1, 'F1');
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol

```solidity
File: src/core/contracts/base/PoolState.sol

41:   require(globalState.unlocked, 'LOK');
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol

```solidity
File: src/core/contracts/DataStorageOperator.sol

27:   require(msg.sender == pool, 'only pool can call this');

45:   require(uint256(_feeConfig.alpha1) + uint256(_feeConfig.alpha2) + uint256(_feeConfig.baseFee) <= type(uint16).max, 'Max fee exceeded');

46:   require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol

```solidity
File: src/core/contracts/libraries/DataStorage.sol

238:  require(lteConsideringOverflow(self[oldestIndex].blockTimestamp, target, time), 'OLD');
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol

```solidity
File: src/core/contracts/libraries/TickManager.sol

96:   require(liquidityTotalAfter < Constants.MAX_LIQUIDITY_PER_TICK + 1, 'LO');
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol

```solidity
File: src/core/contracts/libraries/TickTable.sol

15:   require(tick % Constants.TICK_SPACING == 0, 'tick is not spaced');
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol

### Use a more recent version of solidity

Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

_There are **13** instances of this issue:_

```solidity
File: src/core/contracts/AlgebraFactory.sol

2:    pragma solidity =0.7.6;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol

```solidity
File: src/core/contracts/AlgebraPool.sol

2:    pragma solidity =0.7.6;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol

```solidity
File: src/core/contracts/AlgebraPoolDeployer.sol

2:    pragma solidity =0.7.6;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol

```solidity
File: src/core/contracts/base/PoolImmutables.sol

2:    pragma solidity =0.7.6;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolImmutables.sol

```solidity
File: src/core/contracts/base/PoolState.sol

2:    pragma solidity =0.7.6;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol

```solidity
File: src/core/contracts/DataStorageOperator.sol

2:    pragma solidity =0.7.6;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol

```solidity
File: src/core/contracts/libraries/AdaptiveFee.sol

2:    pragma solidity =0.7.6;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol

```solidity
File: src/core/contracts/libraries/Constants.sol

2:    pragma solidity =0.7.6;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/Constants.sol

```solidity
File: src/core/contracts/libraries/DataStorage.sol

2:    pragma solidity =0.7.6;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol

```solidity
File: src/core/contracts/libraries/PriceMovementMath.sol

2:    pragma solidity =0.7.6;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol

```solidity
File: src/core/contracts/libraries/TickManager.sol

2:    pragma solidity =0.7.6;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol

```solidity
File: src/core/contracts/libraries/TickTable.sol

2:    pragma solidity =0.7.6;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol

```solidity
File: src/core/contracts/libraries/TokenDeltaMath.sol

2:    pragma solidity =0.7.6;
```

https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TokenDeltaMath.sol
