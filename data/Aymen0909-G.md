# Gas Optimizations

## Findings

|               | Issue         | Instances     |
| :-------------: |:-------------|:-------------:|
| 1      | State variables only set in the constructor should be declared `immutable`     | 1 |
| 2      | Variables inside struct should be packed to save gas     | 1    |
| 3      | Usage of `uints/ints` smaller than 32 bytes (256 bits) incurs overhead  |  3 |
| 4      | `x += y/x -= y` costs more gas than `x = x + y/x = x - y` for state variables  |  4 |
| 5      | Splitting `require()` statements that uses `&&` saves gas  |  6 |
| 6      | It costs more gas to initialize variables to zero than to let the default of zero be applied    |  1  |
| 7      | Use of `++i` cost less gas than `i++` in for loops    |  1  |
| 8      | `++i/i++` should be `unchecked{++i}/unchecked{i++}` when it is not possible for them to overflow, as in the case when used in for & while loops |  1  |
| 9      | Use more recent solidity versions |   |
| 10      | Functions guaranteed to revert when called by normal users can be marked `payable` |  12 |

## Findings

### 1- State variables only set in the constructor should be declared `immutable` :

Avoids a Gsset (20000 gas) in the constructor, and replaces each Gwarmacces (100 gas) with a PUSH32 (3 gas).

There is 1 instance of this issue:

File: src/core/contracts/AlgebraPoolDeployer.sol [Line 19](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L19)
```
address private owner;
```

Since there no way of changing the `AlgebraPoolDeployer` contract owner after deployment, it's better to declare the owner address as immutable.

### 2- Variables inside `struct` should be packed to save gas :

As the solidity EVM works with 32 bytes, variables less than 32 bytes should be packed inside a struct so that they can be stored in the same slot, each slot saved can avoid an extra Gsset (20000 gas) for the first setting of the struct. Subsequent reads as well as writes have smaller gas savings.

There is 1 instance of this issue:

File: src/core/contracts/AlgebraPool.sol [Line 675-690](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L675-L690)

```
struct SwapCalculationCache {
    uint256 communityFee; // The community fee of the selling token, uint256 to minimize casts
    uint128 volumePerLiquidityInBlock;
    int56 tickCumulative; // The global tickCumulative at the moment
    uint160 secondsPerLiquidityCumulative; // The global secondPerLiquidity at the moment
    bool computedLatestTimepoint; //  if we have already fetched _tickCumulative_ and _secondPerLiquidity_ from the DataOperator
    int256 amountRequiredInitial; // The initial value of the exact input\output amount
    int256 amountCalculated; // The additive amount of total output\input calculated trough the swap
    uint256 totalFeeGrowth; // The initial totalFeeGrowth + the fee growth during a swap
    uint256 totalFeeGrowthB;
    IAlgebraVirtualPool.Status incentiveStatus; // If there is an active incentive at the moment
    bool exactInput; // Whether the exact input or output is specified
    uint16 fee; // The current dynamic fee
    int24 startTick; // The tick at the start of a swap
    uint16 timepointIndex; // The index of last written timepoint
    }
```

This should be rearranged as follow to save gas :

```
struct SwapCalculationCache {
    uint256 communityFee; // The community fee of the selling token, uint256 to minimize casts
    uint128 volumePerLiquidityInBlock;
    int56 tickCumulative; // The global tickCumulative at the moment
    uint160 secondsPerLiquidityCumulative; // The global secondPerLiquidity at the moment
    uint16 fee; // The current dynamic fee
    int24 startTick; // The tick at the start of a swap
    uint16 timepointIndex; // The index of last written timepoint
    bool computedLatestTimepoint; //  if we have already fetched _tickCumulative_ and _secondPerLiquidity_ from the DataOperator
    int256 amountRequiredInitial; // The initial value of the exact input\output amount
    int256 amountCalculated; // The additive amount of total output\input calculated trough the swap
    uint256 totalFeeGrowth; // The initial totalFeeGrowth + the fee growth during a swap
    uint256 totalFeeGrowthB;
    IAlgebraVirtualPool.Status incentiveStatus; // If there is an active incentive at the moment
    bool exactInput; // Whether the exact input or output is specified
    }
```

In this case the `fee`, `startTick`, `timepointIndex` variables are saved in the same slot as `secondsPerLiquidityCumulative`.

### 3- Usage of `uints/ints` smaller than 32 bytes (256 bits) incurs overhead :

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size as you can check [here](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html).

So use `uint256`/`int256` for state variables and then downcast to lower sizes where needed.

There are 3 instances of this issue:

File: src/core/contracts/base/PoolState.sol [Line 26](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolState.sol#L26)
```
uint128 public override liquidity;
```

File: src/core/contracts/base/PoolState.sol [Line 27](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolState.sol#L27)
```
uint128 internal volumePerLiquidityInBlock;
```

File: src/core/contracts/base/PoolState.sol [Line 30](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolState.sol#L30)
```
uint32 public override liquidityCooldown;
```

### 4- `x += y/x -= y` costs more gas than `x = x + y/x = x - y` for state variables :

There are 4 instances of this issue:

File: src/core/contracts/AlgebraPool.sol [Line 257](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L257)
```
_position.fees0 += fees0;
```

File: src/core/contracts/AlgebraPool.sol [Line 258](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L258)
```
_position.fees1 += fees1;
```

File: src/core/contracts/AlgebraPool.sol [Line 931](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L931)
```
totalFeeGrowth0Token += FullMath.mulDiv(paid0 - fees0, Constants.Q128, _liquidity);
```

File: src/core/contracts/AlgebraPool.sol [Line 945](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L945)
```
totalFeeGrowth1Token += FullMath.mulDiv(paid1 - fees1, Constants.Q128, _liquidity);
```

### 5- Splitting `require()` statements that uses `&&` saves gas (saves 8 gas per &&) :

There are 6 instances of this issue :

```
File: src/core/contracts/AlgebraFactory.sol

110      require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');

File: src/core/contracts/AlgebraPool.sol

739      require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
743      require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
953      require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));
968      require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);

File: src/core/contracts/DataStorageOperator.sol

46       require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0'); 
```

### 6- It costs more gas to initialize variables to zero than to let the default of zero be applied (saves ~3 gas per instance) :

If a variable is not set/initialized, it is assumed to have the default value (0 for uint or int, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

There is 1 instance of this issue:

File: src/core/contracts/libraries/DataStorage.sol [Line 307](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307)
```
for (uint256 i = 0; i < secondsAgos.length; i++)
```    

### 7- Use of `++i` cost less gas than `i++/i=i+1` in for loops :

There is 1 instance of this issue:

File: src/core/contracts/libraries/DataStorage.sol [Line 307](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307)
```
for (uint256 i = 0; i < secondsAgos.length; i++)
```    

### 8- `++i/i++` should be `unchecked{++i}/unchecked{i++}` when it is not possible for them to overflow, as in the case when used in for & while loops :

There is 1 instance of this issue:

File: src/core/contracts/libraries/DataStorage.sol [Line 307](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307)
```
for (uint256 i = 0; i < secondsAgos.length; i++)
```    

### 9- Use more recent solidity versions :

All contracts contained in this project use an old solidity version `pragma solidity =0.7.6`, consider use a solidity version of at least 0.8.4 to get access to Custom errors which are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met) while providing the same amount of information.

### 10- Functions guaranteed to revert when called by normal users can be marked `payable` :

If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for the owner because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are : 

CALLVALUE(gas=2), DUP1(gas=3), ISZERO(gas=3), PUSH2(gas=3), JUMPI(gas=10), PUSH1(gas=3), DUP1(gas=3), REVERT(gas=0), JUMPDEST(gas=1), POP(gas=2). 
Which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

There are 12 instances of this issue:

```
File: src/core/contracts/AlgebraFactory.sol

77       function setOwner(address _owner) external override onlyOwner
84       function setFarmingAddress(address _farmingAddress) external override onlyOwner
91       function setVaultAddress(address _vaultAddress) external override onlyOwner
98       function setBaseFeeConfiguration(uint16 alpha1, uint16 alpha2, uint32 beta1, uint32 beta2, uint16 gamma1, uint16 gamma2, uint32 volumeBeta, uint16 volumeGamma, uint16 baseFee) external override onlyOwner

File: src/core/contracts/AlgebraPoolDeployer.sol

36       function setFactory(address _factory) external override onlyOwner
44       function deploy(address dataStorage, address _factory, address token0, address token1) external override onlyFactory

File: src/core/contracts/DataStorageOperator.sol

37        function initialize(uint32 time, int24 tick) external override onlyPool
53        function getSingleTimepoint(uint32 time, uint32 secondsAgo, int24 tick, uint16 index, uint128 liquidity) external view override onlyPool
88        function getTimepoints(uint32 time, uint32[] memory secondsAgos, int24 tick, uint16 index, uint128 liquidity) external view override onlyPool
110       function getAverages(uint32 time, int24 tick, uint16 index, uint128 liquidity) external view override onlyPool
120       function write(uint16 index, uint32 blockTimestamp, int24 tick, uint128 liquidity, uint128 volumePerLiquidity) external override onlyPool
150       function getFee(uint32 _time, int24 _tick, uint16 _index, uint128 _liquidity) external view override onlyPool
```