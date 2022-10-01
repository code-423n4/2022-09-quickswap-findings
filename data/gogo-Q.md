# 2022-09-QUICKSWAP
## Low Risk and Non-Critical Issues

### Events not emmited on important state changes
Emmiting events is recommended each time when a state variable's value is being changed or just some critical event for the contract has occurred. It also helps off-chain monitoring of the contract's state.

_There are **1** instances of this issue:_

```solidity
File: src/core/contracts/AlgebraPoolDeployer.sol

44:   function deploy(
```
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol

### Use `1e18` instead of `10**18` or `1000000000000000000`


_There are **2** instances of this issue:_

```solidity
File: src/core/contracts/libraries/Constants.sol

6:    uint256 internal constant Q96 = 0x1000000000000000000000000;

7:    uint256 internal constant Q128 = 0x100000000000000000000000000000000;
```
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/Constants.sol

### Natspec is missing


_There are **1** instances of this issue:_

```solidity
File: src/core/contracts/libraries/Constants.sol

      /// @audit
```
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/Constants.sol

### Not used import


_There are **4** instances of this issue:_

```solidity
File: src/core/contracts/AlgebraPool.sol

25:   import './interfaces/IAlgebraPoolDeployer.sol';
```
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol

```solidity
File: src/core/contracts/DataStorageOperator.sol

12:   import './libraries/Constants.sol';
```
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol

```solidity
File: src/core/contracts/libraries/AdaptiveFee.sol

4:    import './Constants.sol';
```
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol

```solidity
File: src/core/contracts/libraries/DataStorage.sol

4:    import './FullMath.sol';
```
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol
