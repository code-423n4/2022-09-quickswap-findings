# Low 

### Administrative function lacks `address(0)` check for changing sensitive address.

Location: [1](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L77)

#### Description 

`AlgebraFactory.setOwner()` function lacks a 0 address check. If an oversight or human error occurs during protocol management, the ownership of the contract would be permanently affected.

#### Suggested course of action

Add a `require` statement to check for the 0 address.

### `AlgebraPool.initialize()` can be called by anyone

Location: [1](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L193)

#### Description

`AlgebraPool.initialize()` configures state values for the contract, including the initial price. This function is not called during deployment of the Pool. It will normally be called using deployment scripts run by the protocol.

Unless the deployment of every pool is conducted via private RPC, it seems plausible that the call to initialize could be front-run. While the effects would be only transient, it would be better to mitigate the issue outright.

#### Suggested course of action

Apply the existing `onlyFactoryOwner` modifier to the function. The owner of the factory contract, whether EOA or multisig, could then make the call with whatever appropriate timing or means, without requiring a private RPC for the whole sequence.


# Non-critical

### Constructor lacks `address(0)` checks for setting sensitive addresses.

Location: [1](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L50)

#### Description 

`AlgebraFactory`'s constructor lacks an `address(0)` check for the `_poolDeployer` address. While the other address argument could be fixed with an additional transaction, this address could not.

#### Suggested course of action

Add a `require` statement to check for the 0 address.

### Consider renaming `setIncentive`

Location: [1](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L959)

#### Description

The interface documentation for `setIncentive()` indicates that it is setting "The address of a virtual pool associated with the incentive." The code does this. However, the name of the function implies that it is directly setting an 'incentive' value, and not the address of a contract.

#### Suggested course of action

Consider renaming the function to `setIncentivePool` or `setIncentivePoolAddress`.

### `AlgebraPool.getInnerCumulatives` contains fragile branching code that may cause errors in future edits

Location: [1](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L103)

#### Description

`getInnerCumulatives()` contains branching code based on the comparison of the `currentTick` value with the `bottomTick` and `topTick` values.

The third case where `currentTick` is greater than `topTick` is implicit in this version of the code; the final `return` is only encountered if the other two tests are false.

While there is nothing wrong with the execution of this code per se, it is fragile. With multiple `return` statements and lack of explicit `if {} else if {} else {}` structure, future developers might cause control flow issues if changes to this function are made. 

Further, the function contains named output values. Generally, the form when using named return values is to assign them as needed and then allow execution to leave the scope, rather than using an explicit `return`.

#### Suggested course of action

Modify the function to use explicit `if {} else if {} else {}`. 

Consider either 
a) removing the named return values and using `return` statements
b) keeping the named values but removing the explicit returns, or 
c) condensing the multiple `return`s into one `return`.

For example, `AlgebraPool._getAmountsForLiquidity` implements this similar set of checks in a way that's robust and idiomatic.

### Using named return values without assignment

Locations:
[1](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L99)
[2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L116)
[3](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L127)
[4](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L135)
[5](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L155)
[6](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L213)
[7](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L332) <- generates compiler warning if addressed as suggested

#### Description

Solidity allows for naming return values from functions. This can ease code comprehension, and cuts down on explicit `return` statements. 

However, the indicated lines combine named return values with explicit `return` statements. This makes function signatures needlessly verbose, as the `returns()` can be shorted considerably in length.

For example, the proper style is seen in `DataStorageOperator.getSingleTimepoint()`.

#### Suggested course of action

Remove unneeded named return values if they aren't necessary.