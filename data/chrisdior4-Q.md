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
  