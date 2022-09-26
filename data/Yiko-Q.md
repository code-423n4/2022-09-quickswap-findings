https://github.com/code-423n4/2022-09-quickswap/blob/59c93c65443f9dec68473ca82740abfd8dd47ac3/src/core/contracts/AlgebraFactory.sol#L91
In AlgebraFactory.sol, in the function at line 92
When setting vault address, address input is not checked if it is address(0) or not.

https://github.com/code-423n4/2022-09-quickswap/blob/59c93c65443f9dec68473ca82740abfd8dd47ac3/src/core/contracts/AlgebraPool.sol#L634
In AlgebraPool.sol, at line 634
Instead of "then", there should be "than".