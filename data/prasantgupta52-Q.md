# 1. Avoid using Floating Pragma 
### Description: 
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively. 
### Links to github files
[AlgebraPool.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L2)
[DataStorageOperator.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L2)
[AlgebraFactory.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L2)
[PoolImmutables.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolImmutables.sol#L2)
[PoolState.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolState.sol#L2)
[AlgebraPoolDeployer.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L2)
[TokenDeltaMath.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TokenDeltaMath.sol#L2)
[Constants.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/Constants.sol#L2)
[DataStorage.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L2)
[AdaptiveFee.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L2)
[TickManager.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickManager.sol#L2)
[TickTable.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickTable.sol#L2)
[PriceMovementMath.sol:L2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L2)
### Instances 
``` 
src/core/contracts/AlgebraPool.sol:2:pragma solidity =0.7.6;
src/core/contracts/DataStorageOperator.sol:2:pragma solidity =0.7.6;
src/core/contracts/AlgebraFactory.sol:2:pragma solidity =0.7.6;
src/core/contracts/base/PoolImmutables.sol:2:pragma solidity =0.7.6;
src/core/contracts/base/PoolState.sol:2:pragma solidity =0.7.6;
src/core/contracts/AlgebraPoolDeployer.sol:2:pragma solidity =0.7.6;
src/core/contracts/libraries/TokenDeltaMath.sol:2:pragma solidity =0.7.6;
src/core/contracts/libraries/Constants.sol:2:pragma solidity =0.7.6;
src/core/contracts/libraries/DataStorage.sol:2:pragma solidity =0.7.6;
src/core/contracts/libraries/AdaptiveFee.sol:2:pragma solidity =0.7.6;
src/core/contracts/libraries/TickManager.sol:2:pragma solidity =0.7.6;
src/core/contracts/libraries/TickTable.sol:2:pragma solidity =0.7.6;
src/core/contracts/libraries/PriceMovementMath.sol:2:pragma solidity =0.7.6;
``` 
----
# 2. require()/revert() statements should have descriptive reason strings 
### Links to github files
[AlgebraPool.sol:L55](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L55)
[AlgebraPool.sol:L122](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L122)
[AlgebraPool.sol:L134](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L134)
[AlgebraPool.sol:L229](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L229)
[AlgebraPool.sol:L953](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L953)
[AlgebraPool.sol:L960](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L960)
[AlgebraPool.sol:L968](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L968)
[DataStorageOperator.sol:L43](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L43)
[AlgebraFactory.sol:L43](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L43)
[AlgebraFactory.sol:L60](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L60)
[AlgebraFactory.sol:L62](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L62)
[AlgebraFactory.sol:L63](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L63)
[AlgebraFactory.sol:L78](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L78)
[AlgebraFactory.sol:L85](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L85)
[AlgebraFactory.sol:L92](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L92)
[AlgebraPoolDeployer.sol:L22](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L22)
[AlgebraPoolDeployer.sol:L27](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L27)
[AlgebraPoolDeployer.sol:L37](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L37)
[AlgebraPoolDeployer.sol:L38](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L38)
[TokenDeltaMath.sol:L30](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TokenDeltaMath.sol#L30)
[TokenDeltaMath.sol:L51](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TokenDeltaMath.sol#L51)
[DataStorage.sol:L369](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L369)
[PriceMovementMath.sol:L52](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L52)
[PriceMovementMath.sol:L53](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L53)
[PriceMovementMath.sol:L70](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L70)
[PriceMovementMath.sol:L71](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L71)
[PriceMovementMath.sol:L87](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L87)
### Instances 
```
src/core/contracts/AlgebraPool.sol:55:    require(msg.sender == IAlgebraFactory(factory).owner());
src/core/contracts/AlgebraPool.sol:122:      require(_lower.initialized);
src/core/contracts/AlgebraPool.sol:134:      require(_upper.initialized);
src/core/contracts/AlgebraPool.sol:229:          require((_blockTimestamp() - lastLiquidityAddTimestamp) >= _liquidityCooldown);
src/core/contracts/AlgebraPool.sol:953:    require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));
src/core/contracts/AlgebraPool.sol:960:    require(msg.sender == IAlgebraFactory(factory).farmingAddress());
src/core/contracts/AlgebraPool.sol:968:    require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown
src/core/contracts/DataStorageOperator.sol:43:    require(msg.sender == factory || msg.sender == IAlgebraFactory(factory).owner());
src/core/contracts/AlgebraFactory.sol:43:    require(msg.sender == owner);
src/core/contracts/AlgebraFactory.sol:60:    require(tokenA != tokenB);
src/core/contracts/AlgebraFactory.sol:62:    require(token0 != address(0));
src/core/contracts/AlgebraFactory.sol:63:    require(poolByPair[token0][token1] == address(0));
src/core/contracts/AlgebraFactory.sol:78:    require(owner != _owner);
src/core/contracts/AlgebraFactory.sol:85:    require(farmingAddress != _farmingAddress);
src/core/contracts/AlgebraFactory.sol:92:    require(vaultAddress != _vaultAddress);
src/core/contracts/AlgebraPoolDeployer.sol:22:    require(msg.sender == factory);
src/core/contracts/AlgebraPoolDeployer.sol:27:    require(msg.sender == owner);
src/core/contracts/AlgebraPoolDeployer.sol:37:    require(_factory != address(0));
src/core/contracts/AlgebraPoolDeployer.sol:38:    require(factory == address(0));
src/core/contracts/libraries/TokenDeltaMath.sol:30:    require(priceDelta < priceUpper); // forbids underflow and 0 priceLower
src/core/contracts/libraries/TokenDeltaMath.sol:51:    require(priceUpper >= priceLower);
src/core/contracts/libraries/DataStorage.sol:369:    require(!self[0].initialized);
src/core/contracts/libraries/PriceMovementMath.sol:52:    require(price > 0);
src/core/contracts/libraries/PriceMovementMath.sol:53:    require(liquidity > 0);
src/core/contracts/libraries/PriceMovementMath.sol:70:        require((product = amount * price) / amount == price); // if the product overflows, we know the denominator underflows
src/core/contracts/libraries/PriceMovementMath.sol:71:        require(liquidityShifted > product); // in addition, we must check that the denominator does not underflow
src/core/contracts/libraries/PriceMovementMath.sol:87:        require(price > quotient);
```
----
# 3. Use of Block.timestamp
Impact - Non-Critical
### Description
Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.
### Links to github files
[PoolState.sol:L50](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolState.sol#L50)
### Instances 
```
src/core/contracts/base/PoolState.sol:50:    return uint32(block.timestamp); // truncation is desired
```
### Recommended Mitigation Steps

Block timestamps should not be used for entropy or generating random numbers—i.e., they should not be the deciding factor (either directly or through some derivation) for winning a game or changing an important state.

Time-sensitive logic is sometimes required; e.g., for unlocking contracts (time-locking), completing an ICO after a few weeks, or enforcing expiry dates. It is sometimes recommended to use block.number and an average block time to estimate times; with a 10 second block time, 1 week equates to approximately, 60480 blocks. Thus, specifying a block number at which to change a contract state can be more secure, as miners are unable to easily manipulate the block number.

----
# 4. Variable names that consist of all capital letters should be reserved for const/immutable variables
### Description
If the variable needs to be different based on which class it comes from, a `view`/`pure` function should be used instead
### Links to github files
[AlgebraFactory.sol:L20](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L20)
[PoolImmutables.sol:L10](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolImmutables.sol#L10)
[PoolImmutables.sol:L13](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolImmutables.sol#L13)
[PoolImmutables.sol:L15](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolImmutables.sol#L15)
[PoolImmutables.sol:L17](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolImmutables.sol#L17)
[DataStorageOperator.sol:L23](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L23)
[DataStorageOperator.sol:L24](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L24)

### Instances 
```
src/core/contracts/AlgebraFactory.sol:20:  address public immutable override poolDeployer;
src/core/contracts/base/PoolImmutables.sol:10:  address public immutable override dataStorageOperator;
src/core/contracts/base/PoolImmutables.sol:13:  address public immutable override factory;
src/core/contracts/base/PoolImmutables.sol:15:  address public immutable override token0;
src/core/contracts/base/PoolImmutables.sol:17:  address public immutable override token1;
src/core/contracts/DataStorageOperator.sol:23:  address private immutable pool;
src/core/contracts/DataStorageOperator.sol:24:  address private immutable factory;
```
----
