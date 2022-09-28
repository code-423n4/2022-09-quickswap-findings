1.	Use > instead of !=0 for uint types to save gas
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L501
2.	Use separate require statements instead require statement with few && conditions.
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L46
