#### Balance checks can be optimized to avoid redundant `extcodesize` check

##### Severity: Gas optimization

##### Context:

- https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L70:#L76

##### Description:

High-level `balanceOf` methods to check token balances make use of a redundant `extcodesize` check and can be optimized as, e.g.:

```
(bool success, bytes memory data) =
    token0.staticcall(abi.encodeWithSelector(IERC20Minimal.balanceOf.selector, address(this)));
require(success && data.length >= 32);
return abi.decode(data, (uint256));
```

#### The factory contract can inherit the deployer contract to remove unecessary logic

##### Severity: Gas optimization

##### Context:

- https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L49
- https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L21:L24

##### Description:

The `AlgebraPoolFactory` contract is intended to be the only allowed executor of `AlgebraPoolDeployer.deploy`, because of this, checks are included to ensure that the sender is the factory contract. Redundant logic can be removed by having the factory contract inherit the deployer contract and making `AlgebraPoolDeployer.deploy` internal.

#### Parameters can be safely deleted after pool deployment

##### Severity: Gas optimization

##### Context:

- https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L44:L53

##### Description:

`AlgebraPoolFactory.deploy` shares the parameters with the `AlgebraPool` instance. However, since these parameters are only used in the pool contract's constructor, they can be deleted once the pool is created, reducing gas consumption.
