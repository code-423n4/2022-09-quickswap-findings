 # 1. [G-1]  ++number (--number)  cost less gas compare number++ (number--) or number += 1 (number -= 1)

1.1. File DataStorage.sol, line 119:

    index -= 1;

Becomes:

     --index;

1.2. File DataStorage.sol, line 307:

     for (uint256 i = 0; i < secondsAgos.length; i++) {

Becomes:

     for (uint256 i = 0; i < secondsAgos.length; ++i) {

1.3. File TickTable.sol, line 100:

     tick += 1;

Becomes:

     ++tick;

# 2. [G-2] Initialize variable without assign equal 0  is cheaper than initialize with assign equal 0

File DataStorage.sol, line 307:

     for (uint256 i = 0; i < secondsAgos.length; i++) {

Becomes:

     for (uint256 i; i < secondsAgos.length; ++i) {

# 3. [G-3] Cache the length of arrays in the loops for saving gas

File DataStorage.sol, line 307:

     for (uint256 i = 0; i < secondsAgos.length; i++) {

Becomes:

     uint256 length = secondsAgos.length;
     for (uint256 i; i < length; ++i) {

# 4. [G-4] Cache read variables in memory to save gas

 4.1. File AlgebraPool.sol, line 722 - 728:

    currentPrice = globalState.price;
    currentTick = globalState.tick;
    cache.fee = globalState.fee;
    cache.timepointIndex = globalState.timepointIndex;
    uint256 _communityFeeToken0 = globalState.communityFeeToken0;
    uint256 _communityFeeToken1 = globalState.communityFeeToken1;
    bool unlocked = globalState.unlocked;

Becomes:

    GlobalState memory globalStateCache = globalState;
    currentPrice = globalStateCache.price;
    currentTick = globalStateCache.tick;
    cache.fee = globalStateCache.fee;
    cache.timepointIndex = globalStateCache.timepointIndex;
    uint256 _communityFeeToken0 = globalStateCache.communityFeeToken0;
    uint256 _communityFeeToken1 = globalStateCache.communityFeeToken1;
    bool unlocked = globalStateCache.unlocked;

 4.2. File AlgebraPool.sol, line 116:

    TickManager.Tick storage _lower = ticks[bottomTick];

Becomes:

    TickManager.Tick memory _lower = ticks[bottomTick];

##### Instances include:

 File AlgebraPool.sol, line 127.
 File DataStorage.sol line 120, 228, 392, 411.
 File TickManager.sol, line 47, 48.
