### Remove test function

Remove the test functions such as this found within poolState.sol line47 before deployment to live, so that there cannot be attempts by unsavoury characters to find bugs within the code. or at least remove @dev comments that highlight functions written to test mechanisms so that an attack not attempted which could have larger impacts on the project if missed before deployment (noted I did not find any other such functions).

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolState.sol#L47


  /// @dev Reentrancy protection. Implemented in every function of the contract since there are checks of balances.
  modifier lock() {
    require(globalState.unlocked, 'LOK');
    globalState.unlocked = false;
    _;
    globalState.unlocked = true;
  }

  /// @dev This function is created for testing by overriding it.
  /// @return A timestamp converted to uint32
  function _blockTimestamp() internal view virtual returns (uint32) {
    return uint32(block.timestamp); // truncation is desired
  }
}
