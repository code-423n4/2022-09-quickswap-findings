## Non-critical
## [N-1] `require()`/`revert()` statements should have descriptive reason strings
```solidity
AlgebraPoolDeployer.sol:22:    require(msg.sender == factory);
AlgebraPoolDeployer.sol:27:    require(msg.sender == owner);
AlgebraPoolDeployer.sol:37:    require(_factory != address(0));
AlgebraPoolDeployer.sol:38:    require(factory == address(0));
TokenDeltaMath.sol:30:    require(priceDelta < priceUpper); // forbids underflow and 0 priceLower
TokenDeltaMath.sol:51:    require(priceUpper >= priceLower);
AlgebraFactory.sol:43:    require(msg.sender == owner);
AlgebraFactory.sol:60:    require(tokenA != tokenB);
AlgebraFactory.sol:62:    require(token0 != address(0));
AlgebraFactory.sol:63:    require(poolByPair[token0][token1] == address(0));
AlgebraFactory.sol:78:    require(owner != _owner);
AlgebraFactory.sol:85:    require(farmingAddress != _farmingAddress);
AlgebraFactory.sol:92:    require(vaultAddress != _vaultAddress);
AlgebraPool.sol:55:    require(msg.sender == IAlgebraFactory(factory).owner());
AlgebraPool.sol:122:      require(_lower.initialized);
AlgebraPool.sol:134:      require(_upper.initialized);
AlgebraPool.sol:953:    require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));
AlgebraPool.sol:960:    require(msg.sender == IAlgebraFactory(factory).farmingAddress());
AlgebraPool.sol:968:    require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
DataStorageOperator.sol:43:    require(msg.sender == factory || msg.sender == IAlgebraFactory(factory).owner());
DataStorage.sol:369:    require(!self[0].initialized);
PriceMovementMath.sol:52:    require(price > 0);
PriceMovementMath.sol:53:    require(liquidity > 0);
PriceMovementMath.sol:70:        require((product = amount * price) / amount == price); // if the product overflows, we know the denominator underflows
PriceMovementMath.sol:71:        require(liquidityShifted > product); // in addition, we must check that the denominator does not underflow
PriceMovementMath.sol:87:        require(price > quotient);
```

## [N-02] Use more recent version of Solidity
```solidity
AlgebraPoolDeployer.sol:2:pragma solidity =0.7.6;
TokenDeltaMath.sol:2:pragma solidity =0.7.6;
AlgebraFactory.sol:2:pragma solidity =0.7.6;
Constants.sol:2:pragma solidity =0.7.6;
TickTable.sol:2:pragma solidity =0.7.6;
AdaptiveFee.sol:2:pragma solidity =0.7.6;
PoolImmutables.sol:2:pragma solidity =0.7.6;
TickManager.sol:2:pragma solidity =0.7.6;
AlgebraPool.sol:2:pragma solidity =0.7.6;
DataStorageOperator.sol:2:pragma solidity =0.7.6;
PoolState.sol:2:pragma solidity =0.7.6;
DataStorage.sol:2:pragma solidity =0.7.6;
PriceMovementMath.sol:2:pragma solidity =0.7.6;
```
