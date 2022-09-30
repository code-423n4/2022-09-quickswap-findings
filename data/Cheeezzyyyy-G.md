## USE MULTIPLE REQUIRE STATEMENTS FOR MULTIPLE CONDITIONS RATHER THAN A SINGLE REQUIRE STATEMENT TO SAVE GAS

Require statements including conditions with the && operator can be broken down in multiple require statements to save gas.

Instance Found:
- [AlgebraFactory.sol#L110](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L110)
- [AlgebraPool.sol#L739](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L739)
- [AlgebraPool.sol#L953](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L953)
- [AlgebraPool.sol#L968](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L968)

## USE REVERT INSTEAD OF REQUIRE STATEMENTS TO SAVE ON GAS

Instances Found i.e. :

- [AlgebraPool.sol#L60-L62](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L60-L62)