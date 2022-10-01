### [L-01] ```require()``` should be used instead of ```assert()```


#### Impact
Prior to solidity version 0.8.0, hitting an assert consumes the remainder of the transaction’s available gas rather than returning it, as ```require()```/```revert()``` do. ```assert()``` should be avoided even past solidity version 0.8.0 as its [documentation](https://docs.soliditylang.org/en/v0.8.14/control-structures.html#panic-via-assert-and-error-via-require) states that “The assert function creates an error of type Panic(uint256). … Properly functioning code should never create a Panic, not even on invalid external input. If this happens, then there is a bug in your contract which you should fix”.


#### Findings:
```
contracts/libraries/DataStorage.sol:L190    assert(false);

```
### [L-02] ```require()```/```revert()``` statements should have descriptive strings.


#### Impact
Consider adding descriptive strings in ```require()```/```revert()```.


#### Findings:
```
contracts/AlgebraFactory.sol:L43    require(msg.sender == owner);

contracts/AlgebraFactory.sol:L60    require(tokenA != tokenB);

contracts/AlgebraFactory.sol:L62    require(token0 != address(0));

contracts/AlgebraFactory.sol:L63    require(poolByPair[token0][token1] == address(0));

contracts/AlgebraFactory.sol:L78    require(owner != _owner);

contracts/AlgebraFactory.sol:L85    require(farmingAddress != _farmingAddress);

contracts/AlgebraFactory.sol:L92    require(vaultAddress != _vaultAddress);

contracts/AlgebraPool.sol:L55    require(msg.sender == IAlgebraFactory(factory).owner());

contracts/AlgebraPool.sol:L122      require(_lower.initialized);

contracts/AlgebraPool.sol:L134      require(_upper.initialized);

contracts/AlgebraPool.sol:L229          require((_blockTimestamp() - lastLiquidityAddTimestamp) >= _liquidityCooldown);

contracts/AlgebraPool.sol:L953    require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));

contracts/AlgebraPool.sol:L960    require(msg.sender == IAlgebraFactory(factory).farmingAddress());

contracts/AlgebraPool.sol:L968    require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);

contracts/AlgebraPoolDeployer.sol:L22    require(msg.sender == factory);

contracts/AlgebraPoolDeployer.sol:L27    require(msg.sender == owner);

contracts/AlgebraPoolDeployer.sol:L37    require(_factory != address(0));

contracts/AlgebraPoolDeployer.sol:L38    require(factory == address(0));

contracts/DataStorageOperator.sol:L43    require(msg.sender == factory || msg.sender == IAlgebraFactory(factory).owner());

contracts/libraries/DataStorage.sol:L369    require(!self[0].initialized);

contracts/libraries/PriceMovementMath.sol:L52    require(price > 0);

contracts/libraries/PriceMovementMath.sol:L53    require(liquidity > 0);

contracts/libraries/PriceMovementMath.sol:L70        require((product = amount * price) / amount == price); // if the product overflows, we know the denominator underflows

contracts/libraries/PriceMovementMath.sol:L71        require(liquidityShifted > product); // in addition, we must check that the denominator does not underflow

contracts/libraries/PriceMovementMath.sol:L87        require(price > quotient);

contracts/libraries/TokenDeltaMath.sol:L30    require(priceDelta < priceUpper); // forbids underflow and 0 priceLower

contracts/libraries/TokenDeltaMath.sol:L51    require(priceUpper >= priceLower);

```

### [L-03] zero-address checks are missing


#### Impact
Zero-address checks are a best practice for input validation of critical address parameters. Accidental use of zero-addresses may result in exceptions, burn fees/tokens, or force redeployment of contracts.

#### Findings:
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L54-L55
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L80
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L87
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L94


