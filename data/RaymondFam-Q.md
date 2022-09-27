## Two-step Transfer of Ownership
`setOwner()` should be implemented giving ownership to another address in a pending state. And then, a `claimOwner()` should be introduced for the new owner to claim ownership. This will ensure the new owner is going to be fully aware of the ownership assigned/transferred.

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L77-L81

Where possible, include a zero address check for `setOwner()` to avoid a non-functional call on `claimOwner()`, as well as having the event emit both the new and old owners instead of just the new owner.