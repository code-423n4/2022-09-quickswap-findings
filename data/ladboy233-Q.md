## Solidity version is not update-to-date

the contract uses the solidity version 0.7.6

Use a solidity version of at least 0.8.0 to get overflow protection without SafeMath Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value


## require statement does not have reason of failure

User will not know which steps goes wrong when transaction reverted.


https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#:43


```
    require(msg.sender == owner);
```
            

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#:60


```
    require(tokenA != tokenB);
```
            

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#:62


```
    require(token0 != address(0));
```
            

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#:63


```
    require(poolByPair[token0][token1] == address(0));
```
            

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#:78


```
    require(owner != _owner);
```
            

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#:85


```
    require(farmingAddress != _farmingAddress);
```
            

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#:92


```
    require(vaultAddress != _vaultAddress);
```
            

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#:55


```
    require(msg.sender == IAlgebraFactory(factory).owner());
```
            

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#:122


```
      require(_lower.initialized);
```
            

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#:134


```
      require(_upper.initialized);
```
            

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#:229


```
          require((_blockTimestamp() - lastLiquidityAddTimestamp) >= _liquidityCooldown);
```
            

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#:953


```
    require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));
```
            

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#:960


```
    require(msg.sender == IAlgebraFactory(factory).farmingAddress());
```
            

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#:968


```
    require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
```
            

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#:22


```
    require(msg.sender == factory);
```
            

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#:27


```
    require(msg.sender == owner);
```
            

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#:37


```
    require(_factory != address(0));
```
            

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#:38


```
    require(factory == address(0));
```
            

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#:43


```
    require(msg.sender == factory || msg.sender == IAlgebraFactory(factory).owner());
```
            
## nested function make it diffucult to read and reason about the contract

there is nested function, we recommand move the internal function out and create a seperate function

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol/#L153

```
    returns (
      uint160 resultPrice,
      uint256 input,
      uint256 output,
      uint256 feeAmount
    )
  {
    function(uint160, uint160, uint128) pure returns (uint256) getAmountA = zeroToOne ? getTokenADelta01 : getTokenADelta10;

```