1.
Title: Gas savings for using solidity 0.8.10

Proof of Concept:
All contract in scope

Recommended Mitigation Steps:
Consider to upgrade pragma to at least 0.8.10.

Solidity 0.8.10 has a useful change which reduced gas costs of external calls
Reference: [here](https://blog.soliditylang.org/2021/11/09/solidity-0.8.10-release-announcement/)
______________________________________________________________________

2.
Title: Unchecking arithmetics operations that can't underflow/overflow

Proof of Concept:
[TokenDeltaMath.sol#L52](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TokenDeltaMath.sol#L52) Should be unchecked due to L#51

Recommended Mitigation Steps:
Use `unchecked`
________________________________________________________________________

3.
Title: Using `!=` in `require` statement is more gas efficient

Proof of Concept:
[PriceMovementMath.sol#L52-L53](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L52-L53)
[AlgebraPool.sol#L434](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L434)
[AlgebraPool.sol#L469](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L469)

Recommended Mitigation Steps:
Change `> 0` to `!= 0`
________________________________________________________________________

4.
Title: `>=` is cheaper than `>`

Impact:
Strict inequalities (`>`) are more expensive than non-strict ones (`>=`). This is due to some supplementary checks (ISZERO, 3 gas)

Proof of Concept:
[AlgebraPool.sol#L237](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L237)

Recommended Mitigation Steps:
Consider using `>=` instead of `>` to avoid some opcodes
________________________________________________________________________

5.
Title: Default value initialization

Impact:
If a variable is not set/initialized, it is assumed to have the default value (0, false, 0x0 etc depending on the data type). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

Proof of Concept:
[DataStorage.sol#L307](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307)

Recommended Mitigation Steps:
Remove explicit initialization for default values.
________________________________________________________________________

6.
Title: Caching `length` for loop can save gas

Proof of Concept:
[DataStorage.sol#L307](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307)

Recommended Mitigation Steps:
Change to:

```
    uint256 Length = secondsAgos.length;
    for (uint256 i = 0; i < Length; i++) {
```
________________________________________________________________________

7.
Title: Using unchecked and prefix increment is more effective for gas saving:

Proof of Concept:
[DataStorage.sol#L307](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307)

Recommended Mitigation Steps:
Change to:

```
for (uint256 i = 0; i < secondsAgos.length;) {
// ...
unchecked { ++i; }
}
```
________________________________________________________________________

8.
Title: Using multiple `require` instead `&&` can save gas

Proof of Concept:
[DataStorageOperator.sol#L46](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L46)
[AlgebraPool.sol#L739](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L739)
[AlgebraPool.sol#L743](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L743)

Recommended Mitigation Steps:
Change to:

```
	require(_feeConfig.gamma1 != 0, 'Gammas must be > 0');
	require(_feeConfig.gamma2 != 0, 'Gammas must be > 0');
	require(_feeConfig.volumeGamma != 0, 'Gammas must be > 0');
```
________________________________________________________________________

9.
Title: abi.encode() is less efficient than abi.encodePacked()

Proof of Concept:
[AlgebraPoolDeployer.sol#L51](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L51)
________________________________________________________________________

10.
Title: Set as `immutable` can save gas

Proof of Concept:
[AlgebraPoolDeployer.sol#L19](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L19)

Recommended Mitigation Steps:
can be set as immutable, which already set once in the constructor
________________________________________________________________________