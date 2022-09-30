## `emit` SHOULD STAY AT END OF THE FUNCTION
This will prevent services that watch the event log from falling victim to re-entrance style attacks.
Using emit at end makes transaction event logs produced chronologically sensible. 
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L79
`
77: function setOwner(address _owner) external override onlyOwner {
78: require(owner != _owner);
79: emit Owner(_owner);
80: owner = _owner;
  }
`
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L86
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L93

## NO ERROR MESSAGE PROVIDED
In require there is no error message function will revert if required condition is not fulfilled without any error.
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L43
`require(msg.sender == owner);`
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L62
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L92

Recommendation
use appropriate error in require so that user will now the cause of revert.

## `interfaces/IAlgebraPoolDeployer.sol` IS NEVER USED
In `AlgebraPool` contract `IAlgebraPoolDeployer.sol` is never used it can be removed. Unless there is a plan to use it in the future. 