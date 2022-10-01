# QA Report

## Summary

|               | Issue         | Risk     | Instances     |
| :-------------: |:-------------|:-------------:|:-------------:|
| 1      | Solidity version less than 0.8 should use `safeMath` library | Low |  |
| 2      | Changing ownership missing check for `address(0)` in `setOwner` function | Low | 1 |
| 3      | Missing checks for `address(0)` when assigning values to `address` state variables | Low | 3 |
| 4      | Setters should check the input value and revert if it's the zero address or zero | Low | 3 |
| 5      | `require()`/`revert()` statements should have descriptive reason strings | NC | 26 |
| 6      | Use more recent solidity versions | NC |  |
| 7      | Named return variables not used anywhere in the functions | NC | 11 |
| 8      | `2**<N> - 1` should be re-written as `type(uint<N>).max`  | NC | 1 |
| 9      | `constant` should be defined rather than using magic numbers  | NC | 2 |

## Findings

### 1- Solidity version less than 0.8 should use safeMath library  :

Solidity version less than 0.8 doesn't come with implicit overflow and underflow checks on unsigned integers which will cause arithmetic errors on operations such as addition or substraction, this could provoke vulnerabilities in the code and could cause loss of funds or protocol logic errors.

#### Impact - Low Risk

#### Mitigation

There are 2 options to fix this issue :

* Consider using more recent solidity versions 0.8+.

* Use openzeppelin safeMath library.

### 2- Changing ownership missing check for `address(0)` in `setOwner` function :

The `setOwner` function from the `AlgebraFactory` contract is missing a check for `address(0)` when setting a new owner which could lead to the loss of the ownership of the contract if an error occurs.

#### Impact - Low Risk

#### Proof of Concept

Instances include:

File: src/core/contracts/AlgebraFactory.sol

[function setOwner(address _owner)](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L77-L81)

#### Mitigation

There are 2 options to fix this issue :

* The simple one is to add non-zero address check on the new owner address in the `setOwner` function to avoid burning ownership by accident.

* The safer option is to use a two-step ownership transfer process in order to avoid giving ownership to a wrong address or zero address by accident, because with the two-step ownership the new owner must approve the transfer.

### 3- Missing checks for `address(0)` when assigning values to `address` state variables  :

Constructors should check that the values written in an immutable addresses variables are not the zero address.

#### Impact - Low Risk

#### Proof of Concept

Instances include:

File: src/core/contracts/AlgebraFactory.sol

[poolDeployer = _poolDeployer;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L54)

File: src/core/contracts/DataStorageOperator.sol

[factory = msg.sender;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L32)

[pool = _pool;](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L33)

#### Mitigation
Add non-zero address checks in the constructors for the instances aforementioned.

### 4- Setters should check the input value and revert if it's the zero address or zero  :

When a setting a new value to a state variable the setter function should check the new value and revert if it's the zero address or zero.

#### Impact - Low Risk

#### Proof of Concept

Instances include:

File: src/core/contracts/AlgebraFactory.sol

[function setOwner(address _owner)](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L77-L81)

[function setFarmingAddress(address _farmingAddress)](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L84-L88)

[ function setVaultAddress(address _vaultAddress)](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L91-L95)

File: src/core/contracts/AlgebraPool.sol

[function setIncentive(address virtualPoolAddress)](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L959-L964)

#### Mitigation
Add non-zero address checks in the setters for the instances aforementioned.

### 5- `require()`/`revert()` statements should have descriptive reason strings :

#### Impact - NON CRITICAL

#### Proof of Concept
Instances include:

```
File: src/core/contracts/libraries/PriceMovementMath.sol

52       require(price > 0);
53       require(liquidity > 0);
70       require((product = amount * price) / amount == price);
71       require(liquidityShifted > product);
87       require(price > quotient);

File: src/core/contracts/libraries/TokenDeltaMath.sol

30       require(priceDelta < priceUpper);
51       require(priceUpper >= priceLower);

File: src/core/contracts/AlgebraFactory.sol

43       require(msg.sender == owner);
60       require(tokenA != tokenB);
62       require(token0 != address(0));
63       require(poolByPair[token0][token1] == address(0));
78       require(owner != _owner);
85       require(farmingAddress != _farmingAddress);
92       require(vaultAddress != _vaultAddress);

File: src/core/contracts/AlgebraPool.sol

55       require(msg.sender == IAlgebraFactory(factory).owner());
122      require(_lower.initialized);
134      require(_upper.initialized);
229      require((_blockTimestamp() - lastLiquidityAddTimestamp) >= _liquidityCooldown);
953      require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));
960      require(msg.sender == IAlgebraFactory(factory).farmingAddress());
968      require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);

File: src/core/contracts/AlgebraPoolDeployer.sol

22       require(msg.sender == factory);
27       require(msg.sender == owner);
37       require(_factory != address(0));
38       require(factory == address(0));

File: src/core/contracts/DataStorageOperator.sol

43       require(msg.sender == factory || msg.sender == IAlgebraFactory(factory).owner());
```

#### Mitigation
Add reason strings to the aforementioned require statements for better comprehension.

### 6- Use more recent solidity versions :

#### Impact - NON CRITICAL

All contracts contained in this project use an old solidity version `pragma solidity =0.7.6`, consider using a more recent solidity version to get access to new features which save gas and to avoid old versions vulnerabilities (such as under/overflows) :

* Solidity version of at least 0.8.4 to get access to Custom errors which are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met) while providing the same amount of information.

* Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers which will prevent arithmetic errors in the code and the developers will not have to use safeMath library.

### 7- Named return variables not used anywhere in the function :

When Named return variable are declared they should be used inside the function instead of the return statement or if not they should be removed to avoid confusion.

#### Impact - NON CRITICAL

#### Proof of Concept
Instances include:

File: src/core/contracts/libraries/AdaptiveFee.sol

[returns (uint16 fee)](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L29)

File: src/core/contracts/libraries/DataStorage.sol

[returns (Timepoint memory targetTimepoint)](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L213)

[returns (uint88 volatilityAverage, uint256 volumePerLiqAverage)](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L332)

File: src/core/contracts/libraries/PriceMovementMath.sol

[returns (uint160 resultPrice)](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L25)

[returns (uint160 resultPrice)](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L41)

[returns (uint160 resultPrice)](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L51)

File: src/core/contracts/libraries/TickManager.sol

[returns (int128 liquidityDelta)](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L137)

File: src/core/contracts/libraries/TickTable.sol

[returns (uint8 mostBitPos)](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L46)

[returns (int24 nextTick, bool initialized)](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L72)

File: src/core/contracts/AlgebraPool.sol

[returns (bool initialized, uint32 blockTimestamp, int56 tickCumulative, uint160 secondsPerLiquidityCumulative, uint88 volatilityCumulative, int24 averageTick, uint144 volumePerLiquidityCumulative)](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L83-L91)

[returns (int56 innerTickCumulative, uint160 innerSecondsSpentPerLiquidity, uint32 innerSecondsSpent)](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L108-L111)

#### Mitigation

Either use the named return variables inplace of the return statement or remove them.

### 8- `2**<N> - 1` should be re-written as `type(uint<N>).max` :

#### Impact - NON CRITICAL

#### Proof of Concept
Instances include:

File: src/core/contracts/libraries/PriceMovementMath.sol

[if (volume >= 2**192)](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L138)

#### Mitigation
Replace the aforementioned statements for better readability.

### 9- `constant` should be defined rather than using magic numbers :

It is best practice to use constant variables rather than hex/numeric literal values to make the code easier to understand and maintain, but if they are used those numbers should be well docummented. 

#### Impact - NON CRITICAL

#### Proof of Concept
Instances include:

File: src/core/contracts/libraries/PriceMovementMath.sol

[1e6](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L157-L195)

File: src/core/contracts/DataStorageOperator.sol

[return AdaptiveFee.getFee(volatilityAverage / 15, volumePerLiqAverage, feeConfig);](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L158)

#### Mitigation
Replace the hex/numeric literals aforementioned with `constants`.