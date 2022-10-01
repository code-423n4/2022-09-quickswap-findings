Issue 1.
!= 0 is a cheaper operation compared to > 0, when dealing with uint...!= 0 will consume less gas as compared to >..

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L59-L62
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L604
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L610
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L434



Issue 2.
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L718
uint32 blockTimestamp converting it into 'uint256' will save you a gas!!!
The EVM only operates on 32 bytes/ 256 bits at a time. 
This means that if you use uint8, EVM has to first convert it uint256 to work on it and the conversion costs extra gas! 
You may wonder, What were the devs thinking? Why did they create smaller variables then? The answer lies in packing. 
In solidity, you can pack multiple small variables into one slot, but if you are defining a lone variable and can’t pack it, 
it’s optimal to use a uint256 rather than uint8.
