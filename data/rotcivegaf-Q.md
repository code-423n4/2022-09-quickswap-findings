# QA report

## Non-critical

| N-N    |Issue|Instances|
|:------:|:----|:-------:|
| [N&#x2011;01] | Missing error message in `require` statement | 20 |
| [N&#x2011;02] | Same error message in `require` statement | 4 |

Total: 24 instances over 2 issues

### [N-01] Missing error message in `require` statements

```solidity
File: /src/core/contracts/AlgebraFactory.sol

43    require(msg.sender == owner);

60    require(tokenA != tokenB);

62    require(token0 != address(0));

63    require(poolByPair[token0][token1] == address(0));

78    require(owner != _owner);

85    require(farmingAddress != _farmingAddress);

92    require(vaultAddress != _vaultAddress);
```

```solidity
File: /src/core/contracts/AlgebraPool.sol

 55    require(msg.sender == IAlgebraFactory(factory).owner());

122      require(_lower.initialized);

134      require(_upper.initialized);

229          require((_blockTimestamp() - lastLiquidityAddTimestamp) >= _liquidityCooldown);

953    require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));

960    require(msg.sender == IAlgebraFactory(factory).farmingAddress());

968    require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
```

```solidity
File: /src/core/contracts/AlgebraPoolDeployer.sol

22    require(msg.sender == factory);

27    require(msg.sender == owner);

27    require(_factory != address(0));

28    require(factory == address(0));
```

```solidity
File: /src/core/contracts/DataStorageOperator.sol

43    require(msg.sender == factory || msg.sender == IAlgebraFactory(factory).owner());
```

```solidity
File: /src/core/contracts/libraries/DataStorage.sol

369    require(!self[0].initialized);
```

```solidity
File: /src/core/contracts/libraries/PriceMovementMath.sol

52    require(price > 0);

53    require(liquidity > 0);

70        require((product = amount * price) / amount == price); // if the product overflows, we know the denominator underflows

71        require(liquidityShifted > product); // in addition, we must check that the denominator does not underflow

87        require(price > quotient);
```

```solidity
File: /src/core/contracts/libraries/TokenDeltaMath.sol

30    require(priceDelta < priceUpper); // forbids underflow and 0 priceLower

51    require(priceUpper >= priceLower);
```

### [N-02] Same error message in `require` statement

```solidity
File: /src/core/contracts/AlgebraPool.sol

454      if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');
455      if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');

474      require((amount0 = uint256(amount0Int)) <= receivedAmount0, 'IIAM2');
475      require((amount1 = uint256(amount1Int)) <= receivedAmount1, 'IIAM2');

608      require(balance0Before.add(uint256(amount0)) <= balanceToken0(), 'IIA');
614      require(balance1Before.add(uint256(amount1)) <= balanceToken1(), 'IIA');
641      require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');
645      require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');

739        require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
743        require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
```
