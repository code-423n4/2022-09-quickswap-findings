## Use Custom Errors Instead of Require to Save Gas
Consider using Solidity 0.8.4 and above and replacing all require statements with custom errors which are cheaper both in deployment and runtime cost. Here are some of the instances entailed:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L109
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L935

## Splitting Require Statements Using && Saves Gas
Instead of using the && operator in a single require statement to check multiple conditions, using multiple require statements with 1 condition per require statement will save 3 GAS per &&. Here are some pf the instances entailed:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L110
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L953

## Function Order Affects Gas Consumption
The order of function will also have an impact on gas consumption. Because in smart contracts, there is a difference in the order of the functions. Each position will have an extra 22 gas. The order is dependent on method ID. So, if you rename the frequently accessed function to more early method ID, you can save gas cost. Please visit the following site for further information:

https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92

## Activate the Optimizer
Before deploying your contract, activate the optimizer when compiling using “solc --optimize --bin sourceFile.sol”. By default, the optimizer will optimize the contract assuming it is called 200 times across its lifetime. If you want the initial contract deployment to be cheaper and the later function executions to be more expensive, set it to “ --optimize-runs=1”. Conversely, if you expect many transactions and do not care for higher deployment cost and output size, set “--optimize-runs” to a high number.

```
module.exports = {
  solidity: {
    version: "0.7.6",
    settings: {
      optimizer: {
        enabled: true,
        runs: 1000,
      },
    },
  },
};
```
Please visit the following site for further information:

https://docs.soliditylang.org/en/v0.5.4/using-the-compiler.html#using-the-commandline-compiler

Here's one example of instance on opcode comparison that delineates the gas saving mechanism:
 
for !=0 before optimization
PUSH1 0x00
DUP2
EQ
ISZERO
PUSH1 [cont offset]
JUMPI 

after optimization
DUP1
PUSH1 [revert offset]
JUMPI

## Private Function Embedded Modifier to Reduce Contract Size
Consider having the logic of a modifier embedded through an internal or private function to reduce contract size if need be. For instance, the following instance of modifier may be rewritten as follows:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L42-L45

```
    function _onlyOwner() private view {
        require(msg.sender == owner);
    }

    modifier onlyOwner() {
        _onlyOwner();
        _;
    }
```
## Non-strict inequalities are cheaper than strict ones
In the EVM, there is no opcode for non-strict inequalities (>=, <=) and two operations are performed (> + = or < + =). As an example, consider replacing >= with the strict counterpart > in the following line of code:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L229

```
          require((_blockTimestamp() - lastLiquidityAddTimestamp) > _liquidityCooldown - 1);
```
Similarly, as an example, consider replacing <= with the strict counterpart < in the following line of code:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L608

```
      require(balance0Before.add(uint256(amount0)) < balanceToken0() + 1, 'IIA');
```
## += Costs More Gas
`+=` generally costs more gas than writing out the assigned equation explicitly. As an example, the following line of code could be rewritten as:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L811

```
        communityFeeAmount = communityFeeAmount + delta;
```
Similarly, as an example, the following line of code could be rewritten as:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L936

```
    paid1 = paid1 - balance1Before;
```
## calldata and memory
When running a function we could pass the function parameters as calldata or memory for variables such as strings, bytes, structs, arrays etc. If we are not modifying the passed parameter we should pass it as calldata because calldata is more gas efficient than memory. Here is one of the instances entailed:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L90

## ++i costs less gas compared to i++
++i costs less gas compared to i++ or i += 1 for unsigned integers considering the pre-increment operation is cheaper (about 5 GAS per iteration).

i++ increments i and makes the compiler create a temporary variable for returning the initial value of i. In contrast, ++i returns the actual incremented value without making the compiler do extra job.

As an example, the for loop below could be refactored as follows:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307-L315

```
    for (uint256 i = 0; i < secondsAgos.length;) {
      current = getSingleTimepoint(self, time, secondsAgos[i], tick, index, oldestIndex, liquidity);
      (tickCumulatives[i], secondsPerLiquidityCumulatives[i], volatilityCumulatives[i], volumePerAvgLiquiditys[i]) = (
        current.tickCumulative,
        current.secondsPerLiquidityCumulative,
        current.volatilityCumulative,
        current.volumePerLiquidityCumulative
      );

      // Unchecked math if using Solidity 0.8.0 and above
      unchecked {
              ++i;
          }
    }
```
Note: If using Solidity ^0.8.0 (or using `SafeMath` in lower version of Solidity), the default "checked" math is not free. The compiler will add some overflow checks, somehow similar to those implemented by `SafeMath`. While it is reasonable to expect these checks to be less expensive than the current `SafeMath`, one should keep in mind that these checks will increase the cost of "basic math operation" that were not previously covered. This particularly concerns variable increments in for loops. Considering no arithmetic overflow/underflow is going to happen here, `unchecked { ++i ;}` to use the previous wrapping behavior further saves gas in the above for loop.

## No Need to Initialize Variables with Default Values
If a variable is not set/initialized, it is assumed to have the default value (0, false, 0x0 etc depending on the data type). If you explicitly initialize it with its default value, you will be incurring more gas. Here is one of the instances entailed:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307

## > 0 Is More Expensive Than != 0 for Unsigned Integers
!= 0 costs 6 less GAS compared to > 0 for unsigned integers in conditional statements with the optimizer enabled. Here are some of the instances entailed:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L451-L452
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L454-L455

## Using Booleans Costs More Storage Overhead
According to Openzeppelin's `ReentrancyGuard.sol`:

https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L35

    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.

    // The values being non-zero value makes deployment a bit more expensive,
    // but in exchange the refund on every call to nonReentrant will be lower in
    // amount. Since refunds are capped to a percentage of the total
    // transaction's gas, it is best to keep them low in cases like this one, to
    // increase the likelihood of the full refund coming into effect.

Consider using uint256(1) and uint256(2) for true/false to avoid:

1. repeated Gwarmaccess  that would cost 100 gas each, and
2. Gsset that would cost 20000 gas when changing from ‘false’ to ‘true’, as well as after having been ‘true’ in the past. 

Here are some of the instances entailed:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L15
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L98
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L161
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L166
