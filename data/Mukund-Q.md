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
