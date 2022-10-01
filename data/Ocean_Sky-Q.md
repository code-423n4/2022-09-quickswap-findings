N-01 Missing comments on some functions, modifiers and variables.

Instances:
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L59
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L66
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L70
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L74
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L96
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L546
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L551
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L561
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L580
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L677
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L684


Mitigation:
Please add comments to explain certain lines of codes such as functions, variables and modifiers.

N-02 Constants should be defined rather than using magic numbers.

Instances
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L52-L53
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L59-L60
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L88
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L92
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L96
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L100
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L104
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L107
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L51-L53
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L80

Mitigation:
Define constant variables for the literal values aforementioned.

 N-03 Require() statements should have descriptive reason strings

Instances:
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L60
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L61
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L62
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L194
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L224
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L434
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L454-L455
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L469
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L474-L475
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L608
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L614
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L636
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L641
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L645
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L731
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L733
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L898
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L921
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L935

