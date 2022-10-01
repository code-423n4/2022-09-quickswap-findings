# [L-01] Misleading comment
## Targets
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L49
    The comment says there's a multiplication by `dt`, however it's not there.

# [L-02] Missing modifier
## Targets
- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L494
    Missing `onlyValidTicks` modifier. The function takes `bottomTick` and `topTick` in arguments.