# UPGRADE PRAGMA TO AT LEAST 0.8.4

Using newer compiler versions and the optimizer give gas optimizations. Also, additional safety checks are available for free.

The advantages here are:

- Low level inliner (>= 0.8.2): Cheaper runtime gas (especially relevant when the contract has small functions).
- Optimizer improvements in packed structs (>= 0.8.3)
- Custom errors (>= 0.8.4): cheaper deployment cost and runtime cost. Note: the runtime cost is only relevant when the revert condition is met. In short, replace revert strings by custom errors.

Most, if not All contract in review are using `pragma solidity =0.7.6;` Consider upgrading pragma to at least 0.8.4.

# NO NEED TO EXPLICITLY INITIALIZE VARIABLES WITH DEFAULT VALUES

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: `for (uint256 i = 0; i < secondsAgos.length; i++) ` (DataStorage.sol#L307)
