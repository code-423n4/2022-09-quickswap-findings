- Use a more recent version of Solidity.
	- Use a solidity version of at least 0.8.0 to get overflow protection without SafeMath; Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining; Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads; Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than `revert()/require()` strings; Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value.
	- Entire scope.

- It costs more gas to initialize NON-CONSTANT/NON-IMMUTABLE variables to zero than to let the default of zero be applied.
	- Instance: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307

- `++i` costs less gas than `i++`, especially when in FOR loops.
	- Analysis: ++i costs less gas compared to `i++` or `i += 1` for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration). This statement is true even with the optimizer enabled. `i++` increments i and returns the initial value of i. Which means: uint i = 1; i++; // == 1 but i == 2 But ++i returns the actual incremented value: uint i = 1; ++i; // == 2 and i == 2 too, so no need for a temporary variable In the first case, the compiler has to create a temporary variable (when used) for returning 1 instead of 2.
	- Instance: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307

- Using unchecked blocks to save gas - increments in for loop can be unchecked (SAVE 30-40 GAS PER LOOP ITERATION).
	- The majority of Solidity for loops increment a uint256 variable that starts at 0. These increment operations never need to be checked for over/underflow because the variable will never reach the max number of uint256 (will run out of gas long before that happens). The default over/underflow check wastes gas in every iteration of virtually every for loop .
	- Instance: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307

- <ARRAY>.length should not be looked up in every loop of a for-loop.
	- Instance: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307


- `ABI.ENCODE()` is less efficient than `ABI.ENCODEPACKED()`.
	- Instance: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L123


- Usage of `UINTS/INTS` smaller than 32 bytes incurs overhead.
	- Instance: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L12


- Using `PRIVATE` rather than `PUBLIC` for `CONSTANTS`, saves gas.
	- Instance: https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L13

- Splitting `REQUIRE()` statements that use `&&` saves gas.
	- Instance:
		- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L110
		- https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L46