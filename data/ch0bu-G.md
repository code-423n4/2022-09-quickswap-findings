## 1. `++i` costs less gas compared to `i++` or `i += 1`, same for `--i/i--`. Especially in for loops

`++i` costs less gas compared to `i++` or `i += 1` for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration). This statement is true even with the optimizer enabled.

`i++` increments i and returns the initial value of `i`.

```
uint i = 1;  
i++; // == 1 but i == 2
```
But ++i returns the actual incremented value:
```
uint i = 1;  
++i; // == 2 and i == 2 too, so no need for a temporary variable  
```
In the first case, the compiler has to create a temporary variable (when used) for returning 1 instead of 2

I suggest using ++i instead of i++ to increment the value of an uint variable.

If done inside for loop, saves 6 gas per loop.

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol
```
307	for (uint256 i = 0; i < secondsAgos.length; i++) {
```

## 2. Use a more recent version of solidity

- Use a solidity version of at least 0.8.0 to get overflow protection without SafeMath  
- Use a solidity version of at least 0.8.2 to get compiler automatic inlining  
- Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads  
- Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than `revert()/require()` strings  
- Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

```
All contracts
```
## 3. No need to explicitly initialize variables with default values

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.


https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol
```
307	for (uint256 i = 0; i < secondsAgos.length; i++) {
```

## 4. Splitting require() statements that use && saves gas


Instead of using the && operator in a single require statement to check multiple conditions, I suggest using multiple require statements with 1 condition per require statement

Link to the issue: https://github.com/code-423n4/2022-01-xdefi-findings/issues/128

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol
```
110	require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
```
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol
```
739	require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
743	require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
953	require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));
968	require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
```
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol
```
46	require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
```

## 5. Using > 0 costs more gas than != 0 for Unsigned Integers

This change saves 6 gas per instance

Proof: While it may seem that > 0 is cheaper than !=, this is only true without the optimizer enabled and outside a require statement. If you enable the optimizer at 10k AND you’re in a require statement, this will save gas. You can see this tweet for more proofs: https://twitter.com/gzeon/status/1485428085885640706

I suggest changing > 0 with != 0 here

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol
```
224	require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges
434	require(liquidityDesired > 0, 'IL');
469	require(liquidityActual > 0, 'IIL2');
898	require(_liquidity > 0, 'L');
```
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol
```
52	require(price > 0);
53	require(liquidity > 0);
```

## 6. \<array>.length should not be looked up in every loop of a for-loop

The overheads outlined below are PER LOOP, excluding the first loop

- storage arrays incur a Gwarmaccess (100 gas)
- memory arrays use MLOAD (3 gas)
- calldata arrays use CALLDATALOAD (3 gas)

Caching the length changes each of these to a DUP\<N> (3 gas), and gets rid of the extra DUP\<N> needed to store the stack offset

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol
```
307	for (uint256 i = 0; i < secondsAgos.length; i++) {
```

## 7. Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.


## 8. Using bools for storage incurs overhead


// Booleans are more expensive than uint256 or any type that takes up a full
// word because each write operation emits an extra SLOAD to first read the
// slot's contents, replace the bits taken up by the boolean, and then write
// back. This is the compiler's defense against contract upgrades and 
// pointer aliasing, and it cannot be disabled.

https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27

Use uint256(1) and uint256(2) for true/false

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol
```
84	bool initialized,
293	bool toggledBottom;
294	bool toggledTop;
591	bool zeroToOne,
629	bool zeroToOne,
680	bool computedLatestTimepoint; //  if we have already fetched _tickCumulative_ and _secondPerLiquidity_ from the DataOperator
686	bool exactInput; // Whether the exact input or output is specified
695	bool initialized; // True if the _nextTick is initialized
704	bool zeroToOne,
728	bool unlocked = globalState.unlocked;
```
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol
```
15	bool initialized; // whether or not the timepoint is initialized
```
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol
```
24	bool zeroToOne
40	bool zeroToOne
49	bool zeroToOne,
50	bool fromInput
137	bool zeroToOne,
```
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol
```
27	bool initialized; // these 8 bits are set to prevent fresh sstores when crossing newly initialized ticks
88	bool upper
```
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol
```
71	bool lte
```
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TokenDeltaMath.sol
```
27	bool roundUp
49	bool roundUp
```
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolState.sol
```
15	bool unlocked; // True if the contract is unlocked, otherwise - false
```









