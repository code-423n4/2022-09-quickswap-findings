# Improper Solidity version

according to common best practice you should use solidity 0.7.0 while in AlgebraFactory and other contracts 0.7.6 has been used , unless there is something special about this particular version it is better follow common best practices .

# Improper approach

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L61

This code assigns ’token0’ and ’token1’ even when tokenA and tokenB are already in the proper order.
instead you can use this : if (tokenA > token B) (tokenA, tokenB) = (tokenB, tokenA);
