## FINDINGS
NB: *Some functions have been truncated where neccessary to just show affected parts of the code*
Throught the report some places might be denoted with audit tags to show the actual place affected.


### Using immutable on variables that are only set in the constructor and never after 
Use immutable if you want to assign a permanent value at construction. Use constants if you already know the permanent value. Both get directly embedded in bytecode, saving SLOAD.
Variables only set in the constructor and never edited afterwards should be marked as immutable, as it would avoid the expensive storage-writing operation in the constructor (around 20 000 gas per variable) and replace the expensive storage-reading operations (around 2100 gas per reading) to a less expensive value reading (3 gas)

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L19
```solidity
File: /src/core/contracts/AlgebraPoolDeployer.sol
19:  address private owner;
```

### Massive 15k per tx gas savings - use 1 and 2 for Reentrancy guard

[See solmate implementation](https://github.com/transmissions11/solmate/blob/main/src/utils/ReentrancyGuard.sol)

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolState.sol#L39-L45
```solidity
File: /src/core/contracts/base/PoolState.sol
39:  /// @dev Reentrancy protection. Implemented in every function of the contract since there are checks of balances.
40:  modifier lock() {
41:    require(globalState.unlocked, 'LOK');
42:    globalState.unlocked = false;
43:    _;
44:    globalState.unlocked = true;
45:  }
```

## The result of a function call should be cached rather than re-calling the function

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L215-L260
### AlgebraPool.sol.\_recalculatePosition():  The results of \_blockTimestamp() should be cached
```solidity
File: /src/core/contracts/AlgebraPool.sol
215:  function _recalculatePosition(
            ...
228:        if (_liquidityCooldown > 0) {
229:          require((_blockTimestamp() - lastLiquidityAddTimestamp) >= _liquidityCooldown);
230:        }
           ....
235:      (_position.liquidity, _position.lastLiquidityAddTimestamp) = (
236:        liquidityNext,
237:        liquidityNext > 0 ? (liquidityDelta > 0 ? _blockTimestamp() : lastLiquidityAddTimestamp) : 0
238:      );
239:    }

260:  }
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L274-L364
### AlgebraPool.sol.\_updatePositionTicksAndFees():  The results of \_blockTimestamp() should be cached
```solidity
File: /src/core/contracts/AlgebraPool.sol
274:  function _updatePositionTicksAndFees(

355:        uint16 newTimepointIndex = _writeTimepoint(cache.timepointIndex, _blockTimestamp(), cache.tick, liquidityBefore, volumePerLiquidityInBlock); //@audit: _blockTimestamp() first access
356:        if (cache.timepointIndex != newTimepointIndex) {
357:          globalState.fee = _getNewFee(_blockTimestamp(), cache.tick, newTimepointIndex, liquidityBefore); //@audit: _blockTimestamp() second access

364:  }
```

### Avoid contract existence checks by using solidity version 0.8.10 or later(only if < 0.8.10 --remove me)

Prior to 0.8.10 the compiler inserted extra code, including EXTCODESIZE (100 gas), to check for contract existence for external calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L55
```solidity
File: /src/core/contracts/AlgebraPool.sol
55:    require(msg.sender == IAlgebraFactory(factory).owner());

71:    return IERC20Minimal(token0).balanceOf(address(this));

75:    return IERC20Minimal(token1).balanceOf(address(this));

93:    return IDataStorageOperator(dataStorageOperator).timepoints(index);

183:      IDataStorageOperator(dataStorageOperator).getTimepoints(
184:        _blockTimestamp(),
185:        secondsAgos,
186:        globalState.tick,
187:        globalState.timepointIndex,
188:        liquidity
189:      );

334:    (uint256 feeGrowthInside0X128, uint256 feeGrowthInside1X128) = ticks.getInnerFeeGrowth(

542:    newFee = IDataStorageOperator(dataStorageOperator).getFee(_time, _tick, _index, _liquidity);

547:    address vault = IAlgebraFactory(factory).vaultAddress();

784:      (step.nextTick, step.initialized) = tickTable.nextTickInTheSameRow(currentTick, zeroToOne);

918:    address vault = IAlgebraFactory(factory).vaultAddress();

960:    require(msg.sender == IAlgebraFactory(factory).farmingAddress());
```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L69
```solidity
File: /src/core/contracts/AlgebraFactory.sol
69:    pool = IAlgebraPoolDeployer(poolDeployer).deploy(address(dataStorage), address(this), token0, token1);
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L43
```solidity
File: /src/core/contracts/DataStorageOperator.sol
43:    require(msg.sender == factory || msg.sender == IAlgebraFactory(factory).owner());
```

### Tighly pack storage variables/optimize the order of variable declaration

Here, the storage variables can be tightly packed from:

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/Constants.sol#L5-L17
### Save 1 Storage SLOT  here
```solidity
File: /src/core/contracts/libraries/Constants.sol
5:  uint8 internal constant RESOLUTION = 96;
6:  uint256 internal constant Q96 = 0x1000000000000000000000000;
7:  uint256 internal constant Q128 = 0x100000000000000000000000000000000;
8:  // fee value in hundredths of a bip, i.e. 1e-6
9:  uint16 internal constant BASE_FEE = 100;
10:  int24 internal constant TICK_SPACING = 60;


12:  // max(uint128) / ( (MAX_TICK - MIN_TICK) / TICK_SPACING )
13:  uint128 internal constant MAX_LIQUIDITY_PER_TICK = 11505743598341114571880798222544994;


15:  uint32 internal constant MAX_LIQUIDITY_COOLDOWN = 1 days;
16:  uint8 internal constant MAX_COMMUNITY_FEE = 250;
17:  uint256 internal constant COMMUNITY_FEE_DENOMINATOR = 1000;
```


```diff
 library Constants {
-  uint8 internal constant RESOLUTION = 96;
   uint256 internal constant Q96 = 0x1000000000000000000000000;
   uint256 internal constant Q128 = 0x100000000000000000000000000000000;
+    uint8 internal constant RESOLUTION = 96;
   // fee value in hundredths of a bip, i.e. 1e-6
   uint16 internal constant BASE_FEE = 100;
   int24 internal constant TICK_SPACING = 60;  

  // max(uint128) / ( (MAX_TICK - MIN_TICK) / TICK_SPACING )
  uint128 internal constant MAX_LIQUIDITY_PER_TICK = 11505743598341114571880798222544994;

  uint32 internal constant MAX_LIQUIDITY_COOLDOWN = 1 days;
  uint8 internal constant MAX_COMMUNITY_FEE = 250;
  uint256 internal constant COMMUNITY_FEE_DENOMINATOR = 1000;
```

### Pack structs by putting data types that can fit in one slot together

As the solidity EVM works with 32 bytes, variables less than 32 bytes should be packed inside a struct so that they can be stored in the same slot, this saves gas when writing to storage ~20000 gas

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L675-L690
### We can save 1 SLOT in the following
```solidity
File: /src/core/contracts/AlgebraPool.sol
675:  struct SwapCalculationCache {
676:    uint256 communityFee; // The community fee of the selling token, uint256 to minimize casts
677:    uint128 volumePerLiquidityInBlock;
678:    int56 tickCumulative; // The global tickCumulative at the moment
679:    uint160 secondsPerLiquidityCumulative; // The global secondPerLiquidity at the moment
680:    bool computedLatestTimepoint; //  if we have already fetched _tickCumulative_ and _secondPerLiquidity_ from the DataOperator
681:    int256 amountRequiredInitial; // The initial value of the exact input\output amount
682:    int256 amountCalculated; // The additive amount of total output\input calculated trough the swap
683:    uint256 totalFeeGrowth; // The initial totalFeeGrowth + the fee growth during a swap
684:    uint256 totalFeeGrowthB;
685:    IAlgebraVirtualPool.Status incentiveStatus; // If there is an active incentive at the moment
686:    bool exactInput; // Whether the exact input or output is specified
687:    uint16 fee; // The current dynamic fee
688:    int24 startTick; // The tick at the start of a swap
689:    uint16 timepointIndex; // The index of last written timepoint
690:  }
```


```diff
   struct SwapCalculationCache {
-    uint256 communityFee; // The community fee of the selling token, uint256 to minimize casts
-    uint128 volumePerLiquidityInBlock;
-    int56 tickCumulative; // The global tickCumulative at the moment
-    uint160 secondsPerLiquidityCumulative; // The global secondPerLiquidity at the moment
-    bool computedLatestTimepoint; //  if we have already fetched _tickCumulative_ and _secondPerLiquidity_ from the DataOperator
-    int256 amountRequiredInitial; // The initial value of the exact input\output amount
-    int256 amountCalculated; // The additive amount of total output\input calculated trough the swap
-    uint256 totalFeeGrowth; // The initial totalFeeGrowth + the fee growth during a swap
+    uint256 communityFee;
+    int56 tickCumulative;
+    int24 startTick;
+    bool exactInput;
+    bool computedLatestTimepoint;
+    uint160 secondsPerLiquidityCumulative;
+    int256 amountRequiredInitial;
+    int256 amountCalculated;
+    uint256 totalFeeGrowth;
     uint256 totalFeeGrowthB;
-    IAlgebraVirtualPool.Status incentiveStatus; // If there is an active incentive at the moment
-    bool exactInput; // Whether the exact input or output is specified
-    uint16 fee; // The current dynamic fee
-    int24 startTick; // The tick at the start of a swap
-    uint16 timepointIndex; // The index of last written timepoint
+    IAlgebraVirtualPool.Status incentiveStatus;
+    uint128 volumePerLiquidityInBlock;
+    uint16 fee;
+    uint16 timepointIndex;
   }
```

### Cache storage values in memory to minimize SLOADs
The code can be optimized by minimizing the number of SLOADs.

SLOADs are expensive (100 gas after the 1st one) compared to MLOADs/MSTOREs (3 gas each). Storage values read multiple times should instead be cached in memory the first time (costing 1 SLOAD) and then read from this cache to avoid multiple SLOADs.

NB: *Some functions have been truncated where neccessary to just show affected parts of the code*

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L137
### AlgebraPool.sol.getInnerCumulatives(): globalState should be cached(Saves 1 SLOAD)
```solidity
FIle: /src/core/contracts/AlgebraPool.sol
137:    (int24 currentTick, uint16 currentTimepointIndex) = (globalState.tick, globalState.timepointIndex);
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L752-L761
### AlgebraPool.sol.\_calculateSwapAndLock(): activeIncentive should be cached(Saves 1 SLOAD)
```solidity
File: /src/core/contracts/AlgebraPool.sol
752:      if (activeIncentive != address(0)) {
753:        IAlgebraVirtualPool.Status _status = IAlgebraVirtualPool(activeIncentive).increaseCumulative(blockTimestamp);
754:        if (_status == IAlgebraVirtualPool.Status.NOT_EXIST) {

```

## Internal/Private functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

Affected code:

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L122-L124
```solidity
File: /src/core/contracts/AlgebraFactory.sol
122:  function computeAddress(address token0, address token1) internal view returns (address pool) {
123:    pool = address(uint256(keccak256(abi.encodePacked(hex'ff', poolDeployer, keccak256(abi.encode(token0, token1)), POOL_INIT_CODE_HASH))));
124:  }
```

The above function is only called once on [Line 65](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L65)

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L215-L220
```solidity
File: /src/core/contracts/AlgebraPool.sol
215:  function _recalculatePosition(
216:    Position storage _position,
217:    int128 liquidityDelta,
218:    uint256 innerFeeGrowth0Token,
219:    uint256 innerFeeGrowth1Token
220:  ) internal {
```
The above function is only called once on [Line 342](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L342)

### Multiple accesses of a mapping/array should use a local variable cache

Caching a mapping's value in a local storage or calldata variable when the value is accessed multiple times saves ~42 gas per access due to not having to perform the same offset calculation every time.
### Help the Optimizer by saving a storage variable's reference instead of repeatedly fetching it

To help the optimizer,declare a storage type variable and use it instead of repeatedly fetching the reference in a map or an array. 
As an example, instead of repeatedly calling ```someMap[someIndex]```, save its reference like this: ```SomeStruct storage someStruct = someMap[someIndex]``` and use it.

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L59-L74
### AlgebraFactory.sol.createPool():poolByPair[token0][token1]  should be cached 
```solidity
File: /src/core/contracts/AlgebraFactory.sol
59:  function createPool(address tokenA, address tokenB) external override returns (address pool) {

63:    require(poolByPair[token0][token1] == address(0));  //@audit: poolByPair[token0][token1]

71:    poolByPair[token0][token1] = pool;  //@audit: poolByPair[token0][token1]
72:    poolByPair[token1][token0] = pool;
73:    emit Pool(token0, token1, pool);
74:  }
```


https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L105-L135
### DataStorage.sol.\_getAverageTick(): self[oldestIndex] should be cached
```solidity
File: /src/core/contracts/libraries/DataStorage.sol
105:  function _getAverageTick(

114:    uint32 oldestTimestamp = self[oldestIndex].blockTimestamp; //@audit: self[oldestIndex]
115:    int56 oldestTickCumulative = self[oldestIndex].tickCumulative; //@audit: self[oldestIndex]
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L217-L218
### DataStorage.sol.\getSingleTimepoint(): self[index] should be cached
```solidity
File: /src/core/contracts/libraries/DataStorage.sol

217:    if (secondsAgo == 0 || lteConsideringOverflow(self[index].blockTimestamp, target, time)) { //@audit: self[index]
218:      Timepoint memory last = self[index]; //@audit: self[index]
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L326-L339
### DataStorage.sol.getAverages(): self[nextIndex] should be cached
```solidity
File: /src/core/contracts/libraries/DataStorage.sol
326:  function getAverages(

336:    if (self[nextIndex].initialized) { //@audit: self[nextIndex]
337:      oldest = self[nextIndex]; //@audit: self[nextIndex]
338:      oldestIndex = nextIndex;
339:    }
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L364-L373
### DataStorage.sol.initialize(): self[0] should be cached
```solidity
File: /src/core/contracts/libraries/DataStorage.sol
  function initialize(
    Timepoint[UINT16_MODULO] storage self,
    uint32 time,
    int24 tick
  ) internal {
    require(!self[0].initialized);
    self[0].initialized = true; //@audit: self[0]
    self[0].blockTimestamp = time; //@audit: self[0]
    self[0].averageTick = tick; //@audit: self[0]
  }
```


### Use Shift Right/Left instead of Division/Multiplication
A division/multiplication by any number x being a power of 2 can be calculated by shifting log2(x) to the right/left.

While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

[relevant source](https://github.com/byterocket/c4-common-issues/blob/main/0-Gas-Optimizations.md/#g008---use-shift-rightleft-instead-of-divisionmultiplication-if-possible)

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L88
```solidity
File: /src/core/contracts/libraries/AdaptiveFee.sol
88:    res += (xLowestDegree * gHighestDegree) / 2;
```

```solidity
88:     res += (xLowestDegree * gHighestDegree) >> 2;

```

### x += y costs more gas than x = x + y for state variables

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L257-L258
```solidity
File: /src/core/contracts/AlgebraPool.sol
257:      _position.fees0 += fees0;
258:      _position.fees1 += fees1;
```

### Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

   When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html Use a larger size then downcast where needed

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L366-L372
```solidity
File: /src/core/contracts/AlgebraPool.sol
366:  function _getAmountsForLiquidity(
367:    int24 bottomTick,
368:    int24 topTick,
369:    int128 liquidityDelta,
370:    int24 currentTick,
371:    uint160 currentPrice
372:  )
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L536-L541
```solidity
File: /src/core/contracts/AlgebraPool.sol
536:  function _getNewFee(
537:    uint32 _time,
538:    int24 _tick,
539:    uint16 _index,
540:    uint128 _liquidity
541:  ) private returns (uint16 newFee) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L551-L557
```solidity
File: /src/core/contracts/AlgebraPool.sol
551:  function _writeTimepoint(
552:    uint16 timepointIndex,
553:    uint32 blockTimestamp,
554:    int24 tick,
555:    uint128 liquidity,
556:    uint128 volumePerLiquidityInBlock
557:  ) private returns (uint16 newTimepointIndex) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L561-L567
```solidity
File: /src/core/contracts/AlgebraPool.sol
561:  function _getSingleTimepoint(
562:    uint32 blockTimestamp,
563:    uint32 secondsAgo,
564:    int24 startTick,
565:    uint16 timepointIndex,
566:    uint128 liquidityStart
567:  )
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L703-L707
```solidity
File: /src/core/contracts/AlgebraPool.sol
703:  function _calculateSwapAndLock(
704:    bool zeroToOne,
705:    int256 amountRequired,
706:    uint160 limitSqrtPrice
707:  )
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TokenDeltaMath.sol#L23-L28
```solidity
File: /src/core/contracts/libraries/TokenDeltaMath.sol
23:  function getToken0Delta(
24:    uint160 priceLower,
25:    uint160 priceUpper,
26:    uint128 liquidity,
27:    bool roundUp
28:  ) internal pure returns (uint256 token0Delta) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TokenDeltaMath.sol#L45-L50
```solidity
File: /src/core/contracts/libraries/TokenDeltaMath.sol
45:  function getToken1Delta(
46:    uint160 priceLower,
47:    uint160 priceUpper,
48:    uint128 liquidity,
49:    bool roundUp
50:  ) internal pure returns (uint256 token1Delta) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TokenDeltaMath.sol#L61-L65
```solidity
File: /src/core/contracts/libraries/TokenDeltaMath.sol
61:  function getToken0Delta(
62:    uint160 priceLower,
63:    uint160 priceUpper,
64:    int128 liquidity
65:  ) internal pure returns (int256 token0Delta) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TokenDeltaMath.sol#L76-L80
```solidity
File: /src/core/contracts/libraries/TokenDeltaMath.sol
76:  function getToken1Delta(
77:    uint160 priceLower,
78:    uint160 priceUpper,
79:    int128 liquidity
80:  ) internal pure returns (int256 token1Delta) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickManager.sol#L78-L89
```solidity
File: /src/core/contracts/libraries/TickManager.sol
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

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickManager.sol#L129-L137
```solidity
File: /src/core/contracts/libraries/TickManager.sol
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

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L20-L25
```solidity
File: /src/core/contracts/libraries/PriceMovementMath.sol
20:  function getNewPriceAfterInput(
21:    uint160 price,
22:    uint128 liquidity,
23:    uint256 input,
24:    bool zeroToOne
25:  ) internal pure returns (uint160 resultPrice) {
```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L36-L41
```solidity
File: /src/core/contracts/libraries/PriceMovementMath.sol
36:  function getNewPriceAfterOutput(
37:    uint160 price,
38:    uint128 liquidity,
39:    uint256 output,
40:    bool zeroToOne
41:  ) internal pure returns (uint160 resultPrice) {
```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L45-L51
```solidity
File: /src/core/contracts/libraries/PriceMovementMath.sol
45:  function getNewPrice(
46:    uint160 price,
47:    uint128 liquidity,
48:    uint256 amount,
49:    bool zeroToOne,
50:    bool fromInput
51:  ) internal pure returns (uint160 resultPrice) {
```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L93-L97
```solidity
File: /src/core/contracts/libraries/PriceMovementMath.sol
93:  function getTokenADelta01(
94:    uint160 to,
95:    uint160 from,
96:    uint128 liquidity
97:  ) internal pure returns (uint256) {
```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L101-L105
```solidity
File: /src/core/contracts/libraries/PriceMovementMath.sol
101:  function getTokenADelta10(
102:    uint160 to,
103:    uint160 from,
104:    uint128 liquidity
105:  ) internal pure returns (uint256) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L109-L113
```solidity
File: /src/core/contracts/libraries/PriceMovementMath.sol
109:  function getTokenBDelta01(
110:    uint160 to,
111:    uint160 from,
112:    uint128 liquidity
113:  ) internal pure returns (uint256) {
```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L117-L121
```solidity
File: /src/core/contracts/libraries/PriceMovementMath.sol
117:  function getTokenBDelta10(
118:    uint160 to,
119:    uint160 from,
120:    uint128 liquidity
121:  ) internal pure returns (uint256) {
```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L136-L143
```solidity
File: /src/core/contracts/libraries/PriceMovementMath.sol
136:  function movePriceTowardsTarget(
137:    bool zeroToOne,
138:    uint160 currentPrice,
139:    uint160 targetPrice,
140:    uint128 liquidity,
141:    int256 amountAvailable,
142:    uint16 fee
143:  )
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L94-L98
```solidity
File: /src/core/contracts/libraries/DataStorage.sol
94:  function lteConsideringOverflow(
95:    uint32 a,
96:    uint32 b,
97:    uint32 currentTime
98:  ) private pure returns (bool res) {
```

### No need to initialize variables with their default values
If a variable is not set/initialized, it is assumed to have the default value (0, false, 0x0 etc depending on the data type). If you explicitly initialize it with its default value, you are just wasting gas.
It costs more gas to initialize variables to zero than to let the default of zero be applied
Declaring uint256 i = 0; means doing an MSTORE of the value 0 Instead you could just declare uint256 i to declare the variable without assigning it’s default value, saving 3 gas per declaration

 https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L307
 ```solidity
File: /src/core/contracts/libraries/DataStorage.sol
307:    for (uint256 i = 0; i < secondsAgos.length; i++) {
 ```

### Cache the length of arrays in loops (saves ~6 gas per iteration)
Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack.

The solidity compiler will always read the length of the array during each iteration. That is,

   1.if it is a storage array, this is an extra sload operation (100 additional extra gas (EIP-2929 2) for each iteration except for the first),
   2.if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first),
   3.if it is a calldata array, this is an extra calldataload operation (3 additional gas for each iteration except for the first)

This extra costs can be avoided by caching the array length (in stack):
 When reading the length of an array,  **sload** or **mload** or **calldataload** operation is only called once and subsequently replaced by a cheap **dupN** instruction. Even though mload , calldataload and dupN have the same gas cost, mload and calldataload needs an additional dupN to put the offset in the stack, i.e., an extra 3 gas. which brings this to 6 gas
 
Here, I suggest storing the array’s length in a variable before the for-loop, and use it instead:


 https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L307
 ```solidity
File: /src/core/contracts/libraries/DataStorage.sol
307:    for (uint256 i = 0; i < secondsAgos.length; i++) {
 ```
 
### ++i costs less gas compared to i++ or i += 1  (~5 gas per iteration)

++i costs less gas compared to i++ or i += 1 for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration). This statement is true even with the optimizer enabled.

i++ increments i and returns the initial value of i. Which means:

```solidity
uint i = 1;  
i++; // == 1 but i == 2  
```

But ++i returns the actual incremented value:

```solidity
uint i = 1;  
++i; // == 2 and i == 2 too, so no need for a temporary variable  
```

In the first case, the compiler has to create a temporary variable (when used) for returning 1 instead of 2

Instances include:

 https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L307
 ```solidity
File: /src/core/contracts/libraries/DataStorage.sol
307:    for (uint256 i = 0; i < secondsAgos.length; i++) {
 ```

### Splitting require() statements that use && saves gas - (saves 8 gas per &&)

Instead of using the && operator in a single require statement to check multiple conditions,using multiple require statements with 1 condition per require statement will save 8 GAS per &&
The gas difference would only be realized if the revert condition is realized(met).

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L110
```solidity
File: /src/core/contracts/AlgebraFactory.sol
110:    require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L46
```solidity
File: /src/core/contracts/DataStorageOperator.sol
46:    require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L739
```solidity
File: /src/core/contracts/AlgebraPool.sol
739:    require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');

743:    require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');

953:    require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));

968:    require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);

```
**Proof**
**The following tests were carried out in remix with both optimization turned on and off**

```solidity
function multiple (uint a) public pure returns (uint){
	require ( a > 1 && a < 5, "Initialized");
	return  a + 2;
}
```
**Execution cost**
21617 with optimization and using &&
21976 without optimization and using &&


After splitting the require statement
```solidity
function multiple(uint a) public pure returns (uint){
	require (a > 1 ,"Initialized");
	require (a < 5 , "Initialized");
	return a + 2;
}
```
**Execution cost**
21609 with optimization and split require
21968 without optimization and using split require


### Pre-Solidity 0.8.13: > 0 is less efficient than != 0 for unsigned integers(6 gas)

Up until Solidity 0.8.13: != 0 costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled (6 gas)
!= 0 costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled (6 gas)

For uints the minimum value would be 0 and never a negative value. Since it cannot be a negative value, then the check > 0 is essentially checking that the value is not equal to 0 therefore >0 can be replaced with !=0 which saves gas.

Proof: While it may seem that > 0 is cheaper than !=, this is only true without the optimizer enabled and outside a require statement. If you enable the optimizer at 10k AND you're in a require statement, this will save gas. You can see this tweet for more proofs: https://twitter.com/gzeon/status/1485428085885640706

I suggest changing > 0 with != 0 here:

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L224
```solidity
File: /src/core/contracts/AlgebraPool.sol
224:      require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges

434:    require(liquidityDesired > 0, 'IL');

469:    require(liquidityActual > 0, 'IIL2');
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L52-L53
```solidity
File: /src/core/contracts/libraries/PriceMovementMath.sol
52:    require(price > 0);
53:    require(liquidity > 0);
```

## A modifier used only once and not being inherited should be inlined to save gas

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L21-L24
```solidity
File: /src/core/contracts/AlgebraPoolDeployer.sol
21:  modifier onlyFactory() {
22:    require(msg.sender == factory);
23:    _;
24:   }
```

The above modifier is only used once on [Line 49](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L49)

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L26-L29
```solidity
File: /src/core/contracts/AlgebraPoolDeployer.sol
26:  modifier onlyOwner() {
27:    require(msg.sender == owner);
28:    _;
29:  }
```
### Using private rather than public for constants, saves gas
If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table.

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L12
```solidity
File: /src/core/contracts/libraries/DataStorage.sol
12:  uint32 public constant WINDOW = 1 days;
```

### Use a more recent version of solidity

Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L2
```solidity
File:  /src/core/contracts/AlgebraFactory.sol
2:  pragma solidity =0.7.6;
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L2
```solidity
File: /src/core/contracts/AlgebraPool.sol
2: pragma solidity =0.7.6;
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L2
```solidity
File: /src/core/contracts/AlgebraPoolDeployer.sol
2: pragma solidity =0.7.6;
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L2
```solidity
File: /src/core/contracts/DataStorageOperator.sol
2: pragma solidity =0.7.6;
```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolImmutables.sol#L2
```solidity
File: /src/core/contracts/base/PoolImmutables.sol
2:pragma solidity =0.7.6;
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolState.sol#L2
```solidity
File: /src/core/contracts/base/PoolState.sol
2: pragma solidity =0.7.6;
```
