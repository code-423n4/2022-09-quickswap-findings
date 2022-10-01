
## Use internal for constants instead of public

Marking variables as internal saves gas on deployment as the compiler does not have to create getter functions for these variables. Variables can still be read from the verified source code or the bytecode

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L12

```
  uint32 public constant WINDOW = 1 days;
```

## Array length can be cached to save gas in for-loops

Reading array length in each iteration consumes more gas than necessary. Cache array length in a variable and use it in the for-loop to save gas

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L307

```
    for (uint256 i = 0; i < secondsAgos.length; i++) 
```

## Use msg.sender instead of `owner` in functions that use onlyOwner modifier

In functions that use onlyOwner modifier, using msg.sender instead of reading owner variable from storage reduces gas

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L78

```
  function setOwner(address _owner) external override onlyOwner { 
    require(owner != _owner); 
    emit Owner(_owner);
    owner = _owner;
  }
```

## Switching from 1 to 2 is more efficient than false to true 

SSTORE from false to true costs more than SSTORE from non-zero to another non-zero value, and boolean varialbes costs more gas than uint256 since additional conversion operations has to be performed

Replace boolean with uint256

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolState.sol#L40-L45

```
  modifier lock() { 
    require(globalState.unlocked, 'LOK');
    globalState.unlocked = false;
    _;
    globalState.unlocked = true;
  }

```

## Cache external calls

Repeated external calls can be stored in a variable and re-used to save gas

`_blockTimestamp()`

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L237

```
    liquidityNext > 0 ? (liquidityDelta > 0 ? _blockTimestamp() : lastLiquidityAddTimestamp) : 0
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L357

```
    globalState.fee = _getNewFee(_blockTimestamp(), cache.tick, newTimepointIndex, liquidityBefore); 
```

## Split require statements to save gas

Require statements including conditions with && operator can be split into multiple require statements to save gas

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L968

```
    require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown); 
```

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L46

```
    require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0'); 
```