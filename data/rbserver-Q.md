## [L-01] `AlgebraFactory` CONTRACT'S `owner` CAN BE SET TO WRONG ADDRESS
When the following `setOwner` function in the `AlgebraFactory` contract is called, `require(owner != _owner)` is executed. However, this does not prevent setting `owner` to a wrong address. To avoid locking `owner`, a two-step procedure for transferring the `AlgebraFactory` contract's ownership can be used instead of directly setting `owner` through calling `setOwner`.

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L77-L81
```solidity
  function setOwner(address _owner) external override onlyOwner {
    require(owner != _owner);
    emit Owner(_owner);
    owner = _owner;
  }
```

## [L-02] CALLING `flash` FUNCTION WITH `amount0` AND `amount1` INPUTS BOTH BEING 0 FOR MANY TIMES CAN CAUSE EVENT LOG POISONING
An attacker can call the following `flash` function with the `amount0` and `amount1` inputs both being 0 for many times to emit useless `Flash` events. This causes event log poisoning in which the potential monitor systems that consume the `Flash` event can be confused and spammed. To prevent this issue, please consider requiring that `amount0` and `amount1` cannot all equal 0 in this function before executing the current function body code.

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L891-L949
```solidity
  function flash(
    address recipient,
    uint256 amount0,
    uint256 amount1,
    bytes calldata data
  ) external override lock {
    uint128 _liquidity = liquidity;
    require(_liquidity > 0, 'L');

    uint16 _fee = globalState.fee;

    uint256 fee0;
    uint256 balance0Before = balanceToken0();
    if (amount0 > 0) {
      fee0 = FullMath.mulDivRoundingUp(amount0, _fee, 1e6);
      TransferHelper.safeTransfer(token0, recipient, amount0);
    }

    uint256 fee1;
    uint256 balance1Before = balanceToken1();
    if (amount1 > 0) {
      fee1 = FullMath.mulDivRoundingUp(amount1, _fee, 1e6);
      TransferHelper.safeTransfer(token1, recipient, amount1);
    }

    IAlgebraFlashCallback(msg.sender).algebraFlashCallback(fee0, fee1, data);

    address vault = IAlgebraFactory(factory).vaultAddress();

    uint256 paid0 = balanceToken0();
    require(balance0Before.add(fee0) <= paid0, 'F0');
    paid0 -= balance0Before;

    if (paid0 > 0) {
      uint8 _communityFeeToken0 = globalState.communityFeeToken0;
      uint256 fees0;
      if (_communityFeeToken0 > 0) {
        fees0 = (paid0 * _communityFeeToken0) / Constants.COMMUNITY_FEE_DENOMINATOR;
        TransferHelper.safeTransfer(token0, vault, fees0);
      }
      totalFeeGrowth0Token += FullMath.mulDiv(paid0 - fees0, Constants.Q128, _liquidity);
    }

    uint256 paid1 = balanceToken1();
    require(balance1Before.add(fee1) <= paid1, 'F1');
    paid1 -= balance1Before;

    if (paid1 > 0) {
      uint8 _communityFeeToken1 = globalState.communityFeeToken1;
      uint256 fees1;
      if (_communityFeeToken1 > 0) {
        fees1 = (paid1 * _communityFeeToken1) / Constants.COMMUNITY_FEE_DENOMINATOR;
        TransferHelper.safeTransfer(token1, vault, fees1);
      }
      totalFeeGrowth1Token += FullMath.mulDiv(paid1 - fees1, Constants.Q128, _liquidity);
    }

    emit Flash(msg.sender, recipient, amount0, amount1, paid0, paid1);
  }
```

## [L-03] MISSING `address(0)` CHECKS FOR CRITICAL ADDRESS INPUTS
To prevent unintended behaviors, critical address inputs should be checked against `address(0)`.

Please consider checking `_poolDeployer` and `_vaultAddress` in the following constructor.
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L50-L56
```solidity
  constructor(address _poolDeployer, address _vaultAddress) {
    owner = msg.sender;
    emit Owner(msg.sender);

    poolDeployer = _poolDeployer;
    vaultAddress = _vaultAddress;
  }
```

Please consider checking `_owner` in the following function.
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L77-L81
```solidity
  function setOwner(address _owner) external override onlyOwner {
    require(owner != _owner);
    emit Owner(_owner);
    owner = _owner;
  }
```

Please consider checking `_farmingAddress` in the following function.
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L84-L88
```solidity
  function setFarmingAddress(address _farmingAddress) external override onlyOwner {
    require(farmingAddress != _farmingAddress);
    emit FarmingAddress(_farmingAddress);
    farmingAddress = _farmingAddress;
  }
```

Please consider checking `_vaultAddress` in the following function.
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L91-L95
```solidity
  function setVaultAddress(address _vaultAddress) external override onlyOwner {
    require(vaultAddress != _vaultAddress);
    emit VaultAddress(_vaultAddress);
    vaultAddress = _vaultAddress;
  }
```

Please consider checking `virtualPoolAddress` in the following function.
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L959-L964
```solidity
  function setIncentive(address virtualPoolAddress) external override {
    require(msg.sender == IAlgebraFactory(factory).farmingAddress());
    activeIncentive = virtualPoolAddress;

    emit Incentive(virtualPoolAddress);
  }
```

Please consider checking `_pool` in the following constructor.
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L31-L34
```solidity
  constructor(address _pool) {
    factory = msg.sender;
    pool = _pool;
  }
```

## [N-01] SOME FUNCTIONS' INPUT VARIABLE NAMES IN `AlgebraPool` CONTRACT DO NOT MATCH THESE IN `IAlgebraPoolActions` INTERFACE
Some functions' input variable names in the `AlgebraPool` contract do not match these in the `IAlgebraPoolActions` interface. For better readability and maintainability, please consider using the same variable names for these function inputs in both the contract and interface.

`liquidityDesired` in the contract is named as `amount` in the interface for the following `mint` function.

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L416-L423
```solidity
  function mint(
    address sender,
    address recipient,
    int24 bottomTick,
    int24 topTick,
    uint128 liquidityDesired,
    bytes calldata data
  )
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/interfaces/pool/IAlgebraPoolActions.sol#L30-L37
```solidity
  function mint(
    address sender,
    address recipient,
    int24 bottomTick,
    int24 topTick,
    uint128 amount,
    bytes calldata data
  )
```

`amountRequired` in the contract is named as `amountSpecified` in the interface for the following `swap` function.

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L589-L595
```solidity
  function swap(
    address recipient,
    bool zeroToOne,
    int256 amountRequired,
    uint160 limitSqrtPrice,
    bytes calldata data
  ) external override returns (int256 amount0, int256 amount1) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/interfaces/pool/IAlgebraPoolActions.sol#L96-L102
```solidity
  function swap(
    address recipient,
    bool zeroToOne,
    int256 amountSpecified,
    uint160 limitSqrtPrice,
    bytes calldata data
  ) external returns (int256 amount0, int256 amount1);
```

`amountRequired` in the contract is named as `amountSpecified` in the interface for the following `swapSupportingFeeOnInputTokens` function.

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L626-L633
```solidity
  function swapSupportingFeeOnInputTokens(
    address sender,
    address recipient,
    bool zeroToOne,
    int256 amountRequired,
    uint160 limitSqrtPrice,
    bytes calldata data
  ) external override returns (int256 amount0, int256 amount1) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/interfaces/pool/IAlgebraPoolActions.sol#L118-L125
```solidity
  function swapSupportingFeeOnInputTokens(
    address sender,
    address recipient,
    bool zeroToOne,
    int256 amountSpecified,
    uint160 limitSqrtPrice,
    bytes calldata data
  ) external returns (int256 amount0, int256 amount1);
```

## [N-02] SOME `require` STATEMENTS DO NOT HAVE REASON STRINGS
Some `require` statements do not have reason strings. To be more descriptive, please consider adding reason strings for the following `require` statements.
```solidity
core\contracts\AlgebraFactory.sol
  60: require(tokenA != tokenB); 
  78: require(owner != _owner); 
  85: require(farmingAddress != _farmingAddress); 
  92: require(vaultAddress != _vaultAddress); 

core\contracts\AlgebraPool.sol
  55: require(msg.sender == IAlgebraFactory(factory).owner()); 
  122: require(_lower.initialized); 
  134: require(_upper.initialized); 
  229: require((_blockTimestamp() - lastLiquidityAddTimestamp) >= _liquidityCooldown); 
  953: require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE)); 
  960: require(msg.sender == IAlgebraFactory(factory).farmingAddress()); 
  968: require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown); 

core\contracts\libraries\DataStorage.sol
  369: require(!self[0].initialized);
```

## [N-03] CONSTANTS CAN BE USED INSTEAD OF MAGIC NUMBERS
To improve readability and maintainability, constants can be used instead of magic numbers. Please consider replacing `1e6` in the following code with constants.

```solidity
core\contracts\AlgebraPool.sol
  905: fee0 = FullMath.mulDivRoundingUp(amount0, _fee, 1e6); 
  912: fee1 = FullMath.mulDivRoundingUp(amount1, _fee, 1e6); 

core\contracts\libraries\PriceMovementMath.sol
  157: uint256 amountAvailableAfterFee = FullMath.mulDiv(uint256(amountAvailable), 1e6 - fee, 1e6); 
  161: feeAmount = FullMath.mulDivRoundingUp(input, fee, 1e6 - fee); 
  170: feeAmount = FullMath.mulDivRoundingUp(input, fee, 1e6 - fee); 
  195: feeAmount = FullMath.mulDivRoundingUp(input, fee, 1e6 - fee); 
```

## [N-04] MISSING NATSPEC COMMENTS
NatSpec comments provide rich code documentation. NatSpec comments are missing for the following functions. Please consider adding them.

```solidity
core\contracts\AlgebraPool.sol
  70: function balanceToken0() private view returns (uint256) { 
  74: function balanceToken1() private view returns (uint256) { 
  366: function _getAmountsForLiquidity( 
  546: function _payCommunityFee(address token, uint256 amount) private {
  551: function _writeTimepoint( 
  561: function _getSingleTimepoint( 
  580: function _swapCallback(
```

## [N-05] INCOMPLETE NATSPEC COMMENTS
NatSpec comments provide rich code documentation. @param and/or @return comments are missing for the following functions. Please consider completing NatSpec comments for them.

```solidity
core\contracts\AlgebraPool.sol
  274: function _updatePositionTicksAndFees(
  536: function _getNewFee(
  703: function _calculateSwapAndLock(
```

## [N-06] OLD SOLIDITY VERSION IS USED
Using a newer version of Solidity can benefit from new features and bug fixes. Please consider using a newer Solidity version for the following contracts.
```solidity
core\contracts\AlgebraFactory.sol
  2: pragma solidity =0.7.6;

core\contracts\AlgebraPool.sol
  2: pragma solidity =0.7.6;

core\contracts\AlgebraPoolDeployer.sol
  2: pragma solidity =0.7.6;

core\contracts\DataStorageOperator.sol
  2: pragma solidity =0.7.6;

core\contracts\base\PoolImmutables.sol
  2: pragma solidity =0.7.6;

core\contracts\base\PoolState.sol
  2: pragma solidity =0.7.6;
```