### Low Severity Findings:


The contracts should be deployed using the most up-to-date, recent version of Solidity. Consider switching to a 0.8.x version, preferably a stable one (e.g. 0.8.4 - 0.8.7).

**Using =0.7.6:**
* src/core/contracts/AlgebraFactory.sol
* src/core/contracts/AlgebraPool.sol
* src/core/contracts/AlgebraPoolDeployer.sol
* src/core/contracts/DataStorageOperator.sol
* src/core/contracts/libraries/AdaptiveFee.sol
* src/core/contracts/libraries/Constants.sol
* src/core/contracts/libraries/DataStorage.sol
* src/core/contracts/libraries/PriceMovementMath.sol
* src/core/contracts/libraries/TickManager.sol
* src/core/contracts/libraries/TickTable.sol
* src/core/contracts/libraries/TokenDeltaMath.sol
* src/core/contracts/base/PoolImmutables.sol
* src/core/contracts/base/PoolState.sol


In **src/core/contracts/AlgebraFactory.sol**, the *setOwner()*, *setFarmingAddress()*, and *setVaultAddress()* methods should have checks to ensure the address is not accidentally set to the 0-address.

Original:

    /// @inheritdoc IAlgebraFactory
    function setOwner(address _owner) external override onlyOwner {
      require(owner != _owner); // should check to ensure it isn't 0 address
      emit Owner(_owner);
      owner = _owner;
    }
 
    /// @inheritdoc IAlgebraFactory
    function setFarmingAddress(address _farmingAddress) external override onlyOwner {
      require(farmingAddress != _farmingAddress); // should check to ensure it isn't 0 address
      emit FarmingAddress(_farmingAddress);
      farmingAddress = _farmingAddress;
    }
 
    /// @inheritdoc IAlgebraFactory
    function setVaultAddress(address _vaultAddress) external override onlyOwner {
      require(vaultAddress != _vaultAddress); // should check to ensure it isn't 0 address
      emit VaultAddress(_vaultAddress);
      vaultAddress = _vaultAddress;
    }

Fix:

    /// @inheritdoc IAlgebraFactory
    function setOwner(address _owner) external override onlyOwner {
      require(owner != _owner);
      require(_owner != address(0));
      emit Owner(_owner);
      owner = _owner;
    }
 
    /// @inheritdoc IAlgebraFactory
    function setFarmingAddress(address _farmingAddress) external override onlyOwner {
      require(farmingAddress != _farmingAddress);
      require(_farmingAddress != address(0));
      emit FarmingAddress(_farmingAddress);
      farmingAddress = _farmingAddress;
    }
 
    /// @inheritdoc IAlgebraFactory
    function setVaultAddress(address _vaultAddress) external override onlyOwner {
      require(vaultAddress != _vaultAddress);
      require(_vaultAddress != address(0));
      emit VaultAddress(_vaultAddress);
      vaultAddress = _vaultAddress;
    }




---

### Non-Critical Findings:

Use scientific notation rather than exponentiation for big numbers. Places where this could be implemented:

* src/core/contracts/DataStorageOperator.sol (line 139)
* src/core/contracts/libraries/AdaptiveFee.sol (lines 53 and 60)
* src/core/contracts/libraries/DataStorage.sol (line 53)

