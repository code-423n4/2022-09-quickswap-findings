[G-01] ++I COSTS LESS GAS COMPARED TO I++ OR I += 1 IN FOR LOOPS (~5 GAS PER ITERATION)

Pre-increments are cheaper than post-increments.

For a uint256 i variable, pre-increment ++i costs 5 gas  less than i++ with the optimizer enabled.

Note that post-increments (or post-decrements) return the old value before incrementing or decrementing, hence the name post-increment:
However, pre-increments (or pre-decrements) return the new value.

Instance:
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307