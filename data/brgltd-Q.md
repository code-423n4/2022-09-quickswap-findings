# [01] Critical changes should use two-step procedure

Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.

```
function setOwner(address _owner) external override onlyOwner {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L77

```
function setFarmingAddress(address _farmingAddress) external override onlyOwner {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L84

```
function setVaultAddress(address _vaultAddress) external override onlyOwner {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L91


```
function setCommunityFee(uint8 communityFee0, uint8 communityFee1) external override lock onlyFactoryOwner {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L952

```
function setIncentive(address virtualPoolAddress) external override {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L959


```
function setLiquidityCooldown(uint32 newLiquidityCooldown) external override onlyFactoryOwner {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L967

# [02] Replace assert with require

```
assert(false);
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L190

Assert should be avoided in production code. As described on the solidity [docs](https://docs.soliditylang.org/en/v0.8.15/control-structures.html#panic-via-assert-and-error-via-require). "The assert function creates an error of type Panic(uint256). … Properly functioning code should never create a Panic, not even on invalid external input. If this happens, then there is a bug in your contract which you should fix."
Even if the code on line 190 is expected to be unreacheable, consider using a require statement instead of assert.

# [03] Missing zero address checks for initializer and setter functions

Missing checks for zero-addresses may lead to infunctional protocol, if the variable addresses are updated incorrectly.

```
constructor(address _poolDeployer, address _vaultAddress) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L50

```
function setOwner(address _owner) external override onlyOwner {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L77

```
function setFarmingAddress(address _farmingAddress) external override onlyOwner {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L84

```
function setVaultAddress(address _vaultAddress) external override onlyOwner {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L91


```
function setIncentive(address virtualPoolAddress) external override {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L959

# [04] Avoid shadowing

The arguments `liquidity` and `volumePerLiquidityInBlock` in the function `_writeTimepoint()` in the contract `AlgebraPool.sol` are showdowed by state variables with the same name from the contract `PoolState.sol`. Considering renaming these function arguments to avoid shadowing.

```
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

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L551-L559

# [05] Inconsistent use of named return variables

Some functions return named variables, others return explicit values.

Following function return explicit value

```
function balanceToken0() private view returns (uint256) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L70

Following function return named variables

```
function collect(
  address recipient,
  int24 bottomTick,
  int24 topTick,
  uint128 amount0Requested,
  uint128 amount1Requested
) external override lock returns (uint128 amount0, uint128 amount1) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L488-L494

Consider adopting a consistent approach to return values throughout the codebase by removing all named return variables, explicitly declaring them as local variables, and adding the necessary return statements where appropriate. This would improve both the explicitness and readability of the code, and it may also help reduce regressions during future code refactors.

# [06] Add a limit for the maximum number of characters per line

The solidity [documentation](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#maximum-line-length) recommends a maximum of 120 characters.

Consider adding a limit of 120 characters or less to prevent large lines. 

```
(uint128 currentLiquidity, uint32 lastLiquidityAddTimestamp) = (_position.liquidity, _position.lastLiquidityAddTimestamp);
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L221

```
(, int256 amount0Int, int256 amount1Int) = _updatePositionTicksAndFees(recipient, bottomTick, topTick, int256(liquidityActual).toInt128()); 
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L472

# [07] Order of funtions

Consider modifying the order of functions in `AlgebraPool.sol`. The solidty [documentation](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#maximum-line-length) recommends this order:

```
constructor
receive function (if exists)
fallback function (if exists)
external
public
internal
private
```

The snippet bellow shows private above external

```
function balanceToken0() private view returns (uint256) {
  return IERC20Minimal(token0).balanceOf(address(this));
}

function balanceToken1() private view returns (uint256) {
  return IERC20Minimal(token1).balanceOf(address(this));
}

/// @inheritdoc IAlgebraPoolState
function timepoints(uint256 index)
  external
  view
  override
  returns (
    bool initialized,
    uint32 blockTimestamp,
    int56 tickCumulative,
    uint160 secondsPerLiquidityCumulative,
    uint88 volatilityCumulative,
    int24 averageTick,
    uint144 volumePerLiquidityCumulative
  )
{
  return IDataStorageOperator(dataStorageOperator).timepoints(index);
}
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L70-L94

# [08] Remove unecessary curly braces wrapped around statements or document why it was used

```
{
  (int256 amount0Int, int256 amount1Int, ) = _getAmountsForLiquidity(
    bottomTick,
    topTick,
    int256(liquidityDesired).toInt128(),
    globalState.tick,
    globalState.price
  );

  amount0 = uint256(amount0Int);
  amount1 = uint256(amount1Int);
}
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L435-L446

```
{ 
  if (amount0 > 0) receivedAmount0 = balanceToken0();
  if (amount1 > 0) receivedAmount1 = balanceToken1();
  IAlgebraMintCallback(msg.sender).algebraMintCallback(amount0, amount1, data);
  if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');
  if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');
}
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L450-L456

# [09] Use SafeCast consistently

Some downcasting oprations are using the SafeCast library contract 

Following examples are using SafeCast.

```
(, int256 amount0Int, int256 amount1Int) = _updatePositionTicksAndFees(recipient, bottomTick, topTick, int256(liquidityActual).toInt128()); 
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L472

```
-int256(amount).toInt128()
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L522

Following examples are not using SafeCast.

```
fees0 = uint128(FullMath.mulDiv(innerFeeGrowth0Token - _innerFeeGrowth0Token, currentLiquidity, Constants.Q128)); 
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L247

```
liquidityActual = uint128(FullMath.mulDiv(uint256(liquidityActual), receivedAmount0, amount0));
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L460

Not using SafeCast for all downcasting operations can cause silent overflows. Normalizing SafeCast if preferred.

# [10] Update the solidity version

All the contracts in scope are using 0.7.6.

Use a solidity version of at least 0.8.0 to get overflow protection without SafeMath (Manual downcasting still needs overflow checks even with 0.8).

Use a solidity version of at least 0.8.2 to get compiler automatic inlining.

Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads.

Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings.

# [11] Missing documentation/NATSPEC

Consider document all functions to improve documentation.

```
function getNewPrice(
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L45


# [12] Replace magic numbers with constants to improve code readability

```
res += (xLowestDegree * g) / 5040 + (xLowestDegree * x) / (40320);
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L107
