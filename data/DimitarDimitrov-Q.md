# 1. Emitting of emit should happed of the bottom of the method

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L52
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L79
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L86
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L93
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L621
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L39

# 2. No correct position of contract parts

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L116 -> this should be at the top
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L48 -> mapping before modifiers
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L96-L100
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L262-L266
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L675-L700

# 3. No correct errors message or missing error message
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L43
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L60-L63
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L78
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L85
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L194
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L224
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L474-L475
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L22-L38

# 4. Usage hardcoded address
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L116 
