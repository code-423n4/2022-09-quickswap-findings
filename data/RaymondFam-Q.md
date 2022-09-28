## Two-step Transfer of Ownership
`setOwner()` should be implemented giving ownership to another address in a pending state. And then, a `claimOwner()` should be introduced for the new owner to claim ownership. This will ensure the new owner is going to be fully aware of the ownership assigned/transferred.

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L77-L81

Where possible, include a zero address check for `setOwner()` to avoid a non-functional call on `claimOwner()`, as well as having the event emit both the new and old owners instead of just the new owner.

## Solidity Version
Consider using the latest released version of Solidity. Apart from exceptional cases, only the latest version receives security fixes. Furthermore, breaking changes as well as new features are introduced regularly.

## Require Error Message
Consider making the require error message more verbose so all other peer developers would better be able to comprehend the intended statement logic. But, do limit the message length to less than 32 character where possible as strings that are more than 32 characters will require more than 1 storage slot, costing more gas.