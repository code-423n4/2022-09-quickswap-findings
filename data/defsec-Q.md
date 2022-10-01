# QUICKSWAP LOW - NON-CRITICAL ISSUES

## ISSUE LIST

| Issues |
| -------- |
| **1. Anyone can create a fake pool to trick unauthorized front-ends**   | 
| **2. NatSpec is incomplete**| 
| **3. Unsafe casts uint256 to int256 and int256 to uint256**| 
| **4. Front-runnable Initializers**|
| **5. Missing zero-address check in the setter functions and initiliazers**|
| **6. transferOwnership should be two step**|
| **7. Upgrade pragma to at least 0.8.4**|
| **8. BaseFee Does not have any bound**|
| **9. Missing Overflow Protection**|


## ISSUES

## 1. Anyone can create a fake pool to trick unauthorized front-ends

While many Quickswap protocol functions are authorized to be called from other Quickswap contracts, the AlgebraFactory() function in PoolFactory is not authorized and is callable from anyone.  However, anyone can create a fake pool with the real base asset address but a fake token. If the threat model is extended to integrating contracts/front-ends that may assume all pools created (as indicated by emitted events) by createPool to be authentic ones (like the one created from the Wand), they may end up interacting or allow interactions with a fake pool which may lead to users losing funds.

```solidity
  function createPool(address tokenA, address tokenB) external override returns (address pool) {
    require(tokenA != tokenB);
    (address token0, address token1) = tokenA < tokenB ? (tokenA, tokenB) : (tokenB, tokenA);
    require(token0 != address(0));
    require(poolByPair[token0][token1] == address(0));

    IDataStorageOperator dataStorage = new DataStorageOperator(computeAddress(token0, token1));

    dataStorage.changeFeeConfiguration(baseFeeConfiguration);

    pool = IAlgebraPoolDeployer(poolDeployer).deploy(address(dataStorage), address(this), token0, token1);

    poolByPair[token0][token1] = pool; // to avoid future addresses comparing we are populating the mapping twice
    poolByPair[token1][token0] = pool;
    emit Pool(token0, token1, pool);
  }

```

**Recommendation**

Consider adding auth to createPool function and permissioning only Wand to access this function for creating pools.


## 2. NatSpec is incomplete

Some functions are missing @param for some of their parameters. Given that NatSpec is an important part of code documentation, this affects code comprehension, auditability and usability.

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L31

```solidity
  constructor(address _pool) {
    factory = msg.sender;
    pool = _pool;
  }
```

Consider adding in full NatSpec comments for all functions to have complete code documentation for future use.

## 3. Unsafe casts uint256 to int256 and int256 to uint256

During the code review, It has been observed that SafeCast library is included but It is not used in the DataStorage contract.

the cast -1(int256) to uint256 was 2^256 - 1
the cast 2**255(uint256) to int256 was - 2^255

```solidity
  function _volatilityOnRange(
    int256 dt,
    int256 tick0,
    int256 tick1,
    int256 avgTick0,
    int256 avgTick1
  ) internal pure returns (uint256 volatility) {
    // On the time interval from the previous timepoint to the current
    // we can represent tick and average tick change as two straight lines:
    // tick = k*t + b, where k and b are some constants
    // avgTick = p*t + q, where p and q are some constants
    // we want to get sum of (tick(t) - avgTick(t))^2 for every t in the interval (0; dt]
    // so: (tick(t) - avgTick(t))^2 = ((k*t + b) - (p*t + q))^2 = (k-p)^2 * t^2 + 2(k-p)(b-q)t + (b-q)^2
    // since everything except t is a constant, we need to use progressions for t and t^2:
    // sum(t) for t from 1 to dt = dt*(dt + 1)/2 = sumOfSequence
    // sum(t^2) for t from 1 to dt = dt*(dt+1)*(2dt + 1)/6 = sumOfSquares
    // so result will be: (k-p)^2 * sumOfSquares + 2(k-p)(b-q)*sumOfSequence + dt*(b-q)^2
    int256 K = (tick1 - tick0) - (avgTick1 - avgTick0); // (k - p)*dt
    int256 B = (tick0 - avgTick0) * dt; // (b - q)*dt
    int256 sumOfSquares = (dt * (dt + 1) * (2 * dt + 1)); // sumOfSquares * 6
    int256 sumOfSequence = (dt * (dt + 1)); // sumOfSequence * 2
    volatility = uint256((K**2 * sumOfSquares + 6 * B * K * sumOfSequence + 6 * dt * B**2) / (6 * dt**2));
  }
```

Use a SafeCast library of openzeppelin toUint256(int256 value) and toInt256(uint256 value) or check the number before cast it.


## 4. Front-runnable Initializers

## Impact - LOW

All contract **initializers** were missing access controls, allowing any user to initialize the contract. By front-running the contract deployers to initialize the contract, the incorrect parameters may be supplied, leaving the contract needing to be redeployed.


## Proof of Concept

1. Navigate to the following contracts.

```
https://github.com/code-423n4/2022-05-aura/blob/4989a2077546a5394e3650bf3c224669a0f7e690/convex-platform/contracts/contracts/ExtraRewardStashV3.sol#L69
```

2. initialize functions does not have access control. They are vulnerable to front-running.

## Tools Used

Manual Code Review

## Recommended Mitigation Steps

While the code that can be run in contract constructors is limited, setting the owner in the contract's constructor to the `msg.sender` and adding the `onlyOwner` modifier to all **initializers** would be a sufficient level of access control.

## 5. Missing zero-address check in the setter functions and initiliazers

## Impact

Missing checks for zero-addresses may lead to infunctional protocol, if the variable addresses are updated incorrectly.

## Proof of Concept

1. Navigate to the following contracts.

```
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L77

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L84

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L91
```

## Tools Used

Code Review

## Recommended Mitigation Steps

Consider adding zero-address checks in the discussed constructors:
require(newAddr != address(0));.


## 6. transferOwnership should be two step

## Impact - NON CRITICAL

The owner is the authorized user in the solidity contracts. Usually, an owner can be updated with transferOwnership function. However, the process is only completed with single transaction. If the address is updated incorrectly, an owner functionality will be lost forever.

## Proof of Concept

1. Navigate to the following contracts.

```
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L77
```

## Tools Used

Code Review

## Recommended Mitigation Steps

Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.

## 7. Upgrade pragma to at least 0.8.4

## Impact

Using newer compiler versions and the optimizer gives gas optimizations
and additional safety checks are available for free.

The advantages of versions 0.8.* over <0.8.0 are:

- Safemath by default from 0.8.0 (can be more gas efficient than
library based safemath.)
- Low level inliner : from 0.8.2, leads to cheaper runtime gas. Especially relevant when the contract has small functions. For example, OpenZeppelin libraries typically have a lot of small helper functions and if they are not inlined, they cost an additional 20 to 40 gas because of 2 extra jump instructions and additional stack operations needed for function calls.
- Optimizer improvements in packed structs: Before 0.8.3, storing packed structs, in some cases used an
additional storage read operation. After EIP-2929, if the slot was already cold, this means unnecessary stack operations and extra deploy time costs. However, if the slot was already warm, this means
additional cost of 100 gas alongside the same unnecessary stack operations and extra deploy time costs.
- Custom errors from 0.8.4, leads to cheaper deploy time cost and run time cost. Note: the run time cost is only relevant when the revert condition is met. In short, replace revert strings by custom errors.



## Proof of Concept

1. The contest repository contracts contain pragma 0.7.6. The contracts pragma version should be updated to 0.8.4.

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L2

## Tools Used

None

## Recommended Mitigation Steps

Consider to upgrade pragma to at least 0.8.4.


## 8. BaseFee Does not have any bound

During the code review, It has been noticed that baseFee does not have on setter function.

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L107

Without pre-defined threshold, baseFee can cause to divide by zero on the other functionalities.

## 9. Missing Overflow Protection

On the contracts, Pragma version 0.7.6 is used. The pragma version does not have overflow protection by default. From that reason, the contracts are missing overflow protection.

**Code Location**

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L801

**Recommendation**

Ensure that "+"&"-" operators should be replaced by the "add"&"sub" functions of SafeMath. Or Update pragma with 0.8.x.
