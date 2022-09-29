## low risks
### L01: the assert() function when false, uses up all the remaining gas and reverts all the changes made.
#### problem
assert(false) compiles to 0xfe, which is an invalid opcode, using up all remaining gas, and reverting all changes.
require(false) compiles to 0xfd which is the REVERT opcode, meaning it will refund the remaining gas. The opcode can also return a value (useful for debugging), but I don't believe that is supported in Solidity as of this moment. (2017-11-21)
#### prof
libraries/DataStorage.sol, 190, b'    assert(false);'

### L02: USE A MORE RECENT VERSION OF SOLIDITY
libraries/TokenDeltaMath.sol, 2, b'pragma solidity =0.7.6;'
AlgebraPoolDeployer.sol, 2, b'pragma solidity =0.7.6;'
libraries/TickManager.sol, 2, b'pragma solidity =0.7.6;'
base/PoolImmutables.sol, 2, b'pragma solidity =0.7.6;'
DataStorageOperator.sol, 2, b'pragma solidity =0.7.6;'
AlgebraPool.sol, 2, b'pragma solidity =0.7.6;'
libraries/TickTable.sol, 2, b'pragma solidity =0.7.6;'
base/PoolState.sol, 2, b'pragma solidity =0.7.6;'
AlgebraFactory.sol, 2, b'pragma solidity =0.7.6;'
libraries/AdaptiveFee.sol, 2, b'pragma solidity =0.7.6;'
libraries/Constants.sol, 2, b'pragma solidity =0.7.6;'
libraries/PriceMovementMath.sol, 2, b'pragma solidity =0.7.6;'
libraries/DataStorage.sol, 2, b'pragma solidity =0.7.6;'

### L03: address variable should check if it is zero MISSING CHECKS FOR ADDRESS(0X0) WHEN ASSIGNING VALUES TO ADDRESS STATE VARIABLES
#### prof
AlgebraPoolDeployer.sol, 32, b'    owner = msg.sender;'
AlgebraPoolDeployer.sol, 40, b'    factory = _factory;'
DataStorageOperator.sol, 32, b'    factory = msg.sender;'
DataStorageOperator.sol, 33, b'    pool = _pool;'
AlgebraFactory.sol, 51, b'    owner = msg.sender;'
AlgebraFactory.sol, 54, b'    poolDeployer = _poolDeployer;'
AlgebraFactory.sol, 55, b'    vaultAddress = _vaultAddress;'
AlgebraFactory.sol, 80, b'    owner = _owner;'
AlgebraFactory.sol, 87, b'    farmingAddress = _farmingAddress;'
AlgebraFactory.sol, 94, b'    vaultAddress = _vaultAddress;'