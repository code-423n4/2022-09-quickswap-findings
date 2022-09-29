## Use Two-Step Transfer Pattern for Access Controls


There is 1 instance of this issue:
 ================= 
file: AlgebraFactory.sol  

### Description

ERC20 operations can be unsafe due to different implementations and
vulnerabilities in the standard.

It is therefore recommended to always either use OpenZeppelin's `SafeERC20` library or at t to wrap each operation in a `require` statement.

To circumvent ERC20's `approve` functions race-condition vulnerability use
OpenZeppelin's `SafeERC20` library's `safe{Increase|Decrease}Allowance`
functions.

In case the vulnerability is of no danger for your implementation, provide
enough documentation explaining the reasonings.



function setOwner(address _owner) external override onlyOwner {require(owner != _owner);
    emit Owner(_owner);
    owner = _owner;
  } 

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L77 

### Should be:   

address owner;
address pendingOwner;

// ...

function setPendingOwner(address newPendingOwner) external {
    require(msg.sender == owner, "!owner");
    emit NewPendingOwner(newPendingOwner);
    pendingOwner = newPendingOwner;
}

function acceptOwnership() external {
    require(msg.sender == pendingOwner, "!pendingOwner");
    emit NewOwner(pendingOwner);
    owner = pendingOwner;
    pendingOwner = address(0);
}

 =================  

## Use at least Solidity version 0.8.0 or use safeMath for lower one

=================

## REQUIRE()/REVERT() STATEMENTS SHOULD HAVE DESCRIPTIVE REASON STRINGS  

There are 10 instances of this issue:


### file: AlgebraFactory.sol


  1. require(tokenA != tokenB);
    (address token0, address token1) = tokenA < tokenB ? (tokenA, tokenB) : (tokenB, tokenA);
  2.require(token0 != address(0));
  3.require(poolByPair[token0][token1] == address(0));    
  4.require(farmingAddress != _farmingAddress);    
  5.require(vaultAddress != _vaultAddress);

1. https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L60 
2. https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L62 
3. https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L63
  4. https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L85 
5. https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L92 

### Should be:   

  1.require(tokenA != tokenB, “REVERT MESSAGE”);
    (address token0, address token1) = tokenA < tokenB ? (tokenA, tokenB) : (tokenB, tokenA);
  2.require(token0 != address(0), “REVERT MESSAGE”);
  3.require(poolByPair[token0][token1] == address(0), “REVERT MESSAGE”);  
  4.require(farmingAddress != _farmingAddress, “REVERT MESSAGE”);  
  5.require(vaultAddress != _vaultAddress, “REVERT MESSAGE”);    

=================

### file:AlgebraPool.sol   


1.require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));  

2.require(msg.sender == IAlgebraFactory(factory).farmingAddress());  


3.require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);  



1. https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L953 
2. https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L960 
3. https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L968

 ### Should be:  

1.require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE), “REVERT MESSAGE”);  
2.require(msg.sender == IAlgebraFactory(factory).farmingAddress(), “REVERT MESSAGE”);  
3.require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown, “REVERT MESSAGE”);  

=================


### file: AlgebraPoolDeployer.sol  

1. require(_factory != address(0));
2. require(factory == address(0));  


 https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L37 

 https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L38  



 ### Should be:  

1. require(_factory != address(0), “REVERT MESSAGE”);
2. require(factory == address(0), “REVERT MESSAGE”);   

==================  
  