# Low

1. `mint` function should use safemath

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L454-L455

Linked lines should use safeMath sub to prevent any chance of an underflow. Swap functions use safemath already, using it here as well is also recommended.