# QA
# Low
## Prevent div by 0
### Impact
On several locations in the code precautions are not being taken to not divide by 0, div by 0 would revert the code. 

### Proof of Concept
Navigate to the following contracts,
- some variables to 0 to accomplish div by 0
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/AdaptiveFee.sol#L55
      res = (alpha * ex) / (g8 + ex); // in worst case: (16 + 155 bits) / 155 bits

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/AdaptiveFee.sol#L62
      res = (alpha * g8) / ex; // in worst case: (16 + 128 bits) / 156 bits

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L53
    uint256((K**2 * sumOfSquares + 6 * B * K * sumOfSequence + 6 * dt * B**2) / (6 * dt**2));

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TokenDeltaMath.sol#L35
      : FullMath.mulDiv(priceDelta, liquidityShifted, priceUpper) / priceLower;


- Related to time most likely to not occur
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L122
          ? (lastTickCumulative - startTimepoint.tickCumulative) / (lastTimestamp - startTimepoint.blockTimestamp)

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L130
        avgTick = (lastTickCumulative - startOfWindow.tickCumulative) / (lastTimestamp - time + WINDOW);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L133
      avgTick = (lastTimestamp == oldestTimestamp) ? tick : (lastTickCumulative - oldestTickCumulative) / (lastTimestamp - oldestTimestamp);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L231
            prevTick = int24((last.tickCumulative - prevLast.tickCumulative) / (last.blockTimestamp - prevLast.blockTimestamp));

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L414
      prevTick = int24((last.tickCumulative - _prevLastTickCumulative) / (last.blockTimestamp - _prevLastBlockTimestamp));

- `timepointTimeDelta` being 0
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L251
      beforeOrAt.tickCumulative += ((atOrAfter.tickCumulative - beforeOrAt.tickCumulative) / timepointTimeDelta) * targetDelta;

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L253
        (uint256(atOrAfter.secondsPerLiquidityCumulative - beforeOrAt.secondsPerLiquidityCumulative) * targetDelta) / timepointTimeDelta

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L255
      beforeOrAt.volatilityCumulative += ((atOrAfter.volatilityCumulative - beforeOrAt.volatilityCumulative) / timepointTimeDelta) * targetDelta;

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L257
        ((atOrAfter.volumePerLiquidityCumulative - beforeOrAt.volumePerLiquidityCumulative) / timepointTimeDelta) *

Variable can be equal to zero therefore div by zero will occur.

### Recommended Mitigation Steps
Recommend making sure division by 0 won’t occur by checking the variables beforehand and handling this edge case.

## Variable shadows another variable
###  Summary 
Name shadowing where two or more variables/functions share the same name could be confusing to developers and/or reviewers
###  Github Permalinks 
Use of `liquidity` as variable in function, shadows `IAlgebraPoolState.liquidity()` and `PoolState.liquidity`
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L555

Use of `volumePerLiquidityInBlock` as variable shadows `PoolState.volumePerLiquidityInBlock`

###  Mitigation
Replace `liquidity` variable in the function parameter to `_liquidity` or a similar substitution
Same with `volumePerLiquidityInBlock`

## `owner` can not be transfered, if address needs to be transfered because of any kind of migration this would be a problem
### Impact
As `owner` address needs to be transfered, in case of being forced to stop using old address, functions with `onlyOwner` would get lost as `setFactory` is, an indeed, `onlyFactory` would get affect because of the same issue
### Github Permalink
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPoolDeployer.sol#L36-L53

### Recommended steps
- Add a source of owner assigning, recommended to include Ownable from OZ and a double step owner transfer

## Missing checks for address(0x0) when assigning values to `address` state or `immutable` variables 
### Summary
Zero address should be checked for state variables, immutable variables. A zero address can lead into problems.
### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L961
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L33
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L54
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L55

### Mitigation
Check zero address before assigning or using it


## Missing checks for address(0x0) on some functions
### Summary
Zero address should be checked for some function parameters. For example in functions like state variable changes, mints, withdrawals... 

### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L76-L95
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L959-L964
### Mitigation
Check zero address before assigning or using it

## Incorrect shift
### Proof of concept
```
 self[rowNumber] ^= 1 << bitNumber;
```
### Github permalink
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L14-L25

### Tools
Slither
### Recommendation
Swap the order of parameters.


## block.timestamp used as time proxy 
### Summary
Risk of using block.timestamp for time should be considered. 
### Details
block.timestamp is not an ideal proxy for time because of issues with synchronization, miner manipulation and changing block times. 

This kind of issue may affect the code allowing or reverting the code before the expected deadline, modifying the normal functioning or reverting sometimes.
### References
SWC ID: 116

### Github Permalinks 
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L193-L206
require(globalState.price == 0,AI)

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L921
  require(balance0Before.add(fee0) <= paid0,F0)
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L935
  require(balance1Before.add(fee1) <= paid1,F1)

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L356
cache.timepointIndex != newTimepointIndex

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L636
require(globalState.unlocked, 'LOK');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L382
currentTick < bottomTick
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L384
currentTick < topTick

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L229
require(bool)((_blockTimestamp() - lastLiquidityAddTimestamp) >= _liquidityCooldown)

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L245

innerFeeGrowth1Token != _innerFeeGrowth1Token
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L250
fees0 | fees1 != 0

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L703-L888

### Mitigation
- Consider the risk of using block.timestamp as time proxy and evaluate if block numbers can be used as an approximation for the application logic. Both have risks that need to be factored in. 
- Consider using an oracle for precision

## Front run initializer
### Summary
The initialize function that initializes important contract state can be called by anyone.
### Details
The attacker can initialize the contract before the legitimate deployer, hoping that the victim continues to use the same contract.

In the best case for the victim, they notice it and have to redeploy their contract costing gas.

### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L360-L373
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L193-L206
### Mitigation
Use the constructor to initialize non-proxied contracts.

For initializing proxy contracts deploy contracts using a factory contract that immediately calls initialize after deployment or make sure to call it immediately after deployment and verify the transaction succeeded.


## Use of asserts()
### Impact

From solidity docs: Properly functioning code should never reach a failing assert statement; if this happens there is a bug in your contract which you should fix.
With assert the user pays the gas and with require it doesn't. The ETH network gas isn't cheap and users can see it as a scam.
You have reachable asserts in the following locations (which should be replaced by require / are mistakenly left from development phase):

### Details
The Solidity assert() function is meant to assert invariants. Properly functioning code should never reach a failing assert statement. A reachable assertion can mean one of two things:

A bug exists in the contract that allows it to enter an invalid state;
The assert statement is used incorrectly, e.g. to validate inputs.

### References
SWC ID: 110

### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L190
    assert(false);

### Recommended Mitigation Steps
Substitute asserts with require/revert.


# Informational
##  Use of magic numbers is confusing and risky
### Summary
Magic numbers are hardcoded numbers used in the code which are ambiguous to their intended purpose. These should be replaced with constants to make code more readable and maintainable.
### Details
Values are hardcoded and would be more readable and maintainable if declared as a constant

[I would notice that I was pretty confuse when watching for example 128 and 255 because of finding in the same contract values that comes from 2^n and 2^n-1 operations in so short of time. Documentation is not enough clear on this kind of numbers and really is hard to assume what is going on. I'm not sure, but this can be a bug of 1?

In several locations in the code numbers like 128, 255, 500, 1e18 are used. It quite easy to make a mistake somewhere, also when comparing values or missing a 0 what can lead to some bad behaviors.]

### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L31-L38
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L138
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L158
### Mitigation
Replace magic hardcoded numbers with declared constants.

## Use of a more recent of solidity
### Summary
Newer versions of solidity get newer features that save gas and improve security of the code
### Details
- Use a solidity version of at least 0.8.0 to not include pragma abi coder v2 explicitly
- Use a solidity version of at least 0.8.0 to get by default revert on overflow/underflow rather than using SafeMath library
Use a solidity version of at least 0.8.4 to get custom Errors to be used instead of regular revert/require strings
### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPoolDeployer.sol
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/AdaptiveFee.sol
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/Constants.sol
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TokenDeltaMath.sol
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/base/PoolState.sol
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/base/PoolImmutables.sol

### Mitigation
Consider changing to pragma 0.8.4 at least

## Bad order of code
### Summary
Clearness of the code is important for the readability and maintainability.
As Solidity guidelines says about declaration order:
1.Type declarations
2.State variables
3.Events
4.Modifiers
5.Functions
Also, state variables order affects to gas in the same way as ordering structs for saving storage slots

### Github Permalink
- structs are declared between functions
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L1-L972

- constructor after functions rather than before
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/base/PoolImmutables.sol#L19-L32

- constant should be declared earlier, and the modifier after the mapping
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L42-L116
### Mitigation
Follow solidity style guidelines https://docs.soliditylang.org/en/v0.8.15/style-guide.html

## Missing Natspec 
### Summary 
Missing Natspec and regular comments affect readability and maintainability of a codebase. 
### Details 
Contracts has partial or full lack of comments
### Github Permalinks 
#### Natspec @param
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L150-L160
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L131-L142
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L31-L128
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L53-L206
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L273-L394
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L415-L972
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L44-L123
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L50-L125
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPoolDeployer.sol#L35-L53
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/AdaptiveFee.sol#L24-L109
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L105-L135
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L121-L129
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/base/PoolImmutables.sol#L29-L32

#### Natspec @return value
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L70
  function balanceToken0() private view returns (uint256) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L74
  function balanceToken1() private view returns (uint256) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L83
    returns (

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L108
    returns (

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L175
    returns (

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L375
    returns (

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L428
    returns (

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L494
  ) external override lock returns (uint128 amount0, uint128 amount1) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L517
  ) external override lock onlyValidTicks(bottomTick, topTick) returns (uint256 amount0, uint256 amount1) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L541
  ) private returns (uint16 newFee) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L557
  ) private returns (uint16 newTimepointIndex) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L570
    returns (

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L595
  ) external override returns (int256 amount0, int256 amount1) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L633
  ) external override returns (int256 amount0, int256 amount1) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L709
    returns (

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L59
  function createPool(address tokenA, address tokenB) external override returns (address pool) {


https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPoolDeployer.sol#L49
  ) external override onlyFactory returns (address pool) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L64
    returns (

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L99
    returns (

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L115
  ) external view override onlyPool returns (uint112 TWVolatilityAverage, uint256 TWVolumePerLiqAverage) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L126
  ) external override onlyPool returns (uint16 indexUpdated) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L135
  ) external pure override returns (uint128 volumePerLiquidity) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L145
  function window() external pure override returns (uint32) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L155
  ) external view override onlyPool returns (uint16 fee) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/AdaptiveFee.sol#L29
  ) internal pure returns (uint16 fee) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/AdaptiveFee.sol#L49
  ) internal pure returns (uint256 res) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/AdaptiveFee.sol#L74
  ) internal pure returns (uint256 res) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L51
  ) internal pure returns (uint160 resultPrice) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L97
  ) internal pure returns (uint256) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L105
  ) internal pure returns (uint256) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L113
  ) internal pure returns (uint256) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L121
  ) internal pure returns (uint256) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L46
  ) internal view returns (uint256 innerFeeGrowth0Token, uint256 innerFeeGrowth1Token) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L30
  function getSingleSignificantBit(uint256 word) internal pure returns (uint8 singleBitPos) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TokenDeltaMath.sol#L28
  ) internal pure returns (uint256 token0Delta) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TokenDeltaMath.sol#L50
  ) internal pure returns (uint256 token1Delta) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TokenDeltaMath.sol#L65
  ) internal pure returns (int256 token0Delta) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TokenDeltaMath.sol#L80
  ) internal pure returns (int256 token1Delta) {


https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/base/PoolImmutables.sol#L20
  function tickSpacing() external pure override returns (int24) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/base/PoolImmutables.sol#L25
  function maxLiquidityPerTick() external pure override returns (uint128) {


 
 ### mitigation
 - Add @param descriptors
 - Add @return descriptors
 - Add inline comments. 
 - Add comments for what the contract does

## Unused code
### Summary
Code that is not used should be removed
### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L93-L99

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L277-L316

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L384-L418

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L148-L191

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L66-L86

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L101-L107

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L94-L101

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L364-L373

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L205-L263

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L105-L135

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L326-L358
### Mitigation
Remove the code that is not used.


## Missing initializer modifier on constructor
### Impact
OpenZeppelin [recommends](https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/5) that the initializer modifier be applied to constructors in order to avoid potential griefs, [social engineering](https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/4), or exploits. 

### Github Permalink
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L66-L68
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L31-L34
### Mitigation
Ensure that the modifier is applied to the implementation contract. If the default constructor is currently being used, it should be changed to be an explicit one with the modifier applied.

## So similar name in different variables or functions
### Summary
Names being so similar may lead to unexpected calls and assumptions.

### Details
Multiple instances over all the code where naming is being declared different just by using 0 or 1, this affects readability and can create wrong assumptions over the code what may lead into typo errors

### Github Permalinks
AlgebraPool._calculateSwapAndLock(bool,int256,uint160)._communityFeeToken0 is too similar to AlgebraPool._calculateSwapAndLock(bool,int256,uint160)._communityFeeToken1

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L726
- TokenDeltaMath.getToken0Delta(uint160,uint160,int128).token0Delta is too similar to TokenDeltaMath.getToken1Delta(uint160,uint160,uint128,bool).token1Delta

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TokenDeltaMath.sol#L65

- AlgebraPool.collect(address,int24,int24,uint128,uint128).amount0Requested is too similar to AlgebraPool.collect(address,int24,int24,uint128,uint128).amount1Requested

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L492

- AlgebraPool.mint(address,address,int24,int24,uint128,bytes).receivedAmount0 is too similar to AlgebraPool.mint(address,address,int24,int24,uint128,bytes).receivedAmount1

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L448

- AlgebraPool.swapSupportingFeeOnInputTokens(address,address,bool,int256,uint160,bytes).balance0Before is too similar to AlgebraPool.swapSupportingFeeOnInputTokens(address,address,bool,int256,uint160,bytes).balance1Before

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L639

- TickManager.update(mapping(int24 => TickManager.Tick),int24,int24,int128,uint256,uint256,uint160,int56,uint32,bool).totalFeeGrowth0Token is too similar to TickManager.update(mapping(int24 => TickManager.Tick),int24,int24,int128,uint256,uint256,uint160,int56,uint32,bool).totalFeeGrowth1Token

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L83

- TickManager.update(mapping(int24 => TickManager.Tick),int24,int24,int128,uint256,uint256,uint160,int56,uint32,bool).totalFeeGrowth0Token is too similar to TickManager.getInnerFeeGrowth(mapping(int24 => TickManager.Tick),int24,int24,int24,uint256,uint256).totalFeeGrowth1Token

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L83

- TokenDeltaMath.getToken0Delta(uint160,uint160,uint128,bool).token0Delta is too similar to TokenDeltaMath.getToken1Delta(uint160,uint160,uint128,bool).token1Delta

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TokenDeltaMath.sol#L28

- TickManager.getInnerFeeGrowth(mapping(int24 => TickManager.Tick),int24,int24,int24,uint256,uint256).totalFeeGrowth0Token is too similar to TickManager.cross(mapping(int24 => TickManager.Tick),int24,uint256,uint256,uint160,int56,uint32).totalFeeGrowth1Token

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L44
- AlgebraPool.flash(address,uint256,uint256,bytes)._communityFeeToken0 is too similar to AlgebraPool._calculateSwapAndLock(bool,int256,uint160)._communityFeeToken1

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L925

- TokenDeltaMath.getToken0Delta(uint160,uint160,int128).token0Delta is too similar to TokenDeltaMath.getToken1Delta(uint160,uint160,int128).token1Delta

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TokenDeltaMath.sol#L65

- AlgebraPool.flash(address,uint256,uint256,bytes).balance0Before is too similar to AlgebraPool.flash(address,uint256,uint256,bytes).balance1Before

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L903

- AlgebraPool.swapSupportingFeeOnInputTokens(address,address,bool,int256,uint160,bytes).balance0Before is too similar to AlgebraPool.flash(address,uint256,uint256,bytes).balance1Before

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L639
- TickManager.getInnerFeeGrowth(mapping(int24 => TickManager.Tick),int24,int24,int24,uint256,uint256).totalFeeGrowth0Token is too similar to TickManager.getInnerFeeGrowth(mapping(int24 => TickManager.Tick),int24,int24,int24,uint256,uint256).totalFeeGrowth1Token

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L44

- AlgebraPool.flash(address,uint256,uint256,bytes).balance0Before is too similar to AlgebraPool.swapSupportingFeeOnInputTokens(address,address,bool,int256,uint160,bytes).balance1Before

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L903

- AlgebraPool._recalculatePosition(AlgebraPool.Position,int128,uint256,uint256)._innerFeeGrowth0Token is too similar to AlgebraPool._recalculatePosition(AlgebraPool.Position,int128,uint256,uint256)._innerFeeGrowth1Token

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L242

- TokenDeltaMath.getToken0Delta(uint160,uint160,uint128,bool).token0Delta is too similar to TokenDeltaMath.getToken1Delta(uint160,uint160,int128).token1Delta

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TokenDeltaMath.sol#L28

- AlgebraPool._recalculatePosition(AlgebraPool.Position,int128,uint256,uint256).innerFeeGrowth0Token is too similar to AlgebraPool._recalculatePosition(AlgebraPool.Position,int128,uint256,uint256).innerFeeGrowth1Token

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L218

- AlgebraPool._updatePositionTicksAndFees(address,int24,int24,int128).feeGrowthInside0X128 is too similar to AlgebraPool._updatePositionTicksAndFees(address,int24,int24,int128).feeGrowthInside1X128

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L334

- AlgebraPool.swap(address,bool,int256,uint160,bytes).balance0Before is too similar to AlgebraPool.swap(address,bool,int256,uint160,bytes).balance1Before

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L606

- TickManager.cross(mapping(int24 => TickManager.Tick),int24,uint256,uint256,uint160,int56,uint32).totalFeeGrowth0Token is too similar to TickManager.update(mapping(int24 => TickManager.Tick),int24,int24,int128,uint256,uint256,uint160,int56,uint32,bool).totalFeeGrowth1Token

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L132
- AlgebraPool._recalculatePosition(AlgebraPool.Position,int128,uint256,uint256).innerFeeGrowth0Token is too similar to IAlgebraPoolState.positions(bytes32).innerFeeGrowth1Token

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L218

- AlgebraPool.flash(address,uint256,uint256,bytes).balance0Before is too similar to AlgebraPool.swap(address,bool,int256,uint160,bytes).balance1Before

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L903

- AlgebraPool._updatePositionTicksAndFees(address,int24,int24,int128)._totalFeeGrowth0Token is too similar to AlgebraPool._updatePositionTicksAndFees(address,int24,int24,int128)._totalFeeGrowth1Token

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L291

- AlgebraPool.collect(address,int24,int24,uint128,uint128).positionFees0 is too similar to AlgebraPool.collect(address,int24,int24,uint128,uint128).positionFees1

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L496
- AlgebraPool.swapSupportingFeeOnInputTokens(address,address,bool,int256,uint160,bytes).balance0Before is too similar to AlgebraPool.swap(address,bool,int256,uint160,bytes).balance1Before

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L639

- AlgebraPool._calculateSwapAndLock(bool,int256,uint160)._communityFeeToken0 is too similar to AlgebraPool.flash(address,uint256,uint256,bytes)._communityFeeToken1

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L726

- TickManager.cross(mapping(int24 => TickManager.Tick),int24,uint256,uint256,uint160,int56,uint32).totalFeeGrowth0Token is too similar to TickManager.getInnerFeeGrowth(mapping(int24 => TickManager.Tick),int24,int24,int24,uint256,uint256).totalFeeGrowth1Token

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L132

- AlgebraPool.swap(address,bool,int256,uint160,bytes).balance0Before is too similar to AlgebraPool.swapSupportingFeeOnInputTokens(address,address,bool,int256,uint160,bytes).balance1Before

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L606

- AlgebraPool.collect(address,int24,int24,uint128,uint128).amount0Requested is too similar to IAlgebraPoolActions.collect(address,int24,int24,uint128,uint128).amount1Requested

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L492

- TickManager.cross(mapping(int24 => TickManager.Tick),int24,uint256,uint256,uint160,int56,uint32).totalFeeGrowth0Token is too similar to TickManager.cross(mapping(int24 => TickManager.Tick),int24,uint256,uint256,uint160,int56,uint32).totalFeeGrowth1Token

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L132

- AlgebraPool.setCommunityFee(uint8,uint8).communityFee0 is too similar to AlgebraPool.setCommunityFee(uint8,uint8).communityFee1

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L952

- TickManager.update(mapping(int24 => TickManager.Tick),int24,int24,int128,uint256,uint256,uint160,int56,uint32,bool).totalFeeGrowth0Token is too similar to TickManager.cross(mapping(int24 => TickManager.Tick),int24,uint256,uint256,uint160,int56,uint32).totalFeeGrowth1Token

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L83

- AlgebraPool.flash(address,uint256,uint256,bytes)._communityFeeToken0 is too similar to AlgebraPool.flash(address,uint256,uint256,bytes)._communityFeeToken1
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L925

- AlgebraPool.setCommunityFee(uint8,uint8).communityFee0 is too similar to IAlgebraPoolPermissionedActions.setCommunityFee(uint8,uint8).communityFee1

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L952

- AlgebraPool.swap(address,bool,int256,uint160,bytes).balance0Before is too similar to AlgebraPool.flash(address,uint256,uint256,bytes).balance1Before

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L606

- TickManager.getInnerFeeGrowth(mapping(int24 => TickManager.Tick),int24,int24,int24,uint256,uint256).totalFeeGrowth0Token is too similar to TickManager.update(mapping(int24 => TickManager.Tick),int24,int24,int128,uint256,uint256,uint160,int56,uint32,bool).totalFeeGrowth1Token

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L44

- PoolState.totalFeeGrowth0Token is too similar to PoolState.totalFeeGrowth1Token

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/base/PoolState.sol#L19
### Mitigation
Consider making more unique the names of this variables/functions

## Maximum line length exceeded
### Summary
Long lines should be wrapped to conform with Solidity Style guidelines. 
### Details 
Lines that exceed the 99 character length suggested by the Solidity Style guidelines. Reference: https://docs.soliditylang.org/en/v0.8.10/style-guide.html#maximum-line-length
### Github Permalinks 
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L137

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L149

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L158

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L212

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L213

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L221

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L237

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L247

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L252

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L271

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L272

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L287

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L291

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L297

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L345

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L352

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L355

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L357

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L383

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L385

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L386

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L392

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L460

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L463

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L472

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L517

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L518

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L529

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L558

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L577

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L601

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L604

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L610

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L621

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L654

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L660

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L664

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L671

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L680

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L682

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L753

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L788

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L789

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L792

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L801

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L802

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L804

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L805

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L809

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L814

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L826

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L872

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L873

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L876

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L880

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L952

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L953

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L954

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L968

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L69

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L71

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L109

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L112

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L113

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L116

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L123

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPoolDeployer.sol#L50

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L16

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L42

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L45

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L46

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L78

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L79

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L115

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L156

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/AdaptiveFee.sol#L23

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/AdaptiveFee.sol#L38

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/AdaptiveFee.sol#L76

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L17

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L18

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L19

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L24

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L44

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L53

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L56

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L57

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L80

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L81

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L89

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L90

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L122

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L125

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L130

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L133

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L137

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L139

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L140

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L144

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L156

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L161

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L166

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L172

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L186

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L195

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L199

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L201

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L223

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L231

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L239

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L251

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L253

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L255

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L257

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L265

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L269

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L271

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L273

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L274

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L275

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L276

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L308

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L309

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L322

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L341

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L345

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L348

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L354

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L355

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L360

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L362

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L376

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L378

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L383

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L408

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L414

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L417

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L8

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L35

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L64

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L67

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L70

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L71

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L72

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L80

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L125

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L126

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L128

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L132

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L133

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L134

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L153

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L163

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L174

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L176

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L182

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L19

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L20

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L25

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L26

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L27

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L37

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L38

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L66

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L70

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L76

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L98

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L108

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L128

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L9

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L15

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L28

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L32

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L33

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L34

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L35

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L36

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L37

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L38

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L39

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L44

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L61

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L65

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L66

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L67

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L77

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L89

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TokenDeltaMath.sol#L11

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TokenDeltaMath.sol#L22

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TokenDeltaMath.sol#L34

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TokenDeltaMath.sol#L44

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TokenDeltaMath.sol#L53

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TokenDeltaMath.sol#L60

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TokenDeltaMath.sol#L75

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/base/PoolState.sol#L13

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/base/PoolState.sol#L39


### Mitigation
Reduce line length to less than 99 at least to improve maintainability and readability of the code 

## Large multiples of ten should use scientific notation (e.g. 1e6) rather than decimal literals (e.g. 1000000), for readability
### Summary
Multiples of 10 can be declared as constants with scientific notation so it's easier to read them and less prone to miss/exceed a 0 of the expected value.

### Details
Values 100000 can be used in scientific notation
### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L16
### Mitigation
Replace hardcoded numbers with constants that represent the scientific corresponding notation

## Missing error messages in require statements
### Summary
Require/revert statements should include error messages in order to help at monitoring the system.
### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L953
    require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L960
    require(msg.sender == IAlgebraFactory(factory).farmingAddress());

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L968
    require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L43
    require(msg.sender == owner);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L60
    require(tokenA != tokenB);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L62
    require(token0 != address(0));

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L63
    require(poolByPair[token0][token1] == address(0));

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L78
    require(owner != _owner);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L85
    require(farmingAddress != _farmingAddress);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L92
    require(vaultAddress != _vaultAddress);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPoolDeployer.sol#L22
    require(msg.sender == factory);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPoolDeployer.sol#L27
    require(msg.sender == owner);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPoolDeployer.sol#L37
    require(_factory != address(0));

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPoolDeployer.sol#L38
    require(factory == address(0));

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L43
    require(msg.sender == factory || msg.sender == IAlgebraFactory(factory).owner());

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L369
    require(!self[0].initialized);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L52
    require(price > 0);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L53
    require(liquidity > 0);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L70
        require((product = amount * price) / amount == price); // if the product overflows, we know the denominator underflows

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L71
        require(liquidityShifted > product); // in addition, we must check that the denominator does not underflow

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L87
        require(price > quotient);
        
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TokenDeltaMath.sol#L30
    require(priceDelta < priceUpper); // forbids underflow and 0 priceLower

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TokenDeltaMath.sol#L51
    require(priceUpper >= priceLower);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L55
    require(msg.sender == IAlgebraFactory(factory).owner());

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L122
      require(_lower.initialized);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L134
      require(_upper.initialized);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L229
          require((_blockTimestamp() - lastLiquidityAddTimestamp) >= _liquidityCooldown);

### Mitigation
Add error messages 
