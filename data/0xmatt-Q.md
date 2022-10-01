## QA Report (low / non-critical)

### L-01: Missing Zero-Address Checks

Zero-address checks are a common best practice for input validation of critical address parameters, particularly in constructors and setters. Accidental use of zero-addresses may result in unexpected exceptions, failures or require the redeployment of contracts.

The project already uses zero-address checks in [some places](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L35-L41).

If it is necessary to be able to set an address to 0 consider implementing a two-step or renounce function if the address is to be subject to access control.

The following instances were identified where zero-address checks should be added.

In AlgebraFactory.sol:

* [Constructor](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L50-L56) - \_poolDeployer, \_vaultAddress
* [createPool()](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L58-L74) - tokenA, tokenB, token1 (tokenA and tokenB if require is moved before assignment, otherwise token0 and token1 require the zero check)
* [setOwner()](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L76-L81) - \_owner
* [setFarmingAddress()](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L83-L88) - \_vaultAddress

In AlgebraPool.sol:

* [mint()](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L415-L485) - Neither sender nor recipient are checked for zero values. Sender is used in a TransferHelper.safeTransfer() call. Recipient is passed to [\_updatePositionTicksAndFees()](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L274-L364).
* [collect()](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L487-L510) - recipient
* [swap()](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L588-L623) - recipient
* [setIncentive()](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L958-L964) - virtualPoolAddress

### L-02: ## Single-Step transfer patterns in use

Several address parameters in [AlgebraFactory](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L76-L81) are changeable in a single step. Although these functions are locked to the owner, in the event that the owner sets an incorrect or null value, some of these settings could have a significant impact on the contract's function, such as permanently losing ownership. This could require a redeployment to address, which could be disruptive.

Consider a two-step nominate and claim process, where the change is nominated by the owner, but must be claimed by the process recipient to succeed. This ensures the updated address is valid.

Instances found:

* [AlgebraFactory#setOwner()](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L76-L81) - Changes ownership
* [AlgebraFactory#setFarmingAddress()](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L83-L88) - Changes farming contract address, which could lead to failed farming functionality.
* [AlgebraFactory#setVaultAddress](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L90-L95) - Changes vault address, which could lead to stuck tokens

In other files:

* [AlgebraPoolDeployer#setFactory()](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L35-L41) - Changes Factory ownership, which is address(0) after construction. Can only be set when zero. Setting factory via parameter at construction may be more efficient.