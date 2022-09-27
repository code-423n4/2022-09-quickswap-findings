## Use Custom Errors Instead of Require to Save Gas
Consider replacing all require statements with custom errors which are cheaper both in deployment and runtime cost starting from Solidity 0.8.4. Here are some of the instances entailed:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L109

## Splitting Require Statements Using && Saves Gas
Instead of using the && operator in a single require statement to check multiple conditions, using multiple require statements with 1 condition per require statement will save 3 GAS per &&. Here are some pf the instances entailed:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L110

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


