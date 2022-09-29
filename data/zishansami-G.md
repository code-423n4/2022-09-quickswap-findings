## Gas Optimizations
 *** 


### G-01: pre-increment `++i/--i` costs less gas than post-increment `i++/i--`
Saves 6 gas per loop in a for loop

Total instances of this issue: 1

instance #1
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307
```solidity
src/core/contracts/libraries/DataStorage.sol

307:    for (uint256 i = 0; i < secondsAgos.length; i++) {

```

 *** 


### G-02: Length of the array (`<array>.length`) need not be looked up in every iteration of a for-loop
Reading array length at each iteration of the loop takes total 6 gas (3 for mload and 3 to place memory_offset) in the stack.
Caching the `array.length` saves around `3 gas` per iteration.

Total instances of this issue: 1

instance #1
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307
```solidity
src/core/contracts/libraries/DataStorage.sol

307:    for (uint256 i = 0; i < secondsAgos.length; i++) {

```

 *** 


### G-03: `x += y` costs more gas than `x = x + y` for state variables

Total instances of this issue: 21

instance #1
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L257
```solidity
src/core/contracts/AlgebraPool.sol

257:      _position.fees0 += fees0;

```

instance #2
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L258
```solidity
src/core/contracts/AlgebraPool.sol

258:      _position.fees1 += fees1;

```

instance #3
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L811
```solidity
src/core/contracts/AlgebraPool.sol

811:        communityFeeAmount += delta;

```

instance #4
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L814
```solidity
src/core/contracts/AlgebraPool.sol

814:      if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);

```

instance #5
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L931
```solidity
src/core/contracts/AlgebraPool.sol

931:      totalFeeGrowth0Token += FullMath.mulDiv(paid0 - fees0, Constants.Q128, _liquidity);

```

instance #6
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L945
```solidity
src/core/contracts/AlgebraPool.sol

945:      totalFeeGrowth1Token += FullMath.mulDiv(paid1 - fees1, Constants.Q128, _liquidity);

```

instance #7
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L84
```solidity
src/core/contracts/libraries/AdaptiveFee.sol

84:    res += xLowestDegree * gHighestDegree;

```

instance #8
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L88
```solidity
src/core/contracts/libraries/AdaptiveFee.sol

88:    res += (xLowestDegree * gHighestDegree) / 2;

```

instance #9
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L92
```solidity
src/core/contracts/libraries/AdaptiveFee.sol

92:    res += (xLowestDegree * gHighestDegree) / 6;

```

instance #10
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L96
```solidity
src/core/contracts/libraries/AdaptiveFee.sol

96:    res += (xLowestDegree * gHighestDegree) / 24;

```

instance #11
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L100
```solidity
src/core/contracts/libraries/AdaptiveFee.sol

100:    res += (xLowestDegree * gHighestDegree) / 120;

```

instance #12
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L104
```solidity
src/core/contracts/libraries/AdaptiveFee.sol

104:    res += (xLowestDegree * gHighestDegree) / 720;

```

instance #13
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L107
```solidity
src/core/contracts/libraries/AdaptiveFee.sol

107:    res += (xLowestDegree * g) / 5040 + (xLowestDegree * x) / (40320);

```

instance #14
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L79
```solidity
src/core/contracts/libraries/DataStorage.sol

79:    last.tickCumulative += int56(tick) * delta;

```

instance #15
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L83
```solidity
src/core/contracts/libraries/DataStorage.sol

83:    last.volumePerLiquidityCumulative += volumePerLiquidity;

```

instance #16
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L251
```solidity
src/core/contracts/libraries/DataStorage.sol

251:      beforeOrAt.tickCumulative += ((atOrAfter.tickCumulative - beforeOrAt.tickCumulative) / timepointTimeDelta) * targetDelta;

```

instance #17
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L252-L254
```solidity
src/core/contracts/libraries/DataStorage.sol

252:      beforeOrAt.secondsPerLiquidityCumulative += uint160(
253:        (uint256(atOrAfter.secondsPerLiquidityCumulative - beforeOrAt.secondsPerLiquidityCumulative) * targetDelta) / timepointTimeDelta
254:      );

```

instance #18
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L255
```solidity
src/core/contracts/libraries/DataStorage.sol

255:      beforeOrAt.volatilityCumulative += ((atOrAfter.volatilityCumulative - beforeOrAt.volatilityCumulative) / timepointTimeDelta) * targetDelta;

```

instance #19
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L256-L258
```solidity
src/core/contracts/libraries/DataStorage.sol

256:      beforeOrAt.volumePerLiquidityCumulative +=
257:        ((atOrAfter.volumePerLiquidityCumulative - beforeOrAt.volumePerLiquidityCumulative) / timepointTimeDelta) *
258:        targetDelta;

```

instance #20
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L100
```solidity
src/core/contracts/libraries/TickTable.sol

100:      tick += 1;

```

instance #21
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L115
```solidity
src/core/contracts/libraries/TickTable.sol

115:        tick += int24(255 - bitNumber);

```

 *** 


### G-04: Adding `payable` to functions which are only meant to be called by specific actors like `onlyOwner` will save gas cost
Marking functions payable removes additional checks for whether a payment was provided, hence reducing gas cost

Total instances of this issue: 8

instance #1
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L77
```solidity
src/core/contracts/AlgebraFactory.sol

77:  function setOwner(address _owner) external override onlyOwner {

```

instance #2
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L84
```solidity
src/core/contracts/AlgebraFactory.sol

84:  function setFarmingAddress(address _farmingAddress) external override onlyOwner {

```

instance #3
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L91
```solidity
src/core/contracts/AlgebraFactory.sol

91:  function setVaultAddress(address _vaultAddress) external override onlyOwner {

```

instance #4
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L98-L108
```solidity
src/core/contracts/AlgebraFactory.sol

98:  function setBaseFeeConfiguration(
99:    uint16 alpha1,
100:    uint16 alpha2,
101:    uint32 beta1,
102:    uint32 beta2,
103:    uint16 gamma1,
104:    uint16 gamma2,
105:    uint32 volumeBeta,
106:    uint16 volumeGamma,
107:    uint16 baseFee
108:  ) external override onlyOwner {

```

instance #5
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L952
```solidity
src/core/contracts/AlgebraPool.sol

952:  function setCommunityFee(uint8 communityFee0, uint8 communityFee1) external override lock onlyFactoryOwner {

```

instance #6
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L967
```solidity
src/core/contracts/AlgebraPool.sol

967:  function setLiquidityCooldown(uint32 newLiquidityCooldown) external override onlyFactoryOwner {

```

instance #7
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L36
```solidity
src/core/contracts/AlgebraPoolDeployer.sol

36:  function setFactory(address _factory) external override onlyOwner {

```

instance #8
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L44-L49
```solidity
src/core/contracts/AlgebraPoolDeployer.sol

44:  function deploy(
45:    address dataStorage,
46:    address _factory,
47:    address token0,
48:    address token1
49:  ) external override onlyFactory returns (address pool) {

```

 *** 


### G-05: No need to initialize non-constant/non-immutable variables to zero
Since the default value is already zero, overwriting is not required.
Saves `8 gas` per instance.

Total instances of this issue: 1

instance #1
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307
```solidity
src/core/contracts/libraries/DataStorage.sol

307:    for (uint256 i = 0; i < secondsAgos.length; i++) {

```

 *** 


### G-06: `require()` containing multiple checks joined with && should be broken into multiple require statements

Total instances of this issue: 6

instance #1
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L110
```solidity
src/core/contracts/AlgebraFactory.sol

110:    require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');

```

instance #2
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L739
```solidity
src/core/contracts/AlgebraPool.sol

739:        require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');

```

instance #3
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L743
```solidity
src/core/contracts/AlgebraPool.sol

743:        require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');

```

instance #4
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L953
```solidity
src/core/contracts/AlgebraPool.sol

953:    require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));

```

instance #5
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L968
```solidity
src/core/contracts/AlgebraPool.sol

968:    require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);

```

instance #6
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L46
```solidity
src/core/contracts/DataStorageOperator.sol

46:    require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');

```

 *** 


### G-07: Using uints/ints smaller than 256 bits increases overhead
Gas usage becomes higher with uint/int smaller than 256 bits because EVM operates on 32 bytes and uses additional operations to reduce the size from 32 bytes to the target size.

Total instances of this issue: 140

instance #1
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L98-L108
```solidity
src/core/contracts/AlgebraFactory.sol

98:  function setBaseFeeConfiguration(
99:    uint16 alpha1,
100:    uint16 alpha2,
101:    uint32 beta1,
102:    uint32 beta2,
103:    uint16 gamma1,
104:    uint16 gamma2,
105:    uint32 volumeBeta,
106:    uint16 volumeGamma,
107:    uint16 baseFee
108:  ) external override onlyOwner {

```

instance #2
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L38
```solidity
src/core/contracts/AlgebraPool.sol

38:  using TickTable for mapping(int16 => uint256);

```

instance #3
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L39
```solidity
src/core/contracts/AlgebraPool.sol

39:  using TickManager for mapping(int24 => TickManager.Tick);

```

instance #4
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L59
```solidity
src/core/contracts/AlgebraPool.sol

59:  modifier onlyValidTicks(int24 bottomTick, int24 topTick) {

```

instance #5
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L79-L92
```solidity
src/core/contracts/AlgebraPool.sol

79:  function timepoints(uint256 index)
80:    external
81:    view
82:    override
83:    returns (
84:      bool initialized,
85:      uint32 blockTimestamp,
86:      int56 tickCumulative,
87:      uint160 secondsPerLiquidityCumulative,
88:      uint88 volatilityCumulative,
89:      int24 averageTick,
90:      uint144 volumePerLiquidityCumulative
91:    )
92:  {

```

instance #6
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L97
```solidity
src/core/contracts/AlgebraPool.sol

97:    int56 tickCumulative;

```

instance #7
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L98
```solidity
src/core/contracts/AlgebraPool.sol

98:    uint160 outerSecondPerLiquidity;

```

instance #8
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L99
```solidity
src/core/contracts/AlgebraPool.sol

99:    uint32 outerSecondsSpent;

```

instance #9
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L103-L113
```solidity
src/core/contracts/AlgebraPool.sol

103:  function getInnerCumulatives(int24 bottomTick, int24 topTick)
104:    external
105:    view
106:    override
107:    onlyValidTicks(bottomTick, topTick)
108:    returns (
109:      int56 innerTickCumulative,
110:      uint160 innerSecondsSpentPerLiquidity,
111:      uint32 innerSecondsSpent
112:    )
113:  {

```

instance #10
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L137
```solidity
src/core/contracts/AlgebraPool.sol

137:    (int24 currentTick, uint16 currentTimepointIndex) = (globalState.tick, globalState.timepointIndex);

```

instance #11
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L148
```solidity
src/core/contracts/AlgebraPool.sol

148:      uint32 globalTime = _blockTimestamp();

```

instance #12
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L149-L155
```solidity
src/core/contracts/AlgebraPool.sol

149:      (int56 globalTickCumulative, uint160 globalSecondsPerLiquidityCumulative, , ) = _getSingleTimepoint(
150:        globalTime,
151:        0,
152:        currentTick,
153:        currentTimepointIndex,
154:        liquidity
155:      );

```

instance #13
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L193
```solidity
src/core/contracts/AlgebraPool.sol

193:  function initialize(uint160 initialPrice) external override {

```

instance #14
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L196
```solidity
src/core/contracts/AlgebraPool.sol

196:    int24 tick = TickMath.getTickAtSqrtRatio(initialPrice);

```

instance #15
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L198
```solidity
src/core/contracts/AlgebraPool.sol

198:    uint32 timestamp = _blockTimestamp();

```

instance #16
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L215-L220
```solidity
src/core/contracts/AlgebraPool.sol

215:  function _recalculatePosition(
216:    Position storage _position,
217:    int128 liquidityDelta,
218:    uint256 innerFeeGrowth0Token,
219:    uint256 innerFeeGrowth1Token
220:  ) internal {

```

instance #17
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L221
```solidity
src/core/contracts/AlgebraPool.sol

221:    (uint128 currentLiquidity, uint32 lastLiquidityAddTimestamp) = (_position.liquidity, _position.lastLiquidityAddTimestamp);

```

instance #18
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L227
```solidity
src/core/contracts/AlgebraPool.sol

227:        uint32 _liquidityCooldown = liquidityCooldown;

```

instance #19
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L234
```solidity
src/core/contracts/AlgebraPool.sol

234:      uint128 liquidityNext = LiquidityMath.addDelta(currentLiquidity, liquidityDelta);

```

instance #20
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L244
```solidity
src/core/contracts/AlgebraPool.sol

244:    uint128 fees0;

```

instance #21
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L249
```solidity
src/core/contracts/AlgebraPool.sol

249:    uint128 fees1;

```

instance #22
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L274-L286
```solidity
src/core/contracts/AlgebraPool.sol

274:  function _updatePositionTicksAndFees(
275:    address owner,
276:    int24 bottomTick,
277:    int24 topTick,
278:    int128 liquidityDelta
279:  )
280:    private
281:    returns (
282:      Position storage position,
283:      int256 amount0,
284:      int256 amount1
285:    )
286:  {

```

instance #23
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L296
```solidity
src/core/contracts/AlgebraPool.sol

296:      uint32 time = _blockTimestamp();

```

instance #24
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L297
```solidity
src/core/contracts/AlgebraPool.sol

297:      (int56 tickCumulative, uint160 secondsPerLiquidityCumulative, , ) = _getSingleTimepoint(time, 0, cache.tick, cache.timepointIndex, liquidity);

```

instance #25
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L351
```solidity
src/core/contracts/AlgebraPool.sol

351:      int128 globalLiquidityDelta;

```

instance #26
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L354
```solidity
src/core/contracts/AlgebraPool.sol

354:        uint128 liquidityBefore = liquidity;

```

instance #27
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L355
```solidity
src/core/contracts/AlgebraPool.sol

355:        uint16 newTimepointIndex = _writeTimepoint(cache.timepointIndex, _blockTimestamp(), cache.tick, liquidityBefore, volumePerLiquidityInBlock);

```

instance #28
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L366-L380
```solidity
src/core/contracts/AlgebraPool.sol

366:  function _getAmountsForLiquidity(
367:    int24 bottomTick,
368:    int24 topTick,
369:    int128 liquidityDelta,
370:    int24 currentTick,
371:    uint160 currentPrice
372:  )
373:    private
374:    pure
375:    returns (
376:      int256 amount0,
377:      int256 amount1,
378:      int128 globalLiquidityDelta
379:    )
380:  {

```

instance #29
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L403-L407
```solidity
src/core/contracts/AlgebraPool.sol

403:  function getOrCreatePosition(
404:    address owner,
405:    int24 bottomTick,
406:    int24 topTick
407:  ) private view returns (Position storage) {

```

instance #30
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L416-L433
```solidity
src/core/contracts/AlgebraPool.sol

416:  function mint(
417:    address sender,
418:    address recipient,
419:    int24 bottomTick,
420:    int24 topTick,
421:    uint128 liquidityDesired,
422:    bytes calldata data
423:  )
424:    external
425:    override
426:    lock
427:    onlyValidTicks(bottomTick, topTick)
428:    returns (
429:      uint256 amount0,
430:      uint256 amount1,
431:      uint128 liquidityActual
432:    )
433:  {

```

instance #31
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L463
```solidity
src/core/contracts/AlgebraPool.sol

463:      uint128 liquidityForRA1 = uint128(FullMath.mulDiv(uint256(liquidityActual), receivedAmount1, amount1));

```

instance #32
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L488-L494
```solidity
src/core/contracts/AlgebraPool.sol

488:  function collect(
489:    address recipient,
490:    int24 bottomTick,
491:    int24 topTick,
492:    uint128 amount0Requested,
493:    uint128 amount1Requested
494:  ) external override lock returns (uint128 amount0, uint128 amount1) {

```

instance #33
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L496
```solidity
src/core/contracts/AlgebraPool.sol

496:    (uint128 positionFees0, uint128 positionFees1) = (position.fees0, position.fees1);

```

instance #34
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L513-L517
```solidity
src/core/contracts/AlgebraPool.sol

513:  function burn(
514:    int24 bottomTick,
515:    int24 topTick,
516:    uint128 amount
517:  ) external override lock onlyValidTicks(bottomTick, topTick) returns (uint256 amount0, uint256 amount1) {

```

instance #35
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L536-L541
```solidity
src/core/contracts/AlgebraPool.sol

536:  function _getNewFee(
537:    uint32 _time,
538:    int24 _tick,
539:    uint16 _index,
540:    uint128 _liquidity
541:  ) private returns (uint16 newFee) {

```

instance #36
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L551-L557
```solidity
src/core/contracts/AlgebraPool.sol

551:  function _writeTimepoint(
552:    uint16 timepointIndex,
553:    uint32 blockTimestamp,
554:    int24 tick,
555:    uint128 liquidity,
556:    uint128 volumePerLiquidityInBlock
557:  ) private returns (uint16 newTimepointIndex) {

```

instance #37
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L561-L576
```solidity
src/core/contracts/AlgebraPool.sol

561:  function _getSingleTimepoint(
562:    uint32 blockTimestamp,
563:    uint32 secondsAgo,
564:    int24 startTick,
565:    uint16 timepointIndex,
566:    uint128 liquidityStart
567:  )
568:    private
569:    view
570:    returns (
571:      int56 tickCumulative,
572:      uint160 secondsPerLiquidityCumulative,
573:      uint112 volatilityCumulative,
574:      uint256 volumePerAvgLiquidity
575:    )
576:  {

```

instance #38
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L589-L595
```solidity
src/core/contracts/AlgebraPool.sol

589:  function swap(
590:    address recipient,
591:    bool zeroToOne,
592:    int256 amountRequired,
593:    uint160 limitSqrtPrice,
594:    bytes calldata data
595:  ) external override returns (int256 amount0, int256 amount1) {

```

instance #39
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L596
```solidity
src/core/contracts/AlgebraPool.sol

596:    uint160 currentPrice;

```

instance #40
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L597
```solidity
src/core/contracts/AlgebraPool.sol

597:    int24 currentTick;

```

instance #41
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L598
```solidity
src/core/contracts/AlgebraPool.sol

598:    uint128 currentLiquidity;

```

instance #42
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L626-L633
```solidity
src/core/contracts/AlgebraPool.sol

626:  function swapSupportingFeeOnInputTokens(
627:    address sender,
628:    address recipient,
629:    bool zeroToOne,
630:    int256 amountRequired,
631:    uint160 limitSqrtPrice,
632:    bytes calldata data
633:  ) external override returns (int256 amount0, int256 amount1) {

```

instance #43
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L649
```solidity
src/core/contracts/AlgebraPool.sol

649:    uint160 currentPrice;

```

instance #44
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L650
```solidity
src/core/contracts/AlgebraPool.sol

650:    int24 currentTick;

```

instance #45
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L651
```solidity
src/core/contracts/AlgebraPool.sol

651:    uint128 currentLiquidity;

```

instance #46
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L677
```solidity
src/core/contracts/AlgebraPool.sol

677:    uint128 volumePerLiquidityInBlock;

```

instance #47
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L703-L717
```solidity
src/core/contracts/AlgebraPool.sol

703:  function _calculateSwapAndLock(
704:    bool zeroToOne,
705:    int256 amountRequired,
706:    uint160 limitSqrtPrice
707:  )
708:    private
709:    returns (
710:      int256 amount0,
711:      int256 amount1,
712:      uint160 currentPrice,
713:      int24 currentTick,
714:      uint128 currentLiquidity,
715:      uint256 communityFeeAmount
716:    )
717:  {

```

instance #48
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L718
```solidity
src/core/contracts/AlgebraPool.sol

718:    uint32 blockTimestamp;

```

instance #49
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L763-L769
```solidity
src/core/contracts/AlgebraPool.sol

763:      uint16 newTimepointIndex = _writeTimepoint(
764:        cache.timepointIndex,
765:        blockTimestamp,
766:        cache.startTick,
767:        currentLiquidity,
768:        cache.volumePerLiquidityInBlock
769:      );

```

instance #50
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L835
```solidity
src/core/contracts/AlgebraPool.sol

835:          int128 liquidityDelta;

```

instance #51
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L897
```solidity
src/core/contracts/AlgebraPool.sol

897:    uint128 _liquidity = liquidity;

```

instance #52
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L900
```solidity
src/core/contracts/AlgebraPool.sol

900:    uint16 _fee = globalState.fee;

```

instance #53
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L925
```solidity
src/core/contracts/AlgebraPool.sol

925:      uint8 _communityFeeToken0 = globalState.communityFeeToken0;

```

instance #54
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L939
```solidity
src/core/contracts/AlgebraPool.sol

939:      uint8 _communityFeeToken1 = globalState.communityFeeToken1;

```

instance #55
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L952
```solidity
src/core/contracts/AlgebraPool.sol

952:  function setCommunityFee(uint8 communityFee0, uint8 communityFee1) external override lock onlyFactoryOwner {

```

instance #56
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L967
```solidity
src/core/contracts/AlgebraPool.sol

967:  function setLiquidityCooldown(uint32 newLiquidityCooldown) external override onlyFactoryOwner {

```

instance #57
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L37
```solidity
src/core/contracts/DataStorageOperator.sol

37:  function initialize(uint32 time, int24 tick) external override onlyPool {

```

instance #58
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L53-L70
```solidity
src/core/contracts/DataStorageOperator.sol

53:  function getSingleTimepoint(
54:    uint32 time,
55:    uint32 secondsAgo,
56:    int24 tick,
57:    uint16 index,
58:    uint128 liquidity
59:  )
60:    external
61:    view
62:    override
63:    onlyPool
64:    returns (
65:      int56 tickCumulative,
66:      uint160 secondsPerLiquidityCumulative,
67:      uint112 volatilityCumulative,
68:      uint256 volumePerAvgLiquidity
69:    )
70:  {

```

instance #59
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L71
```solidity
src/core/contracts/DataStorageOperator.sol

71:    uint16 oldestIndex;

```

instance #60
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L88-L105
```solidity
src/core/contracts/DataStorageOperator.sol

88:  function getTimepoints(
89:    uint32 time,
90:    uint32[] memory secondsAgos,
91:    int24 tick,
92:    uint16 index,
93:    uint128 liquidity
94:  )
95:    external
96:    view
97:    override
98:    onlyPool
99:    returns (
100:      int56[] memory tickCumulatives,
101:      uint160[] memory secondsPerLiquidityCumulatives,
102:      uint112[] memory volatilityCumulatives,
103:      uint256[] memory volumePerAvgLiquiditys
104:    )
105:  {

```

instance #61
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L110-L115
```solidity
src/core/contracts/DataStorageOperator.sol

110:  function getAverages(
111:    uint32 time,
112:    int24 tick,
113:    uint16 index,
114:    uint128 liquidity
115:  ) external view override onlyPool returns (uint112 TWVolatilityAverage, uint256 TWVolumePerLiqAverage) {

```

instance #62
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L120-L126
```solidity
src/core/contracts/DataStorageOperator.sol

120:  function write(
121:    uint16 index,
122:    uint32 blockTimestamp,
123:    int24 tick,
124:    uint128 liquidity,
125:    uint128 volumePerLiquidity
126:  ) external override onlyPool returns (uint16 indexUpdated) {

```

instance #63
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L131-L135
```solidity
src/core/contracts/DataStorageOperator.sol

131:  function calculateVolumePerLiquidity(
132:    uint128 liquidity,
133:    int256 amount0,
134:    int256 amount1
135:  ) external pure override returns (uint128 volumePerLiquidity) {

```

instance #64
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L150-L155
```solidity
src/core/contracts/DataStorageOperator.sol

150:  function getFee(
151:    uint32 _time,
152:    int24 _tick,
153:    uint16 _index,
154:    uint128 _liquidity
155:  ) external view override onlyPool returns (uint16 fee) {

```

instance #65
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L156
```solidity
src/core/contracts/DataStorageOperator.sol

156:    (uint88 volatilityAverage, uint256 volumePerLiqAverage) = timepoints.getAverages(_time, _tick, _index, _liquidity);

```

instance #66
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L25-L29
```solidity
src/core/contracts/libraries/AdaptiveFee.sol

25:  function getFee(
26:    uint88 volatility,
27:    uint256 volumePerLiquidity,
28:    Configuration memory config
29:  ) internal pure returns (uint16 fee) {

```

instance #67
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L44-L49
```solidity
src/core/contracts/libraries/AdaptiveFee.sol

44:  function sigmoid(
45:    uint256 x,
46:    uint16 g,
47:    uint16 alpha,
48:    uint256 beta
49:  ) internal pure returns (uint256 res) {

```

instance #68
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L70-L74
```solidity
src/core/contracts/libraries/AdaptiveFee.sol

70:  function exp(
71:    uint256 x,
72:    uint16 g,
73:    uint256 gHighestDegree
74:  ) internal pure returns (uint256 res) {

```

instance #69
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/Constants.sol#L5
```solidity
src/core/contracts/libraries/Constants.sol

5:  uint8 internal constant RESOLUTION = 96;

```

instance #70
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/Constants.sol#L9
```solidity
src/core/contracts/libraries/Constants.sol

9:  uint16 internal constant BASE_FEE = 100;

```

instance #71
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/Constants.sol#L10
```solidity
src/core/contracts/libraries/Constants.sol

10:  int24 internal constant TICK_SPACING = 60;

```

instance #72
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/Constants.sol#L13
```solidity
src/core/contracts/libraries/Constants.sol

13:  uint128 internal constant MAX_LIQUIDITY_PER_TICK = 11505743598341114571880798222544994;

```

instance #73
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/Constants.sol#L15
```solidity
src/core/contracts/libraries/Constants.sol

15:  uint32 internal constant MAX_LIQUIDITY_COOLDOWN = 1 days;

```

instance #74
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/Constants.sol#L16
```solidity
src/core/contracts/libraries/Constants.sol

16:  uint8 internal constant MAX_COMMUNITY_FEE = 250;

```

instance #75
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L12
```solidity
src/core/contracts/libraries/DataStorage.sol

12:  uint32 public constant WINDOW = 1 days;

```

instance #76
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L66-L74
```solidity
src/core/contracts/libraries/DataStorage.sol

66:  function createNewTimepoint(
67:    Timepoint memory last,
68:    uint32 blockTimestamp,
69:    int24 tick,
70:    int24 prevTick,
71:    uint128 liquidity,
72:    int24 averageTick,
73:    uint128 volumePerLiquidity
74:  ) private pure returns (Timepoint memory) {

```

instance #77
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L75
```solidity
src/core/contracts/libraries/DataStorage.sol

75:    uint32 delta = blockTimestamp - last.blockTimestamp;

```

instance #78
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L94-L98
```solidity
src/core/contracts/libraries/DataStorage.sol

94:  function lteConsideringOverflow(
95:    uint32 a,
96:    uint32 b,
97:    uint32 currentTime
98:  ) private pure returns (bool res) {

```

instance #79
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L105-L113
```solidity
src/core/contracts/libraries/DataStorage.sol

105:  function _getAverageTick(
106:    Timepoint[UINT16_MODULO] storage self,
107:    uint32 time,
108:    int24 tick,
109:    uint16 index,
110:    uint16 oldestIndex,
111:    uint32 lastTimestamp,
112:    int56 lastTickCumulative
113:  ) internal view returns (int256 avgTick) {

```

instance #80
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L114
```solidity
src/core/contracts/libraries/DataStorage.sol

114:    uint32 oldestTimestamp = self[oldestIndex].blockTimestamp;

```

instance #81
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L115
```solidity
src/core/contracts/libraries/DataStorage.sol

115:    int56 oldestTickCumulative = self[oldestIndex].tickCumulative;

```

instance #82
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L148-L154
```solidity
src/core/contracts/libraries/DataStorage.sol

148:  function binarySearch(
149:    Timepoint[UINT16_MODULO] storage self,
150:    uint32 time,
151:    uint32 target,
152:    uint16 lastIndex,
153:    uint16 oldestIndex
154:  ) private view returns (Timepoint storage beforeOrAt, Timepoint storage atOrAfter) {

```

instance #83
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L161
```solidity
src/core/contracts/libraries/DataStorage.sol

161:      (bool initializedBefore, uint32 timestampBefore) = (beforeOrAt.initialized, beforeOrAt.blockTimestamp);

```

instance #84
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L166
```solidity
src/core/contracts/libraries/DataStorage.sol

166:          (bool initializedAfter, uint32 timestampAfter) = (atOrAfter.initialized, atOrAfter.blockTimestamp);

```

instance #85
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L205-L213
```solidity
src/core/contracts/libraries/DataStorage.sol

205:  function getSingleTimepoint(
206:    Timepoint[UINT16_MODULO] storage self,
207:    uint32 time,
208:    uint32 secondsAgo,
209:    int24 tick,
210:    uint16 index,
211:    uint16 oldestIndex,
212:    uint128 liquidity
213:  ) internal view returns (Timepoint memory targetTimepoint) {

```

instance #86
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L214
```solidity
src/core/contracts/libraries/DataStorage.sol

214:    uint32 target = time - secondsAgo;

```

instance #87
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L223
```solidity
src/core/contracts/libraries/DataStorage.sol

223:        int24 avgTick = int24(_getAverageTick(self, time, tick, index, oldestIndex, last.blockTimestamp, last.tickCumulative));

```

instance #88
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L224
```solidity
src/core/contracts/libraries/DataStorage.sol

224:        int24 prevTick = tick;

```

instance #89
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L247
```solidity
src/core/contracts/libraries/DataStorage.sol

247:      uint32 timepointTimeDelta = atOrAfter.blockTimestamp - beforeOrAt.blockTimestamp;

```

instance #90
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L248
```solidity
src/core/contracts/libraries/DataStorage.sol

248:      uint32 targetDelta = target - beforeOrAt.blockTimestamp;

```

instance #91
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L277-L293
```solidity
src/core/contracts/libraries/DataStorage.sol

277:  function getTimepoints(
278:    Timepoint[UINT16_MODULO] storage self,
279:    uint32 time,
280:    uint32[] memory secondsAgos,
281:    int24 tick,
282:    uint16 index,
283:    uint128 liquidity
284:  )
285:    internal
286:    view
287:    returns (
288:      int56[] memory tickCumulatives,
289:      uint160[] memory secondsPerLiquidityCumulatives,
290:      uint112[] memory volatilityCumulatives,
291:      uint256[] memory volumePerAvgLiquiditys
292:    )
293:  {

```

instance #92
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L299
```solidity
src/core/contracts/libraries/DataStorage.sol

299:    uint16 oldestIndex;

```

instance #93
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L326-L332
```solidity
src/core/contracts/libraries/DataStorage.sol

326:  function getAverages(
327:    Timepoint[UINT16_MODULO] storage self,
328:    uint32 time,
329:    int24 tick,
330:    uint16 index,
331:    uint128 liquidity
332:  ) internal view returns (uint88 volatilityAverage, uint256 volumePerLiqAverage) {

```

instance #94
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L333
```solidity
src/core/contracts/libraries/DataStorage.sol

333:    uint16 oldestIndex;

```

instance #95
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L343
```solidity
src/core/contracts/libraries/DataStorage.sol

343:    uint32 oldestTimestamp = oldest.blockTimestamp;

```

instance #96
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L351
```solidity
src/core/contracts/libraries/DataStorage.sol

351:      uint88 _oldestVolatilityCumulative = oldest.volatilityCumulative;

```

instance #97
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L352
```solidity
src/core/contracts/libraries/DataStorage.sol

352:      uint144 _oldestVolumePerLiquidityCumulative = oldest.volumePerLiquidityCumulative;

```

instance #98
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L364-L368
```solidity
src/core/contracts/libraries/DataStorage.sol

364:  function initialize(
365:    Timepoint[UINT16_MODULO] storage self,
366:    uint32 time,
367:    int24 tick
368:  ) internal {

```

instance #99
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L384-L391
```solidity
src/core/contracts/libraries/DataStorage.sol

384:  function write(
385:    Timepoint[UINT16_MODULO] storage self,
386:    uint16 index,
387:    uint32 blockTimestamp,
388:    int24 tick,
389:    uint128 liquidity,
390:    uint128 volumePerLiquidity
391:  ) internal returns (uint16 indexUpdated) {

```

instance #100
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L402
```solidity
src/core/contracts/libraries/DataStorage.sol

402:    uint16 oldestIndex;

```

instance #101
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L408
```solidity
src/core/contracts/libraries/DataStorage.sol

408:    int24 avgTick = int24(_getAverageTick(self, blockTimestamp, tick, index, oldestIndex, last.blockTimestamp, last.tickCumulative));

```

instance #102
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L409
```solidity
src/core/contracts/libraries/DataStorage.sol

409:    int24 prevTick = tick;

```

instance #103
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L412
```solidity
src/core/contracts/libraries/DataStorage.sol

412:      uint32 _prevLastBlockTimestamp = _prevLast.blockTimestamp;

```

instance #104
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L413
```solidity
src/core/contracts/libraries/DataStorage.sol

413:      int56 _prevLastTickCumulative = _prevLast.tickCumulative;

```

instance #105
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L20-L25
```solidity
src/core/contracts/libraries/PriceMovementMath.sol

20:  function getNewPriceAfterInput(
21:    uint160 price,
22:    uint128 liquidity,
23:    uint256 input,
24:    bool zeroToOne
25:  ) internal pure returns (uint160 resultPrice) {

```

instance #106
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L36-L41
```solidity
src/core/contracts/libraries/PriceMovementMath.sol

36:  function getNewPriceAfterOutput(
37:    uint160 price,
38:    uint128 liquidity,
39:    uint256 output,
40:    bool zeroToOne
41:  ) internal pure returns (uint160 resultPrice) {

```

instance #107
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L45-L51
```solidity
src/core/contracts/libraries/PriceMovementMath.sol

45:  function getNewPrice(
46:    uint160 price,
47:    uint128 liquidity,
48:    uint256 amount,
49:    bool zeroToOne,
50:    bool fromInput
51:  ) internal pure returns (uint160 resultPrice) {

```

instance #108
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L93-L97
```solidity
src/core/contracts/libraries/PriceMovementMath.sol

93:  function getTokenADelta01(
94:    uint160 to,
95:    uint160 from,
96:    uint128 liquidity
97:  ) internal pure returns (uint256) {

```

instance #109
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L101-L105
```solidity
src/core/contracts/libraries/PriceMovementMath.sol

101:  function getTokenADelta10(
102:    uint160 to,
103:    uint160 from,
104:    uint128 liquidity
105:  ) internal pure returns (uint256) {

```

instance #110
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L109-L113
```solidity
src/core/contracts/libraries/PriceMovementMath.sol

109:  function getTokenBDelta01(
110:    uint160 to,
111:    uint160 from,
112:    uint128 liquidity
113:  ) internal pure returns (uint256) {

```

instance #111
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L117-L121
```solidity
src/core/contracts/libraries/PriceMovementMath.sol

117:  function getTokenBDelta10(
118:    uint160 to,
119:    uint160 from,
120:    uint128 liquidity
121:  ) internal pure returns (uint256) {

```

instance #112
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L136-L152
```solidity
src/core/contracts/libraries/PriceMovementMath.sol

136:  function movePriceTowardsTarget(
137:    bool zeroToOne,
138:    uint160 currentPrice,
139:    uint160 targetPrice,
140:    uint128 liquidity,
141:    int256 amountAvailable,
142:    uint16 fee
143:  )
144:    internal
145:    pure
146:    returns (
147:      uint160 resultPrice,
148:      uint256 input,
149:      uint256 output,
150:      uint256 feeAmount
151:    )
152:  {

```

instance #113
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L39-L46
```solidity
src/core/contracts/libraries/TickManager.sol

39:  function getInnerFeeGrowth(
40:    mapping(int24 => Tick) storage self,
41:    int24 bottomTick,
42:    int24 topTick,
43:    int24 currentTick,
44:    uint256 totalFeeGrowth0Token,
45:    uint256 totalFeeGrowth1Token
46:  ) internal view returns (uint256 innerFeeGrowth0Token, uint256 innerFeeGrowth1Token) {

```

instance #114
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L78-L89
```solidity
src/core/contracts/libraries/TickManager.sol

78:  function update(
79:    mapping(int24 => Tick) storage self,
80:    int24 tick,
81:    int24 currentTick,
82:    int128 liquidityDelta,
83:    uint256 totalFeeGrowth0Token,
84:    uint256 totalFeeGrowth1Token,
85:    uint160 secondsPerLiquidityCumulative,
86:    int56 tickCumulative,
87:    uint32 time,
88:    bool upper
89:  ) internal returns (bool flipped) {

```

instance #115
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L92
```solidity
src/core/contracts/libraries/TickManager.sol

92:    int128 liquidityDeltaBefore = data.liquidityDelta;

```

instance #116
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L93
```solidity
src/core/contracts/libraries/TickManager.sol

93:    uint128 liquidityTotalBefore = data.liquidityTotal;

```

instance #117
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L95
```solidity
src/core/contracts/libraries/TickManager.sol

95:    uint128 liquidityTotalAfter = LiquidityMath.addDelta(liquidityTotalBefore, liquidityDelta);

```

instance #118
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L129-L137
```solidity
src/core/contracts/libraries/TickManager.sol

129:  function cross(
130:    mapping(int24 => Tick) storage self,
131:    int24 tick,
132:    uint256 totalFeeGrowth0Token,
133:    uint256 totalFeeGrowth1Token,
134:    uint160 secondsPerLiquidityCumulative,
135:    int56 tickCumulative,
136:    uint32 time
137:  ) internal returns (int128 liquidityDelta) {

```

instance #119
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L14
```solidity
src/core/contracts/libraries/TickTable.sol

14:  function toggleTick(mapping(int16 => uint256) storage self, int24 tick) internal {

```

instance #120
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L17
```solidity
src/core/contracts/libraries/TickTable.sol

17:    int16 rowNumber;

```

instance #121
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L18
```solidity
src/core/contracts/libraries/TickTable.sol

18:    uint8 bitNumber;

```

instance #122
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L30
```solidity
src/core/contracts/libraries/TickTable.sol

30:  function getSingleSignificantBit(uint256 word) internal pure returns (uint8 singleBitPos) {

```

instance #123
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L46
```solidity
src/core/contracts/libraries/TickTable.sol

46:  function getMostSignificantBit(uint256 word) internal pure returns (uint8 mostBitPos) {

```

instance #124
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L68-L72
```solidity
src/core/contracts/libraries/TickTable.sol

68:  function nextTickInTheSameRow(
69:    mapping(int16 => uint256) storage self,
70:    int24 tick,
71:    bool lte
72:  ) internal view returns (int24 nextTick, bool initialized) {

```

instance #125
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L74
```solidity
src/core/contracts/libraries/TickTable.sol

74:      int24 tickSpacing = Constants.TICK_SPACING;

```

instance #126
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L83
```solidity
src/core/contracts/libraries/TickTable.sol

83:      int16 rowNumber;

```

instance #127
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L84
```solidity
src/core/contracts/libraries/TickTable.sol

84:      uint8 bitNumber;

```

instance #128
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L101
```solidity
src/core/contracts/libraries/TickTable.sol

101:      int16 rowNumber;

```

instance #129
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L102
```solidity
src/core/contracts/libraries/TickTable.sol

102:      uint8 bitNumber;

```

instance #130
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L121
```solidity
src/core/contracts/libraries/TickTable.sol

121:  function uncompressAndBoundTick(int24 tick) private pure returns (int24 boundedTick) {

```

instance #131
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TokenDeltaMath.sol#L23-L28
```solidity
src/core/contracts/libraries/TokenDeltaMath.sol

23:  function getToken0Delta(
24:    uint160 priceLower,
25:    uint160 priceUpper,
26:    uint128 liquidity,
27:    bool roundUp
28:  ) internal pure returns (uint256 token0Delta) {

```

instance #132
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TokenDeltaMath.sol#L45-L50
```solidity
src/core/contracts/libraries/TokenDeltaMath.sol

45:  function getToken1Delta(
46:    uint160 priceLower,
47:    uint160 priceUpper,
48:    uint128 liquidity,
49:    bool roundUp
50:  ) internal pure returns (uint256 token1Delta) {

```

instance #133
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TokenDeltaMath.sol#L61-L65
```solidity
src/core/contracts/libraries/TokenDeltaMath.sol

61:  function getToken0Delta(
62:    uint160 priceLower,
63:    uint160 priceUpper,
64:    int128 liquidity
65:  ) internal pure returns (int256 token0Delta) {

```

instance #134
Permalink: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TokenDeltaMath.sol#L76-L80
```solidity
src/core/contracts/libraries/TokenDeltaMath.sol

76:  function getToken1Delta(
77:    uint160 priceLower,
78:    uint160 priceUpper,
79:    int128 liquidity
80:  ) internal pure returns (int256 token1Delta) {

```

instance #135
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolState.sol#L14
```solidity
src/core/contracts/base/PoolState.sol

14:    uint8 communityFeeToken1;

```

instance #136
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolState.sol#L26
```solidity
src/core/contracts/base/PoolState.sol

26:  uint128 public override liquidity;

```

instance #137
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolState.sol#L27
```solidity
src/core/contracts/base/PoolState.sol

27:  uint128 internal volumePerLiquidityInBlock;

```

instance #138
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolState.sol#L30
```solidity
src/core/contracts/base/PoolState.sol

30:  uint32 public override liquidityCooldown;

```

instance #139
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolState.sol#L35
```solidity
src/core/contracts/base/PoolState.sol

35:  mapping(int24 => TickManager.Tick) public override ticks;

```

instance #140
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolState.sol#L37
```solidity
src/core/contracts/base/PoolState.sol

37:  mapping(int16 => uint256) public override tickTable;

```

 *** 


### G-08: Using `!= 0` costs less gas than `> 0` in a require() statement
Saves 6 gas per instance for solidity version less than 0.8.12.

Total instances of this issue: 11

instance #1
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L434
```solidity
src/core/contracts/AlgebraPool.sol

434:    require(liquidityDesired > 0, 'IL');

```

instance #2
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L454
```solidity
src/core/contracts/AlgebraPool.sol

454:      if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');

```

instance #3
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L455
```solidity
src/core/contracts/AlgebraPool.sol

455:      if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');

```

instance #4
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L469
```solidity
src/core/contracts/AlgebraPool.sol

469:    require(liquidityActual > 0, 'IIL2');

```

instance #5
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L641
```solidity
src/core/contracts/AlgebraPool.sol

641:      require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');

```

instance #6
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L645
```solidity
src/core/contracts/AlgebraPool.sol

645:      require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');

```

instance #7
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L898
```solidity
src/core/contracts/AlgebraPool.sol

898:    require(_liquidity > 0, 'L');

```

instance #8
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L52
```solidity
src/core/contracts/libraries/PriceMovementMath.sol

52:    require(price > 0);

```

instance #9
Link: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L53
```solidity
src/core/contracts/libraries/PriceMovementMath.sol

53:    require(liquidity > 0);

```

