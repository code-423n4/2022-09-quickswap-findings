#1 missing zero address check on token0 and token1

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L122

Deterministically computes the pool address given the factory and PoolKey. in computeAddress() there are token0 and token1 address. to avoid non exsistance address we suggest to add simple check for token0 and token1 address.
example

    require(token0 != address(0), "invalid address");
    require(token1 != address(0), "invalid address");

#2 Missing revert message

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L62-L63

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L52-L53

require statement if in false condition will revert error message. code in above use require statement but missing revert message, we suggest to add message torequire statement to incrase creadibility users.

#3 Missing natspec comment

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L45

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L93-L122

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickTable.sol#L121

the function has a natspec comment  to explain utility about function or parameter but code above missing it. we recommend to add natspec comment to increase readability