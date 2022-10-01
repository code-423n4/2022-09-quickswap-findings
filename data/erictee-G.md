### [G-01] ```abi.encode()``` is less efficient than ```abi.encodePacked()```


#### Impact
Consider using ```abi.encodePacked()``` instead for efficieny.


#### Findings:
```
contracts/AlgebraFactory.sol:L123    pool = address(uint256(keccak256(abi.encodePacked(hex'ff', poolDeployer, keccak256(abi.encode(token0, token1)), POOL_INIT_CODE_HASH))));

contracts/AlgebraPoolDeployer.sol:L51    pool = address(new AlgebraPool{salt: keccak256(abi.encode(token0, token1))}());

```
###  [G-02] Cache Array Length Outside of Loop


#### Impact
Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.


#### Findings:
```
contracts/libraries/DataStorage.sol:L307    for (uint256 i = 0; i < secondsAgos.length; i++) {

```
### [G-03] Use custom errors rather than ```revert()```/```require()``` string to save gas


#### Impact
Custom errors are available from solidity version 0.8.4, it saves around 50 gas each time they are hit by avoiding having to allocate and store the revert string.


#### Findings:
```
contracts/AlgebraFactory.sol:L109    require(uint256(alpha1) + uint256(alpha2) + uint256(baseFee) <= type(uint16).max, 'Max fee exceeded');

contracts/AlgebraFactory.sol:L110    require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');

contracts/AlgebraPool.sol:L60    require(topTick < TickMath.MAX_TICK + 1, 'TUM');

contracts/AlgebraPool.sol:L61    require(topTick > bottomTick, 'TLU');

contracts/AlgebraPool.sol:L62    require(bottomTick > TickMath.MIN_TICK - 1, 'TLM');

contracts/AlgebraPool.sol:L194    require(globalState.price == 0, 'AI');

contracts/AlgebraPool.sol:L224      require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges

contracts/AlgebraPool.sol:L434    require(liquidityDesired > 0, 'IL');

contracts/AlgebraPool.sol:L454      if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');

contracts/AlgebraPool.sol:L455      if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');

contracts/AlgebraPool.sol:L469    require(liquidityActual > 0, 'IIL2');

contracts/AlgebraPool.sol:L474      require((amount0 = uint256(amount0Int)) <= receivedAmount0, 'IIAM2');

contracts/AlgebraPool.sol:L475      require((amount1 = uint256(amount1Int)) <= receivedAmount1, 'IIAM2');

contracts/AlgebraPool.sol:L608      require(balance0Before.add(uint256(amount0)) <= balanceToken0(), 'IIA');

contracts/AlgebraPool.sol:L614      require(balance1Before.add(uint256(amount1)) <= balanceToken1(), 'IIA');

contracts/AlgebraPool.sol:L636    require(globalState.unlocked, 'LOK');

contracts/AlgebraPool.sol:L641      require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');

contracts/AlgebraPool.sol:L645      require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');

contracts/AlgebraPool.sol:L731      require(unlocked, 'LOK');

contracts/AlgebraPool.sol:L733      require(amountRequired != 0, 'AS');

contracts/AlgebraPool.sol:L739        require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');

contracts/AlgebraPool.sol:L743        require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');

contracts/AlgebraPool.sol:L898    require(_liquidity > 0, 'L');

contracts/AlgebraPool.sol:L921    require(balance0Before.add(fee0) <= paid0, 'F0');

contracts/AlgebraPool.sol:L935    require(balance1Before.add(fee1) <= paid1, 'F1');

contracts/DataStorageOperator.sol:L27    require(msg.sender == pool, 'only pool can call this');

contracts/DataStorageOperator.sol:L45    require(uint256(_feeConfig.alpha1) + uint256(_feeConfig.alpha2) + uint256(_feeConfig.baseFee) <= type(uint16).max, 'Max fee exceeded');

contracts/DataStorageOperator.sol:L46    require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');

contracts/libraries/DataStorage.sol:L238    require(lteConsideringOverflow(self[oldestIndex].blockTimestamp, target, time), 'OLD');

contracts/libraries/TickTable.sol:L15    require(tick % Constants.TICK_SPACING == 0, 'tick is not spaced'); // ensure that the tick is spaced

contracts/libraries/TickManager.sol:L96    require(liquidityTotalAfter < Constants.MAX_LIQUIDITY_PER_TICK + 1, 'LO');

contracts/base/PoolState.sol:L41    require(globalState.unlocked, 'LOK');

```
### [G-04] Using ```> 0``` costs more gas than ```!= 0``` when used on a uint in a ```require()``` statement.


#### Impact
When working with unsigned integer types, comparisons with != 0 are cheaper than with > 0 . This changes saves 6 gas per instance.


#### Findings:
```
contracts/AlgebraPool.sol:L224      require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges

contracts/AlgebraPool.sol:L434    require(liquidityDesired > 0, 'IL');

contracts/AlgebraPool.sol:L454      if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');

contracts/AlgebraPool.sol:L455      if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');

contracts/AlgebraPool.sol:L469    require(liquidityActual > 0, 'IIL2');

contracts/AlgebraPool.sol:L641      require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');

contracts/AlgebraPool.sol:L645      require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');

contracts/AlgebraPool.sol:L898    require(_liquidity > 0, 'L');

contracts/libraries/PriceMovementMath.sol:L52    require(price > 0);

contracts/libraries/PriceMovementMath.sol:L53    require(liquidity > 0);

```
### [G-05] Functions guaranteed to revert when called by normal users can be declared as payable.


#### Impact
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.


#### Findings:
```
contracts/AlgebraFactory.sol:L77  function setOwner(address _owner) external override onlyOwner {

contracts/AlgebraFactory.sol:L84  function setFarmingAddress(address _farmingAddress) external override onlyOwner {

contracts/AlgebraFactory.sol:L91  function setVaultAddress(address _vaultAddress) external override onlyOwner {

contracts/AlgebraPool.sol:L952  function setCommunityFee(uint8 communityFee0, uint8 communityFee1) external override lock onlyFactoryOwner {

contracts/AlgebraPool.sol:L967  function setLiquidityCooldown(uint32 newLiquidityCooldown) external override onlyFactoryOwner {

contracts/AlgebraPoolDeployer.sol:L36  function setFactory(address _factory) external override onlyOwner {

contracts/DataStorageOperator.sol:L37  function initialize(uint32 time, int24 tick) external override onlyPool {

```
### [G-06] ```X += Y``` costs more gas than ```X = X + Y``` for state variables.


#### Impact
Consider changing ```X += Y``` to ```X = X + Y``` to save gas.


#### Findings:
```
contracts/AlgebraPool.sol:L257      _position.fees0 += fees0;

contracts/AlgebraPool.sol:L258      _position.fees1 += fees1;

contracts/AlgebraPool.sol:L804        amountRequired += step.output.toInt256(); // increase remaining output amount (since its negative)

contracts/AlgebraPool.sol:L811        communityFeeAmount += delta;

contracts/AlgebraPool.sol:L814      if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);

contracts/AlgebraPool.sol:L931      totalFeeGrowth0Token += FullMath.mulDiv(paid0 - fees0, Constants.Q128, _liquidity);

contracts/AlgebraPool.sol:L945      totalFeeGrowth1Token += FullMath.mulDiv(paid1 - fees1, Constants.Q128, _liquidity);

contracts/libraries/DataStorage.sol:L79    last.tickCumulative += int56(tick) * delta;

contracts/libraries/DataStorage.sol:L80    last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0

contracts/libraries/DataStorage.sol:L81    last.volatilityCumulative += uint88(_volatilityOnRange(delta, prevTick, tick, last.averageTick, averageTick)); // always fits 88 bits

contracts/libraries/DataStorage.sol:L83    last.volumePerLiquidityCumulative += volumePerLiquidity;

contracts/libraries/DataStorage.sol:L251      beforeOrAt.tickCumulative += ((atOrAfter.tickCumulative - beforeOrAt.tickCumulative) / timepointTimeDelta) * targetDelta;

contracts/libraries/DataStorage.sol:L252      beforeOrAt.secondsPerLiquidityCumulative += uint160(

contracts/libraries/DataStorage.sol:L255      beforeOrAt.volatilityCumulative += ((atOrAfter.volatilityCumulative - beforeOrAt.volatilityCumulative) / timepointTimeDelta) * targetDelta;

contracts/libraries/DataStorage.sol:L256      beforeOrAt.volumePerLiquidityCumulative +=

contracts/libraries/AdaptiveFee.sol:L84    res += xLowestDegree * gHighestDegree;

contracts/libraries/AdaptiveFee.sol:L88    res += (xLowestDegree * gHighestDegree) / 2;

contracts/libraries/AdaptiveFee.sol:L92    res += (xLowestDegree * gHighestDegree) / 6;

contracts/libraries/AdaptiveFee.sol:L96    res += (xLowestDegree * gHighestDegree) / 24;

contracts/libraries/AdaptiveFee.sol:L100    res += (xLowestDegree * gHighestDegree) / 120;

contracts/libraries/AdaptiveFee.sol:L104    res += (xLowestDegree * gHighestDegree) / 720;

contracts/libraries/AdaptiveFee.sol:L107    res += (xLowestDegree * g) / 5040 + (xLowestDegree * x) / (40320);

contracts/libraries/TickTable.sol:L100      tick += 1;

contracts/libraries/TickTable.sol:L112        tick += int24(getSingleSignificantBit(-_row & _row)); // least significant bit

contracts/libraries/TickTable.sol:L115        tick += int24(255 - bitNumber);

```
### [G-07] ++i costs less gas than i++, especially when it's used in for loops.


#### Impact
Saves 6 gas per loop.


#### Findings:
```
contracts/libraries/DataStorage.sol:L307    for (uint256 i = 0; i < secondsAgos.length; i++) {

```
### [G-08] Using private rather than public for constants to saves gas.


#### Impact
If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table.


#### Findings:
```
contracts/libraries/DataStorage.sol:L12  uint32 public constant WINDOW = 1 days;

```
### [G-09] Splitting ```require()``` statements that use && saves gas.


#### Impact
Consider splitting the ```require()``` statements to save gas.


#### Findings:
```
contracts/AlgebraFactory.sol:L110    require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');

contracts/AlgebraPool.sol:L739        require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');

contracts/AlgebraPool.sol:L743        require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');

contracts/AlgebraPool.sol:L953    require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));

contracts/AlgebraPool.sol:L968    require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);

contracts/DataStorageOperator.sol:L46    require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');

```
### [G-10] Explicit initialization with zero not required


#### Impact
Explicit initialization with zero is not required for variable declaration because uints are 0 by default. Removing this will reduce contract size and save a bit of gas.


#### Findings:
```
contracts/libraries/DataStorage.sol:L307    for (uint256 i = 0; i < secondsAgos.length; i++) {

```
### [G-11] ```++i```/```i++``` should be ```unchecked{++i}```/```unchecked{i++}``` when it is not possible for them to overflow, as is the case when used in for- and while-loops.


#### Impact
The ```unchecked``` keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop.


#### Findings:
```
contracts/libraries/DataStorage.sol:L307    for (uint256 i = 0; i < secondsAgos.length; i++) {

```
### [G-12] Use a more recent version of solidity.


#### Impact
Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining 
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads 
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings 
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value.


#### Findings:
```
contracts/AlgebraFactory.sol:L2      pragma solidity =0.7.6;

contracts/AlgebraPool.sol:L2      pragma solidity =0.7.6;

contracts/AlgebraPoolDeployer.sol:L2      pragma solidity =0.7.6;

contracts/DataStorageOperator.sol:L2      pragma solidity =0.7.6;

contracts/libraries/DataStorage.sol:L2      pragma solidity =0.7.6;

contracts/libraries/AdaptiveFee.sol:L2      pragma solidity =0.7.6;

contracts/libraries/TickTable.sol:L2      pragma solidity =0.7.6;

contracts/libraries/PriceMovementMath.sol:L2      pragma solidity =0.7.6;

contracts/libraries/Constants.sol:L2      pragma solidity =0.7.6;

contracts/libraries/TokenDeltaMath.sol:L2      pragma solidity =0.7.6;

contracts/libraries/TickManager.sol:L2      pragma solidity =0.7.6;

contracts/base/PoolImmutables.sol:L2      pragma solidity =0.7.6;

contracts/base/PoolState.sol:L2      pragma solidity =0.7.6;

```
