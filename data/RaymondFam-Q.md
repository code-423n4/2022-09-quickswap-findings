## Two-step Transfer of Ownership
`setOwner()` should be implemented giving ownership to another address in a pending state. And then, a `claimOwner()` should be introduced for the newly nominated owner to claim ownership. This will ensure the new owner is going to be fully aware of the ownership assigned/transferred. Additionally, the risk of having the ownership transferred to an invalid address that would cause the contract to be without an owner will be avoided.

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L77-L81

## Solidity Version
Consider using the latest released version of Solidity. Apart from exceptional cases such having the optimizer give better gas optimizations, only the latest version receives security fixes. Furthermore, breaking changes as well as new features are introduced regularly. For instance, contract existence checks (>= 0.8.10) will have external calls skip contract existence checks if the external call has a return value. Additionally, solidity version of at least 0.8.13 features the ability to use `using for` with a list of free functions.

## Require Error Message
Consider making the require error message more verbose so all other peer developers would better be able to comprehend the intended statement logic. But, do limit the message length to less than 32 character where possible as strings that are more than 32 characters will require more than 1 storage slot, costing more gas. Here are some of the instances entailed:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L194
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L224

## Zero Address Checks
Implement zero address checks for all setter functions where possible to avoid accidental error(s) that could result in non-functional calls associated with it. Here are some of the instances entailed:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L84-L95.

## Events Associated With Setter Functions
Consider having events associated with setter functions emit both the new and old values instead of just the new value. Here are some of the few instances entailed:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L77-L95

## Typo Mistakes
The comment associated with the following code lines should have `... to a LP` replaced with `... to an LP`:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L46-L47

The comment associated with the following code lines should have `isTopTick` replaced with `isBottomTick`:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L309

The comment associated with the following code lines should have `... factor the ...` replaced with `... factor for the ...`:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L18

## Missing Require Error Message
Consider adding a less than 32 character string message to all require statements just so that a relevant message would be displayed just in case of a revert. Here are some of the instances entailed:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L229
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L960

## LowGasSafeMath for Other Bit Size Integer
As an example, consider using `LowGasSafeMath` for uint160 and uint32 for the following instances that involve arithmetic operation just in case of underflow:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L110-L111
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L142-L143

## Unnecessary Bitwise Or Logic
`(amount0 | amount1 != 0)` in the following code logic is similar to `(amount0 ! =0 || amount0 !=0)`.

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L501-L507

For simplicity, it should be refactored as follows:

```
if (amount0 != 0) {
  position.fees0 = positionFees0 - amount0;
  TransferHelper.safeTransfer(token0, recipient, amount0);
}

if (amount1 != 0) {
  position.fees1 = positionFees1 - amount1;
  TransferHelper.safeTransfer(token1, recipient, amount1);
}
``` 
There are two benefits of doing this:
1. Only one condition check is needed for amount0 and amount1.
2. If one of the conditions failed, it didn't have to go through the unnecessary steps of running line 502/503 and line 505/506.

##  Missing NatSpec
Solidity contracts can use a special form of comments, i.e., the Ethereum Natural Language Specification Format (NatSpec) to provide rich documentation for functions, return variables and more. Please visit the following link for further details:

https://docs.soliditylang.org/en/v0.8.16/natspec-format.html

The following contract is found to be having inadequate NatSpec:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/Constants.sol

## Sanity Check for Pool Address
Pool address is computed/generated twice in `createPool()` in `AlgebraFactory.sol` with no tests catered `computeAddress()`. Consider refactoring the following lines of codes:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L65-L69

```
    address cache = computeAddress(token0, token1);

    IDataStorageOperator dataStorage = new DataStorageOperator(cache);

    dataStorage.changeFeeConfiguration(baseFeeConfiguration);

    pool = IAlgebraPoolDeployer(poolDeployer).deploy(address(dataStorage), address(this), token0, token1);

    require(pool == cache, 'Invalid pool address');
```
## Lines Too Long
Lines in source code are typically limited to 80 characters, but it’s reasonable to stretch beyond this limit when need be as monitor screens theses days are comparatively larger. Considering the files will most likely reside in GitHub that will have a scroll bar automatically kick in when the length is over 164 characters, all code lines and comments should be split when/before hitting this length. Keep line width to max 120 characters for better readability where possible. Here is one of the instances entailed:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L38
