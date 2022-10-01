
1. Missing zero address check
The following functions are missing a zero address check;
**Occurrences**

- AlgebraFactory.constructor() -_poolDeployer, _vaultAddress arguments
- DataStorageOperator.constructor() - _pool argument
- PoolImmutables.constructor() - deployer argument
- AlgebraFactory.createPool()
- AlgebraFactory.setOwner() - _owner argument
- AlgebraFactory.setFarmingAddress() - _farmingAddress argument
- AlgebraFactory.setVaultAddress() - _vaultAddress argument
- AlgebraPool.setIncentive() - virtualPoolAddress argument


2. The Pure function should be a view function.
**Occurences**

- DataStorageOperator.window() : This function reads a state from the contract, hence it should have view mutability.

- DataStorageOperator.calculateVolumePerLiquidity() : This function reads `MAX_VOLUME_PER_LIQUIDITY` state from the DataStorageOperator contract, hence it should have view mutability.

- PoolImmutables.tickSpacing() reads state from the contract, hence it should have view mutability

- PoolImmutables.maxLiquidityPerTick() reads state from the contract, hence it should have view mutability



3. Missing zero value check
The following do not check for a zero value
- AlgebraPool.setLiquidityCooldown() - newLiquidityCooldown argument
- AlgebraPool.setCommunityFee - communityFee0 and communityFee1 arguments

