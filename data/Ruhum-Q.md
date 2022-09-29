# Report

- [Low](#low)
  - [L-01: use two-step process for criticial address changes](#l-01-use-two-step-process-for-criticial-address-changes)
  - [L-02: pool isn't initialized after creation](#l-02-pool-isnt-initialized-after-creation)

# Low

## L-01: use two-step process for criticial address changes

Consider using a two-step process for transferring the ownership of a contract. While it costs a little more gas, it's safer than transferring directly.

Here's an example from the Compound Timelock contract: https://github.com/compound-finance/compound-protocol/blob/master/contracts/Timelock.sol#L45-L58

- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L77

## L-02: pool isn't initialized after creation

A pool has a `initialize()` function where the initial price is set and the pool is unlocked. Inside the simulation and deployment scripts, the function is never called. Uniswap uses a custom contract, PoolInitializer, to deploy pools where the initialize function is called right after deployment.

- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L193
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L69
