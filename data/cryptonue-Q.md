# MISSING ZERO-ADDRESS CHECK IN THE SETTER FUNCTIONS AND INITIALIZERS

Missing checks for zero-addresses may lead to infunctional protocol, if the variable addresses are updated incorrectly.

## Proof of concept

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L77-L81
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L84-L88
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L91-L95
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L416-L485

## Recommended Mitigation
Consider adding zero-address checks 