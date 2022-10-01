## Using unchecked blocks to save gas

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers, when it is not possible
for the arithmetic operation to underflow or overflow. The operation should be unchecked to save gas.

1.Consider unchecking the following arithmetic operations:

- [`Line-L141-L143`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L141-L143)
- [`Line-L157-L159`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L157-L159)
- [`Line-L164-L166`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L164-L166)
- [`Line-L229`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L229)
- [`Line-L247`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L247)
- [`Line-252`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L252)
- [`Line-L455-L455`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L454-L455)
- [`Line-L478-L482`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L478-L482)
- [`Line-L502-L503`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L502-L503)
- [`Line-L801`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L801)
- [`Line-L810-L811`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L810-L811)
- [`Line-L859`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L859)
- [`Line-L873-L874`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L873-L874)
- [`Line-L922`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L922)
- [`Line-L931`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L931)
- [`Line-L936`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L936)
- [`Line-L945`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L945)
- [`Line-L257-L258`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L257-L258)
- [`Line-L804-L805`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L804-L805)
- [`Line-L109`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L109)
- [`Line-L45`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L45)

## Using "x += y" cost more gas than "x = x + y"

1.Consider using this method only for the following state variables

- [`Line-L811`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L811)
- [`Line-L810`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L810)
- [`Line-L922`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L922)
- [`Line-L936`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L936)

## Cache storage values in memory to minimize the gas cost

The code can be optimized by minimizing the number of SLOADs, since SLOADs are more expensive than MLOADs.

1.Consider caching `MAX_VOLUME_PER_LIQUIDITY` in the following function

- [`Line-L140`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L140)

## Constants can be set as private to save gas

1.Consider declaring the constans `UINT16_MODULO` and `MAX_VOLUME_PER_LIQUIDITY` as private:

- [`Line-L15-L16`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L15-L16)

## Splitting require() statements that use && saves gas

Instead of using the && operator in a single require statement to check multiple conditions,using multiple require statements with 1 condition 
per require statement will save 8 GAS per &&.

- [`Line-L110`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L110)
- [`Line-L953`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L953)
- [`Line-L968`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L968)
- [`Line-L46`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L46)

## Copying state struct in memory is wrong and wastes gas

Using storage pointer instead of memory location can save a decent amount of gas.

1.One function in the contract AlgebraPool, use a memory location to retrieve the struct Cumulatives
- [`Line-L114`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L114)
- [`Line-125`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L125)

2.One function in the contract AlgebraPool, use a memory location to retrieve the struct UpdatePositionCache
- [`Line-L287`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L287)

3.One function in the contract AlgebraPool, use a memory location to retrieve the struct SwapCalculationCache
- [`Line-L719`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L719)

4.One function in the contract AlgebraPool, use a memory location to retrieve the struct PriceMovementCache
- [`Line-L779`](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L779)
