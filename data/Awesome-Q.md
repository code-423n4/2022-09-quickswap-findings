## Two-Step Transfer Of Ownership
`setOwner()` should be enforced handing ownership to another address in a pending state. And also, a `claimOwner()` should be put in place for the brand new owner to claim ownership. This will make sure the latest owner is aware of the ownership assigned transferred.

[Line 77-81](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L77-L81)

Where it is doable, include a zero address check for `setOwner()` to steer clear of a non-functional call on `claimOwner()`, as well as having the event emit both the new and old owners rather than simply the new owner