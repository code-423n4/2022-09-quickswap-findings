
**`lteConsideringOverflow` may return incorrect value as per specification**
In DataStorage.sol the function [`lteConsideringOverflow`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L94-L101) is supposed to return whether `a <= b`, but if we have indeed `a <= b` the function will return `false` if `a <= currentTime < b`, and if we have `b < a` the function will return `true` if `b <= currentTime < a`.

&nbsp;

**`setOwner` should be a two-step process**
`setOwner` in [AlgebraFactory.sol#L77-L81](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L77-L81) immediately transfers ownership, which is critical, to the new address. If any mistake is made this cannot be reverted.
Consider implementing a two-step process where the new owner is nominated, and then has to accept ownership. Thus the validity of the new address is ensured.

&nbsp;

**Incorrect comment**
At [DataStorage.sol#L393](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L393) `an timepoint` should be `a timepoint`.
