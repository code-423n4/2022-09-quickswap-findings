# UPGRADE PRAGMA TO AT LEAST 0.8.4

Using newer compiler versions and the optimizer give gas optimizations. Also, additional safety checks are available for free.

The advantages here are:

- Low level inliner (>= 0.8.2): Cheaper runtime gas (especially relevant when the contract has small functions).
- Optimizer improvements in packed structs (>= 0.8.3)
- Custom errors (>= 0.8.4): cheaper deployment cost and runtime cost. Note: the runtime cost is only relevant when the revert condition is met. In short, replace revert strings by custom errors.

Most, if not All contract in review are using `pragma solidity =0.7.6;` Consider upgrading pragma to at least 0.8.4.

# USE CUSTOM ERRORS INSTEAD OF REVERT STRINGS TO SAVE GAS

Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met)

Source: https://blog.soliditylang.org/2021/04/21/custom-errors/:
> Starting from Solidity v0.8.4, there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., revert("Insufficient funds.");), but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.

Custom errors are defined using the error statement, which can be used inside and outside of contracts (including interfaces and libraries).

Instances include:

AlgebraFactory.sol#L109-110
```
require(uint256(alpha1) + uint256(alpha2) + uint256(baseFee) <= type(uint16).max, 'Max fee exceeded');
require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
```

DataStorageOperator.sol#L45-46
```
require(uint256(_feeConfig.alpha1) + uint256(_feeConfig.alpha2) + uint256(_feeConfig.baseFee) <= type(uint16).max, 'Max fee exceeded');
require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
```


# NO NEED TO EXPLICITLY INITIALIZE VARIABLES WITH DEFAULT VALUES

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: `for (uint256 i = 0; i < secondsAgos.length; i++) ` (DataStorage.sol#L307)

# SPLITTING REQUIRE() STATEMENTS THAT USE && SAVES GAS

If you’re using the Optimizer at 200, instead of using the && operator in a single require statement to check multiple conditions, I suggest using multiple require statements with 1 condition per require statement:

example instances:
```
AlgebraFactory.sol#L110      require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
AlgebraPool.sol#L739          require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
AlgebraPool.sol#L743          require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
AlgebraPool.sol#L953          require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));
AlgebraPool.sol#L968          require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
DataStorageOperator.sol#L46      require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');

```