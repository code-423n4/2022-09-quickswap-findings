# QUICKSWAP GAS OPTIMIZATION

[S]: Suggested optimation, save a decent amount of gas without compromising readability;

[M]: Minor optimation, the amount of gas saved is minor, change when you see fit;

[N]: Non-preferred, the amount of gas saved is at cost of readability, only apply when gas saving is a top priority.



# ISSUE LIST

#### C4-001 : Adding unchecked directive can save gas [S]
#### C4-002 : Check if amount > 0 before token transfer can save gas [S]
#### C4-003 : There is no need to assign default values to variables [S]
#### C4-004 : Using operator && used more gas [S]
#### C4-005 : Non-strict inequalities are cheaper than strict ones [M]
#### C4-006 : Cache array length in for loops can save gas [S]
#### C4-007 : ++i is more gas efficient than i++ in loops forwarding
#### C4-008 : `> 0` can be replaced with `!= 0` for gas optimization
#### C4-009 : Free gas savings for using solidity 0.8.10+ [S]
#### C4-010 : Use Custom Errors instead of Revert Strings to save Gas [S]
#### C4-011 : Delete - ABI Coder V2 For Gas Optimization
[S]
#### C4-012 : Exponential is more costly [S]



# C4-001 : Adding unchecked directive can save gas

## Impact

For the arithmetic operations that will never over/underflow, using the unchecked directive (Solidity v0.8 has default overflow/underflow checks) can save some gas from the unnecessary internal over/underflow checks.

## Proof of Concept

```
  2022-09-quickswap-main/src/core/contracts/libraries/DataStorage.sol::307 => for (uint256 i = 0; i < secondsAgos.length; i++) {
  2022-09-quickswap-main/src/core/contracts/libraries/TickMath.sol::71 => uint256 msb = 0;
  2022-09-quickswap-main/src/core/contracts/test/DataStorageTest.sol::56 => for (uint256 i = 0; i < params.length; i++) {
```

## Tools Used

None

## Recommended Mitigation Steps

Consider applying unchecked arithmetic where overflow/underflow is not possible. Example can be seen from below.

```
Unchecked{i++};
```

# C4-002 : Check if amount > 0 before token transfer can save gas

## Impact

Since _amount can be 0. Checking if (_amount != 0) before the transfer can potentially save an external call and the unnecessary gas cost of a 0 token transfer.

## Proof of Concept

```
2022-09-quickswap-main/src/core/contracts//AlgebraPool.sol:479:      TransferHelper.safeTransfer(token0, sender, receivedAmount0 - amount0);
2022-09-quickswap-main/src/core/contracts//AlgebraPool.sol:482:      TransferHelper.safeTransfer(token1, sender, receivedAmount1 - amount1);
2022-09-quickswap-main/src/core/contracts//AlgebraPool.sol:505:      if (amount0 > 0) TransferHelper.safeTransfer(token0, recipient, amount0);
2022-09-quickswap-main/src/core/contracts//AlgebraPool.sol:506:      if (amount1 > 0) TransferHelper.safeTransfer(token1, recipient, amount1);
2022-09-quickswap-main/src/core/contracts//AlgebraPool.sol:548:    TransferHelper.safeTransfer(token, vault, amount);
2022-09-quickswap-main/src/core/contracts//AlgebraPool.sol:604:      if (amount1 < 0) TransferHelper.safeTransfer(token1, recipient, uint256(-amount1)); // transfer to recipient
2022-09-quickswap-main/src/core/contracts//AlgebraPool.sol:610:      if (amount0 < 0) TransferHelper.safeTransfer(token0, recipient, uint256(-amount0)); // transfer to recipient
2022-09-quickswap-main/src/core/contracts//AlgebraPool.sol:658:      if (amount1 < 0) TransferHelper.safeTransfer(token1, recipient, uint256(-amount1));
2022-09-quickswap-main/src/core/contracts//AlgebraPool.sol:660:      if (amount0 < amountRequired) TransferHelper.safeTransfer(token0, sender, uint256(amountRequired.sub(amount0)));
2022-09-quickswap-main/src/core/contracts//AlgebraPool.sol:662:      if (amount0 < 0) TransferHelper.safeTransfer(token0, recipient, uint256(-amount0));
2022-09-quickswap-main/src/core/contracts//AlgebraPool.sol:664:      if (amount1 < amountRequired) TransferHelper.safeTransfer(token1, sender, uint256(amountRequired.sub(amount1)));
2022-09-quickswap-main/src/core/contracts//AlgebraPool.sol:906:      TransferHelper.safeTransfer(token0, recipient, amount0);
2022-09-quickswap-main/src/core/contracts//AlgebraPool.sol:913:      TransferHelper.safeTransfer(token1, recipient, amount1);
2022-09-quickswap-main/src/core/contracts//AlgebraPool.sol:929:        TransferHelper.safeTransfer(token0, vault, fees0);
2022-09-quickswap-main/src/core/contracts//AlgebraPool.sol:943:        TransferHelper.safeTransfer(token1, vault, fees1);
```

All Contracts

## Tools Used

None

## Recommended Mitigation Steps

Consider checking amount != 0.

# C4-003 : There is no need to assign default values to variables

## Impact -  Gas Optimization

Uint is default initialized to 0. There is no need assign false to variable.

## Proof of Concept

```
  2022-09-quickswap-main/src/core/contracts/libraries/DataStorage.sol::307 => for (uint256 i = 0; i < secondsAgos.length; i++) {
  2022-09-quickswap-main/src/core/contracts/libraries/TickMath.sol::71 => uint256 msb = 0;
  2022-09-quickswap-main/src/core/contracts/test/DataStorageTest.sol::56 => for (uint256 i = 0; i < params.length; i++) {

```

## Tools Used

Code Review

## Recommended Mitigation Steps

uint x = 0 costs more gas than uint x without having any different functionality.



# C4-004 : Using operator && used more gas

## Impact

Using double require instead of operator && can save more gas.

## Proof of Concept

1. Navigate to the following contracts.

```
2022-09-quickswap-main/src/core/contracts//DataStorageOperator.sol:46:    require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
2022-09-quickswap-main/src/core/contracts//AlgebraPool.sol:739:        require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
2022-09-quickswap-main/src/core/contracts//AlgebraPool.sol:743:        require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
2022-09-quickswap-main/src/core/contracts//AlgebraPool.sol:953:    require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));
2022-09-quickswap-main/src/core/contracts//AlgebraPool.sol:968:    require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
2022-09-quickswap-main/src/core/contracts//AlgebraFactory.sol:110:    require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
2022-09-quickswap-main/src/core/contracts//libraries/TickMath.sol:67:    require(price >= MIN_SQRT_RATIO && price < MAX_SQRT_RATIO, 'R');
2022-09-quickswap-main/src/core/contracts//libraries/TransferHelper.sol:22:    require(success && (data.length == 0 || abi.decode(data, (bool))), 'TF');

```

## Tools Used

Code Review

## Recommended Mitigation Steps

Example

```

using &&:

function check(uint x)public view{
require(x == 0 && x < 1 );
}
// gas cost 21630

using double require:

require(x == 0 );
require( x < 1);
}
}
// gas cost 21622
```


# C4-005 : Non-strict inequalities are cheaper than strict ones

## Impact

Strict inequalities add a check of non equality which costs around 3 gas.

## Proof of Concept

```
2022-09-quickswap-main/src/core/contracts//libraries/PriceMovementMath.sol:155:    if (amountAvailable >= 0) {
2022-09-quickswap-main/src/core/contracts//libraries/PriceMovementMath.sol:159:      if (amountAvailableAfterFee >= input) {
2022-09-quickswap-main/src/core/contracts//libraries/PriceMovementMath.sol:180:      if (uint256(amountAvailable) >= output) resultPrice = targetPrice;
2022-09-quickswap-main/src/core/contracts//libraries/DataStorage.sol:156:    uint256 right = lastIndex >= oldestIndex ? lastIndex : lastIndex + UINT16_MODULO; // newest timepoint considering one index overflow
```

## Tools Used

Code Review

## Recommended Mitigation Steps

Use >= or <= instead of > and < when possible.



# C4-006 : Cache array length in for loops can save gas

## Impact

Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack.

Caching the array length in the stack saves around 3 gas per iteration.

## Proof of Concept

1. Navigate to the following smart contract line.

```
  2022-09-quickswap-main/src/core/contracts/libraries/DataStorage.sol::307 => for (uint256 i = 0; i < secondsAgos.length; i++) {

```

## Tools Used

None

## Recommended Mitigation Steps

Consider to cache array length.



# C4-008 : ++i is more gas efficient than i++ in loops forwarding

## Impact

++i is more gas efficient than i++ in loops forwarding.

## Proof of Concept

1. Navigate to the following contracts.

```
  2022-09-quickswap-main/src/core/contracts/libraries/DataStorage.sol::307 => for (uint256 i = 0; i < secondsAgos.length; i++) {
```

## Tools Used

Code Review

## Recommended Mitigation Steps

It is  recommend to use unchecked{++i} and change i declaration to uint256.


# C4-009 : `> 0` can be replaced with `!= 0` for gas optimization

## Impact

`!= 0` is a cheaper operation compared to `> 0`, when dealing with uint.


## Proof of Concept

1. Navigate to the following contract sections.

```
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::224 => require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::228 => if (_liquidityCooldown > 0) {
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::237 => liquidityNext > 0 ? (liquidityDelta > 0 ? _blockTimestamp() : lastLiquidityAddTimestamp) : 0
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::434 => require(liquidityDesired > 0, 'IL');
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::451 => if (amount0 > 0) receivedAmount0 = balanceToken0();
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::452 => if (amount1 > 0) receivedAmount1 = balanceToken1();
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::454 => if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::455 => if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::469 => require(liquidityActual > 0, 'IIL2');
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::505 => if (amount0 > 0) TransferHelper.safeTransfer(token0, recipient, amount0);
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::506 => if (amount1 > 0) TransferHelper.safeTransfer(token1, recipient, amount1);
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::617 => if (communityFee > 0) {
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::641 => require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::645 => require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::667 => if (communityFee > 0) {
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::734 => (cache.amountRequiredInitial, cache.exactInput) = (amountRequired, amountRequired > 0);
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::808 => if (cache.communityFee > 0) {
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::814 => if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::898 => require(_liquidity > 0, 'L');
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::904 => if (amount0 > 0) {
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::911 => if (amount1 > 0) {
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::924 => if (paid0 > 0) {
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::927 => if (_communityFeeToken0 > 0) {
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::938 => if (paid1 > 0) {
  2022-09-quickswap-main/src/core/contracts/AlgebraPool.sol::941 => if (_communityFeeToken1 > 0) {
  2022-09-quickswap-main/src/core/contracts/DataStorageOperator.sol::46 => require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
  2022-09-quickswap-main/src/core/contracts/DataStorageOperator.sol::138 => if (volume >= 2**192) volumeShifted = (type(uint256).max) / (liquidity > 0 ? liquidity : 1);
  2022-09-quickswap-main/src/core/contracts/DataStorageOperator.sol::139 => else volumeShifted = (volume << 64) / (liquidity > 0 ? liquidity : 1);
  2022-09-quickswap-main/src/core/contracts/interfaces/IAlgebraFactory.sol::113 => * gammas must be > 0
  2022-09-quickswap-main/src/core/contracts/libraries/DataStorage.sol::80 => last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0
  2022-09-quickswap-main/src/core/contracts/libraries/FullMath.sol::114 => require(denominator > 0);
  2022-09-quickswap-main/src/core/contracts/libraries/FullMath.sol::120 => if (mulmod(a, b, denominator) > 0) {
  2022-09-quickswap-main/src/core/contracts/libraries/PriceMovementMath.sol::52 => require(price > 0);
  2022-09-quickswap-main/src/core/contracts/libraries/PriceMovementMath.sol::53 => require(liquidity > 0);
  2022-09-quickswap-main/src/core/contracts/libraries/TickMath.sol::52 => if (tick > 0) ratio = type(uint256).max / ratio;

```

## Tools Used

None

## Recommended Mitigation Steps

Consider to  replace `> 0`  with `!= 0` for gas optimization.


# C4-010 : Free gas savings for using solidity 0.8.10+

## Impact

Using newer compiler versions and the optimizer gives gas optimizations and additional safety checks are available for free.

## Proof of Concept

```
All Contracts
```


Solidity 0.8.13 has a useful change which reduced gas costs of external calls which expect a return value: https://blog.soliditylang.org/2021/11/09/solidity-0.8.10-release-announcement/

Solidity 0.8.15 has some improvements too but not well tested.

Code Generator: Skip existence check for external contract if return data is expected. In this case, the ABI decoder will revert if the contract does not exist

All Contracts

https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L2

## Tools Used

None

## Recommended Mitigation Steps

Consider to upgrade pragma to at least 0.8.15.

# C4-011 : Use Custom Errors instead of Revert Strings to save Gas


Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met)

Source Custom Errors in Solidity:

Starting from Solidity v0.8.4, there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., revert("Insufficient funds.");), but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.

Custom errors are defined using the error statement, which can be used inside and outside of contracts (including interfaces and libraries).

Instances include:

All require Statements


## Tools Used

Code Review

## Recommended Mitigation Steps

Recommended to replace revert strings with custom errors.



# C4-012 : Delete - ABI Coder V2 For Gas Optimization

## Code Location

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L3

## Impact

From Pragma 0.8.0, ABI coder v2 is activated by default. The pragma abicoder v2 can be deleted from the repository. That will provide gas optimization.


## Tools Used

None

## Recommended Mitigation Steps

ABI coder v2 is activated by default. It is recommended to delete redundant codes.

From Solidity v0.8.0 Breaking Changes https://docs.soliditylang.org/en/v0.8.0/080-breaking-changes.html


# C4-013 : Less than 256 uints are not gas efficient

## Impact -  Gas Optimization

Lower than uint256 size storage instance variables are actually less gas efficient. E.g. using uint128 does not give any efficiency, actually, it is the opposite as EVM operates on default of 256-bit values so uint16 is more expensive in this case as it needs a conversion. It only gives improvements in cases where you can pack variables together, e.g. structs.

## Proof of Concept

1. Navigate to the following contracts.

```
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L16
```

## Tools Used

None

## Recommended Mitigation Steps

Consider to review all uint types. Change them with uint256 If the integer is not necessary to present with uint128.`


# C4-014 : Exponential is more costly

## Impact

In the solidity exponential is more costly than 1e18 definition.

## Proof of Concept

1. Navigate to the following contract.

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L138

## Tools Used

None

## Recommended Mitigation Steps

Consider changing 2**18 definition with 2e18.