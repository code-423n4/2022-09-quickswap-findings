
# GAS
## Upgrade to solidity 0.8.4 to use of custom errors rather than revert() / require() error message
### Summary
Custom errors reduce 38 gas if the condition is met and 22 gas otherwise.
Also reduces contract size and deployment costs.
### Details
Since version 0.8.4 the use of custom errors rather than revert() / require() saves gas as noticed in
https://blog.soliditylang.org/2021/04/21/custom-errors/
https://github.com/code-423n4/2022-04-pooltogether-findings/issues/13

### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L60
    require(topTick < TickMath.MAX_TICK + 1, 'TUM');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L61
    require(topTick > bottomTick, 'TLU');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L62
    require(bottomTick > TickMath.MIN_TICK - 1, 'TLM');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L194
    require(globalState.price == 0, 'AI');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L224
      require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L434
    require(liquidityDesired > 0, 'IL');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L454
      if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L455
      if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L469
    require(liquidityActual > 0, 'IIL2');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L474
      require((amount0 = uint256(amount0Int)) <= receivedAmount0, 'IIAM2');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L475
      require((amount1 = uint256(amount1Int)) <= receivedAmount1, 'IIAM2');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L608
      require(balance0Before.add(uint256(amount0)) <= balanceToken0(), 'IIA');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L614
      require(balance1Before.add(uint256(amount1)) <= balanceToken1(), 'IIA');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L636
    require(globalState.unlocked, 'LOK');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L641
      require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L645
      require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L731
      require(unlocked, 'LOK');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L733
      require(amountRequired != 0, 'AS');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L739
        require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L743
        require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L898
    require(_liquidity > 0, 'L');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L921
    require(balance0Before.add(fee0) <= paid0, 'F0');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L935
    require(balance1Before.add(fee1) <= paid1, 'F1');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L109
    require(uint256(alpha1) + uint256(alpha2) + uint256(baseFee) <= type(uint16).max, 'Max fee exceeded');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L110
    require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L27
    require(msg.sender == pool, 'only pool can call this');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L45
    require(uint256(_feeConfig.alpha1) + uint256(_feeConfig.alpha2) + uint256(_feeConfig.baseFee) <= type(uint16).max, 'Max fee exceeded');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L46
    require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L238
    require(lteConsideringOverflow(self[oldestIndex].blockTimestamp, target, time), 'OLD');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L96
    require(liquidityTotalAfter < Constants.MAX_LIQUIDITY_PER_TICK + 1, 'LO');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L15
    require(tick % Constants.TICK_SPACING == 0, 'tick is not spaced'); // ensure that the tick is spaced

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/base/PoolState.sol#L41
    require(globalState.unlocked, 'LOK');


### Mitigation
Consider upgrading to solidity version 0.8.4 at least and replace each error message in a require by a custom error

## duplicated require() check should be refactored
### Summary
duplicated require() / revert() checks should be
refactored to a modifier or function to save gas
### Details
Event appears twice and can be reduced

### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L454
      if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L455
      if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');
- - - -
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L608
      require(balance0Before.add(uint256(amount0)) <= balanceToken0(), 'IIA');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L614
      require(balance1Before.add(uint256(amount1)) <= balanceToken1(), 'IIA');
- - - -
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L731
      require(unlocked, 'LOK');
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L636
    require(globalState.unlocked, 'LOK');
- - - -
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L739
        require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L743
        require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');

### Mitigation
refactor this checks to different functions to save gas

## use != rather than >0 for unsigned integers in require() statements
### Summary
When the optimizer is enabled, gas is wasted by doing a greater-than operation, rather than a not-equals operation inside require() statements. When Using != , the optimizer is able to avoid the EQ, ISZERO, and associated operations, by relying on the JUMPI that comes afterwards, which itself checks for zero.
### Github Permalinks

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L52
    require(price > 0);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L53
    require(liquidity > 0);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L898
    require(_liquidity > 0, 'L');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L645
      require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L641
      require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L469
    require(liquidityActual > 0, 'IIL2');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L434
    require(liquidityDesired > 0, 'IL');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L224
      require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges

### Mitigation
Use != 0 rather than > 0 for unsigned integers in require()  statements.

## splitting require() statements that use && saves gas
### Summary
Instead of using the && operator in a single require statement to check multiple conditions. Consider using multiple require statements with 1 condition per require statement (saving 3 gas per & ):
### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L739
        require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L743
        require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L953
    require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L968
    require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L110
    require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L46
    require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');



### Mitigation
Split require && statements

## usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead
### Summary
When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

### Details
https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html 
Use a larger size than downcast where needed

### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/Constants.sol#L5
  uint8 internal constant RESOLUTION = 96;

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/Constants.sol#L16
  uint8 internal constant MAX_COMMUNITY_FEE = 250;

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/Constants.sol#L5
  uint8 internal constant RESOLUTION = 96;

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/Constants.sol#L16
  uint8 internal constant MAX_COMMUNITY_FEE = 250;

### Mitigation
Consider using some data type that uses 32 bytes, for example uint256

## Pack structs tightly
### Summary
Gas efficiency can be achieved by tightly packing the struct. Struct variables are stored in 32 bytes each so you can group smaller types to occupy less storage. 

### Details
You can read more here: https://fravoll.github.io/solidity-patterns/tight_variable_packing.html or in the official documentation: https://docs.soliditylang.org/en/v0.4.21/miscellaneous.html
### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L41
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L675
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L692
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/AdaptiveFee.sol#L10

### Mitigation
Order structs to reduce gas usage.

## Using private rather than public for constants saves gas
### Summary
If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table

### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L12
  uint32 public constant WINDOW = 1 days;

### Mitigation
Consider replacing public for private in constants for gas saving.

## Index initialized in for loop
### Summary
In for loops is not needed to initialize indexes to 0 as it is the default uint/int value. This saves gas.

### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L307
    for (uint256 i = 0; i < secondsAgos.length; i++) {

### Mitigation
Don't initialize variables to default value


## use of i++ in loop rather than ++i
### Summary
++i costs less gas than i++, especially when it's used in for loops
### Details
using ++i doesn't affect the flow of regular for loops and improves gas cost

### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L307
    for (uint256 i = 0; i < secondsAgos.length; i++) {
### Mitigation
Substitute to ++i 

## ++i costs less gas compared to i++ or i+=1, the same happens with --i and i-- or i-=1
### Summary
++i costs less gas compared to i++ or i += 1 for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration). 
This statement is true even with the optimizer enabled. 

### Details
i++ increments i and returns the initial value of i . 
Which means:
uint i = 1;
i++; // == 1 but i == 2

But ++i returns the actual incremented value:

uint i = 1;
++i; // == 2 and i == 2 too, so no need for a temporary variable

In the first case, the compiler has to create a temporary variable (when used) for returning 1 instead of 2

### Github Permalinks
+= 1
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L100
      tick += 1;

-= 1
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L119
        index -= 1; // considering underflow

### Mitigation
Replace to ++i or --i as needed.


## increments can be unchecked in loops
### Summary
Unchecked operations as the ++i on for loops are cheaper than checked one.

### Details
In Solidity 0.8+, there’s a default overflow check on unsigned integers. It’s possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline..

    The code would go from:

    for (uint256 i; i < numIterations; i++) {
    // ...
    }

    to

    for (uint256 i; i < numIterations;) {
      // ...
      unchecked { ++i; }
    }
    The risk of overflow is inexistent for a uint256 here.

### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L307
    for (uint256 i = 0; i < secondsAgos.length; i++) {
### Mitigation
Add unchecked ++i at the end of all the for loop where it's not expected to overflow and remove them from the for header


## <array>.length should no be looked up in every loop of a for-loop
### Summary
In loops not assigning the length to a variable so memory accessed a lot (caching local variables)

### Details
The overheads outlined below are PER LOOP, excluding the first loop
storage arrays incur a Gwarmaccess (100 gas)
memory arrays use MLOAD (3 gas)
calldata arrays use CALLDATALOAD (3 gas)

### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L307
    for (uint256 i = 0; i < secondsAgos.length; i++) {


### Mitigation
Assign the length of the array.length to a local variable in loops for gas savings


## Variables should be cached when used several times
### Summary
Variables read more than once improves gas usage when cached into local variable
### Details
In loops or state variables, this is even more gas saving

### Github Permalinks
`beforeOrAt`, `atOrAfter` can be cached into local variables and then used/assigned/returned rather than accessing state variables multiple times
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L159-L190

### Mitigation
Cache variables used more than one into a local variable.


## Shift right instead of dividing by 2
### Summary
Shifting is cheaper than dividing by 2

### Details
A division by 2 can be calculated by shifting one to the right.
While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity’s division operation also includes a division-by-0 prevention which is bypassed using shifting.

### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/AdaptiveFee.sol#L88
    res += (xLowestDegree * gHighestDegree) / 2;

### Mitigation
Consider replacing / 2 with >> 1 here

## Functions guaranteed to revert when called by normal users can be marked payable
### Summary
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function.

Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

### Details
The extra opcodes avoided are:
CALLVALUE (2), DUP1 (3), ISZERO (3), PUSH2 (3), JUMPI (10), PUSH1 (3), DUP1 (3), REVERT(0), JUMPDEST (1), POP (2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L77
  function setOwner(address _owner) external override onlyOwner {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L84
  function setFarmingAddress(address _farmingAddress) external override onlyOwner {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L91
  function setVaultAddress(address _vaultAddress) external override onlyOwner {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraFactory.sol#L108
  ) external override onlyOwner {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPoolDeployer.sol#L36
  function setFactory(address _factory) external override onlyOwner {


### Mitigation
Consider adding payable to save gas

## >= cheaper than >
### Summary
Strict inequalities ( > ) are more expensive than non-strict ones ( >= ). This is due to some supplementary checks (ISZERO, 3 gas)

### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L228
        if (_liquidityCooldown > 0) {
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L237
        liquidityNext > 0 ? (liquidityDelta > 0 ? _blockTimestamp() : lastLiquidityAddTimestamp) : 0

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L451
      if (amount0 > 0) receivedAmount0 = balanceToken0();

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L452
      if (amount1 > 0) receivedAmount1 = balanceToken1();

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L454
      if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L455
      if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L505
      if (amount0 > 0) TransferHelper.safeTransfer(token0, recipient, amount0);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L506
      if (amount1 > 0) TransferHelper.safeTransfer(token1, recipient, amount1);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L617
    if (communityFee > 0) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L667
    if (communityFee > 0) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L734
      (cache.amountRequiredInitial, cache.exactInput) = (amountRequired, amountRequired > 0);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L808
      if (cache.communityFee > 0) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L814
      if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L904
    if (amount0 > 0) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L911
    if (amount1 > 0) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L924
    if (paid0 > 0) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L927
      if (_communityFeeToken0 > 0) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L938
    if (paid1 > 0) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L941
      if (_communityFeeToken1 > 0) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L138
    if (volume >= 2**192) volumeShifted = (type(uint256).max) / (liquidity > 0 ? liquidity : 1);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L139
    else volumeShifted = (volume << 64) / (liquidity > 0 ? liquidity : 1);

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L80
    last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L226
      if (liquidityDelta < 0) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L346
      if (liquidityDelta < 0) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L604
      if (amount1 < 0) TransferHelper.safeTransfer(token1, recipient, uint256(-amount1)); // transfer to recipient

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L610
      if (amount0 < 0) TransferHelper.safeTransfer(token0, recipient, uint256(-amount0)); // transfer to recipient

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L658
      if (amount1 < 0) TransferHelper.safeTransfer(token1, recipient, uint256(-amount1));

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L662
      if (amount0 < 0) TransferHelper.safeTransfer(token0, recipient, uint256(-amount0));
### Mitigation
Consider using >= 1 instead of > 0 to avoid some opcodes


## <X> += <Y> costs more gas than <X> = <X> + <Y> for state variables
### Summary
x+=y costs more gas than x=x+y for state variables

### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L256
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L931
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L931
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L945
### Mitigation
Don't use += for state variables as it cost more gas.

## Use calldata instead of memory for function parameters
### Summary
It is generally cheaper to load variables directly from calldata, rather than copying them to memory. 
### Details
Only use memory if the variable needs to be modified
### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L88-L107

### Mitigation
Use calldata rather than memory in external functions where the parameter is not modified but only read

## Unused named returns
### Summary
Using both named returns and a return statement isn’t necessary. Removing one of those can improve code clarity 
### Details
Also as returns variable is ignored, it wastes extra gas

### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L83
    returns (

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L108
    returns (

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L175
    returns (

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L557
  ) private returns (uint16 newTimepointIndex) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPool.sol#L570
    returns (

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L99
    returns (

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L115
  ) external view override onlyPool returns (uint112 TWVolatilityAverage, uint256 TWVolumePerLiqAverage) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L126
  ) external override onlyPool returns (uint16 indexUpdated) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L135
  ) external pure override returns (uint128 volumePerLiquidity) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/DataStorageOperator.sol#L155
  ) external view override onlyPool returns (uint16 fee) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/AdaptiveFee.sol#L29
  ) internal pure returns (uint16 fee) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L154
  ) private view returns (Timepoint storage beforeOrAt, Timepoint storage atOrAfter) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L213
  ) internal view returns (Timepoint memory targetTimepoint) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L332
  ) internal view returns (uint88 volatilityAverage, uint256 volumePerLiqAverage) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/DataStorage.sol#L391
  ) internal returns (uint16 indexUpdated) {



https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L25
  ) internal pure returns (uint160 resultPrice) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L41
  ) internal pure returns (uint160 resultPrice) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/PriceMovementMath.sol#L51
  ) internal pure returns (uint160 resultPrice) {


https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickManager.sol#L137
  ) internal returns (int128 liquidityDelta) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L46
  function getMostSignificantBit(uint256 word) internal pure returns (uint8 mostBitPos) {

https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/libraries/TickTable.sol#L72
  ) internal view returns (int24 nextTick, bool initialized) {





### Mitigation
Remove return or returns when both used

## abi.encode() is less gas efficient than abi.encodePacked()
### Summary
In general, abi.encodePacked is more gas-efficient.

### Details
Changing the abi.encode function to abi.encodePacked can save gas since the abi.encode function pads extra null bytes at the end of the call data, which is unnecessary. Also, in general, abi.encodePacked is more gas-efficient.
### Github Permalinks
https://github.com/code-423n4/2022-09-quickswap/blob/2ead456d3603d8a4d839cf88f1e41c102b5d040f/src/core/contracts/AlgebraPoolDeployer.sol#L51
### Mitigation
Consider changing abi.encode to abi.encodePacked


