Throughout the Algebra contracts, revert statements return with a `string` reason; which can be optimized to use much less gas over the course of the project using custom errors. For example, on [Line 60](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L60) of `AlgebraPool.sol`, the following line seen below reverts with the string `TUM`.
```
require(topTick < TickMath.MAX_TICK + 1, 'TUM');
```
This can be optimized by throwing a custom error which uses three less op-codes to do the same thing. The optimized equivalent can be seen below.
```
err TUM();
...
if(topTick < TickMath.MAX_TICK + 1) revert TUM();
```