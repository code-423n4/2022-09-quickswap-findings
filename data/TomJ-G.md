## Table of Contents
Total of 8 issues found
- Use require instead of &&
- Internal Function Called Only Once can be Inlined
- Use Bit Shifting Instead of Multiplication/Division of 2
- Use Calldata instead of Memory for Read Only Function Parameters
- Store Array's Length as a Variable
- ++i Costs Less Gas than i++
- != 0 costs less gass than > 0
- Use Custom Errors to Save Gas

&ensp;
### Use require instead of &&

#### Issue
When there are multiple conditions in require statement, break down the require statement into
multiple require statements instead of using && can save gas.

#### PoC
Total of 6 instances found.
```solidity
./DataStorageOperator.sol:46:    require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
./AlgebraPool.sol:739:        require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
./AlgebraPool.sol:743:        require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
./AlgebraPool.sol:953:    require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));
./AlgebraPool.sol:968:    require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
./AlgebraFactory.sol:110:    require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
```

#### Mitigation
Break down into several require statement.
For example,
```solidity
require(_feeConfig.gamma1 != 0);
require(_feeConfig.gamma2 != 0); 
require(_feeConfig.volumeGamma != 0, 'Gammas must be > 0');
```

&ensp;
### Internal Function Called Only Once Can be Inlined

#### Issue
Certain function is defined even though it is called only once.
Inline it instead to where it is called to avoid usage of extra gas.

#### PoC
Total of 6 instances found.

1. getMostSignificantBit() of TickTable.sol
This function called only once at line 92
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickTable.sol#L46
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickTable.sol#L92

2. computeAddress() of AlgebraFactory.sol
This function called only once at line 65
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L122
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L65

3. _recalculatePosition() of AlgebraPool.sol
This function called only once at line 342
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L215
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L342

4. getNewPriceAfterInput() of PriceMovementMath.sol
This function called only once at line 163
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L20
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L163

5. getNewPriceAfterOutput() of PriceMovementMath.sol
This function called only once at line 182
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L36
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L182

6. _volatilityOnRange() of DataStorage.sol
This function called only once at line 81
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L32
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L81

#### Mitigation
I recommend to not define above functions and instead inline it at place it is called.

&ensp;
### Use Bit Shifting Instead of Multiplication/Division of 2

#### Issue
The MUL and DIV opcodes cost 5 gas but SHL and SHR only costs 3 gas.
Since MUL/DIV and SHL/SHR result the same, use cheaper bit shifting.

#### PoC
Total of 1 instance found.
```solidity
./AdaptiveFee.sol:88:    res += (xLowestDegree * gHighestDegree) / 2;
```

#### Mitigation
Use bit shifting instead of multiplication/division.
Example:
```solidity
uint256 center = upper - (upper - lower) / 2; 
Good:
uint256 center = upper - (upper - lower) >> 2; 
```

&ensp;
### Use Calldata instead of Memory for Read Only Function Parameters

#### Issue
It is cheaper gas to use calldata than memory if the function parameter is read only.
Calldata is a non-modifiable, non-persistent area where function arguments are stored, 
and behaves mostly like memory. More details on following link.
link: https://docs.soliditylang.org/en/v0.8.15/types.html#data-location

#### PoC
Total of 1 instance found.
```solidity
./DataStorageOperator.sol:90:    uint32[] memory secondsAgos,
```

#### Mitigation
Change memory to calldata

&ensp;
### Store Array's Length as a Variable 

#### Issue
By storing an array's length as a variable before the for-loop,
can save 3 gas per iteration.

#### PoC
Total of 1 instance found.
```solidity
./DataStorage.sol:307:    for (uint256 i = 0; i < secondsAgos.length; i++) {
```

#### Mitigation
Store array's length as a variable before looping it.
For example, I suggest changing it to
```solidity
uint256 len = secondsAgosLength;
for (uint256 i = 0; i < len; i++) {
```

&ensp;
### ++i Costs Less Gas than i++

#### Issue
Prefix increments/decrements (++i or --i) costs cheaper gas than 
postfix increment/decrements (i++ or i--).

#### PoC
Total of 1 instance found.
```solidity
./DataStorage.sol:307:    for (uint256 i = 0; i < secondsAgos.length; i++) {
```

#### Mitigation
Change it to postfix increments/decrements.
It saves 6 gas per loop.

&ensp;
### != 0 costs less gass than > 0

#### Issue
!= 0 costs less gas when optimizer is enabled and is used for unsigned integers in require statement.

#### PoC
Total of 8 instances found.
```solidity
./AlgebraPool.sol:224:      require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges
./AlgebraPool.sol:434:    require(liquidityDesired > 0, 'IL');
./AlgebraPool.sol:454:      if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');
./AlgebraPool.sol:455:      if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');
./AlgebraPool.sol:469:    require(liquidityActual > 0, 'IIL2');
./AlgebraPool.sol:898:    require(_liquidity > 0, 'L');
./PriceMovementMath.sol:52:    require(price > 0);
./PriceMovementMath.sol:53:    require(liquidity > 0);
```

#### Mitigation
I suggest changing > 0 to != 0

&ensp;
### Use Custom Errors to Save Gas

#### Issue
Custom errors from Solidity 0.8.4 are cheaper than revert strings.
I suggest implementing custom errors to save gas.
Reference: https://blog.soliditylang.org/2021/04/21/custom-errors/

#### PoC
Total of 32 instances found.
```solidity
./TickManager.sol:96:    require(liquidityTotalAfter < Constants.MAX_LIQUIDITY_PER_TICK + 1, 'LO');
./DataStorageOperator.sol:27:    require(msg.sender == pool, 'only pool can call this');
./DataStorageOperator.sol:45:    require(uint256(_feeConfig.alpha1) + uint256(_feeConfig.alpha2) + uint256(_feeConfig.baseFee) <= type(uint16).max, 'Max fee exceeded');
./DataStorageOperator.sol:46:    require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
./AlgebraPool.sol:60:    require(topTick < TickMath.MAX_TICK + 1, 'TUM');
./AlgebraPool.sol:61:    require(topTick > bottomTick, 'TLU');
./AlgebraPool.sol:62:    require(bottomTick > TickMath.MIN_TICK - 1, 'TLM');
./AlgebraPool.sol:194:    require(globalState.price == 0, 'AI');
./AlgebraPool.sol:224:      require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges
./AlgebraPool.sol:434:    require(liquidityDesired > 0, 'IL');
./AlgebraPool.sol:454:      if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');
./AlgebraPool.sol:455:      if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');
./AlgebraPool.sol:469:    require(liquidityActual > 0, 'IIL2');
./AlgebraPool.sol:474:      require((amount0 = uint256(amount0Int)) <= receivedAmount0, 'IIAM2');
./AlgebraPool.sol:475:      require((amount1 = uint256(amount1Int)) <= receivedAmount1, 'IIAM2');
./AlgebraPool.sol:608:      require(balance0Before.add(uint256(amount0)) <= balanceToken0(), 'IIA');
./AlgebraPool.sol:614:      require(balance1Before.add(uint256(amount1)) <= balanceToken1(), 'IIA');
./AlgebraPool.sol:636:    require(globalState.unlocked, 'LOK');
./AlgebraPool.sol:641:      require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');
./AlgebraPool.sol:645:      require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');
./AlgebraPool.sol:731:      require(unlocked, 'LOK');
./AlgebraPool.sol:733:      require(amountRequired != 0, 'AS');
./AlgebraPool.sol:739:        require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
./AlgebraPool.sol:743:        require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
./AlgebraPool.sol:898:    require(_liquidity > 0, 'L');
./AlgebraPool.sol:921:    require(balance0Before.add(fee0) <= paid0, 'F0');
./AlgebraPool.sol:935:    require(balance1Before.add(fee1) <= paid1, 'F1');
./TickTable.sol:15:    require(tick % Constants.TICK_SPACING == 0, 'tick is not spaced'); // ensure that the tick is spaced
./PoolState.sol:41:    require(globalState.unlocked, 'LOK');
./AlgebraFactory.sol:109:    require(uint256(alpha1) + uint256(alpha2) + uint256(baseFee) <= type(uint16).max, 'Max fee exceeded');
./AlgebraFactory.sol:110:    require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
./DataStorage.sol:238:    require(lteConsideringOverflow(self[oldestIndex].blockTimestamp, target, time), 'OLD');
```

&ensp;