### Use a more recent version of Solidity > 0.8.0

Files Found: All files refer to pragma solidity =0.7.6;

Explanation: The following repo bares resemblance to [UniswapV3 Core](https://github.com/Uniswap/v3-core/blob/main/contracts/UniswapV3Factory.sol). Which also uses =0.7.6.

### Mitigation:

- Use a solidity version of at least 0.8.0 to get overflow protection without SafeMath 
- Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining
- Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads 
- Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings 
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value
 --- 
 ## `Abi.Encode()` is less efficient than `abi.Encodepacked()` 

 ### Files Found: 
 There are 2 instances of this issue. 
 - File: src/core/contracts/AlgebraFactory.sol - line 123 
- File: src/core/contracts/AlgebraPoolDeployer.sol - line 51 
 
 
 ### Mitigation: 
 Use Abi.encode packed where necessary 
 --- 

 --- 
 ## `x += y` costs more gas than `x = x + y` for state variables 
 ### Files Found: 
 There are 7 instances of this issue. 
 - File: src/core/contracts/AlgebraPool.sol - line 257 
 - File: src/core/contracts/AlgebraPool.sol - line 258 
 - File: src/core/contracts/AlgebraPool.sol - line 804 
 - File: src/core/contracts/AlgebraPool.sol - line 811 
 - File: src/core/contracts/AlgebraPool.sol - line 814 
 - File: src/core/contracts/AlgebraPool.sol - line 931 
 - File: src/core/contracts/AlgebraPool.sol - line 945 
 
 
 ### Mitigation: 
 Use `x = x + y` instead of `x += y` 
 --- 
