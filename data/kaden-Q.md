#### Earned interest to tokens in pool can be stolen

##### Severity: Low

##### Context:

- https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L603:615

##### Description:

In `AlgebraPool.swap`, the token balance of the contract is called before and after the `_swapCallback` is executed to enforce the user to transfer an appropriate amount of tokens to the contract. It's possible that in the case of a non-standard ERC-20, an attacker may call a method on the token to accrue interest to the pool contract, such that the balance increases sufficiently for them to execute their swap without actually transferring any tokens.

##### Remediation:

It is recommended that the possible effects of non-standard ERC-20 tokens are well documented to minimize loss of user funds.