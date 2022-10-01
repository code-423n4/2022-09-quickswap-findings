# QA (LOW & NON-CRITICAL)

## [L-01] Risk of re-init
AlgebraPool.sol's initialize() function is implemented to set a fresh pair's initial price and ticks. 
It checks the `globalState.price == 0` to set the values. However, if somehow, the globalState.price drops to zero after the swap function, an orchestrated actor can re-initialize the pool with a higher price to inflate the underlying token price.


## [L-02] Dependency on blockTimestamp
DataStorage.sol's `write()` function checks the timestamp as  `if (_last.blockTimestamp == blockTimestamp)` and returns with `index`
However, if the chain is forked and there are reorgs, this check can result in wrong indexing. There might a manual implementation of setting indexes in this rare case and it would be best if the contract would have the features of pausable.sol to save the day.

## [L-03] Rugging risk
The owner address is having too much authority to set many things at the codebase. A divided role model assignment would serve better for the codebase in terms of risk management. E.g. ;
The owner calls `setVaultAddress` of his EOA and receives the assets. 
The owner calls `setFactory` in AlgebraPoolDeployer and set the new factory address as his contract address and diverts the user assets into his own managed pools.


## [L-04] Critical address / parameter changes
Critical address / value changes are not done in two steps for the following methods:
AlgebraFactory.sol's `setOwner()` function.


## [N-01] Scoped contracts are missing proper NatSpec comments
The scoped contracts are missing proper NatSpec comments such as @notice @dev @param on many places.It is recommended that Solidity contracts are **fully** annotated using NatSpec for all public interfaces (**everything in the ABI**) [Reference](https://docs.soliditylang.org/en/latest/style-guide.html#natspec) 

## [N-02] Different and outdated compiler versions 
The scoped contracts have different solidity compiler ranges referenced. This leads to potential security flaws between deployed contracts depending on the compiler version chosen for any particular file. It also greatly increases the cost of maintenance as different compiler versions have different semantics and behavior. Current Solidity version available is 0.8.17

## [N-03] Shadowed variable
AlgebraPool.sol's `_writeTimepoint()` function's `liquidity`, `volumePerLiquidityInBlock` variables are shadowed with the global ones.