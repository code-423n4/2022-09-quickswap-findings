

### [L01] require() should be used instead of assert()


#### Findings:
```
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::190 => assert(false);
```


### [L02] Missing checks for `address(0x0)` when assigning values to `address` state variables


#### Findings:
```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::54 => poolDeployer = _poolDeployer;
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::55 => vaultAddress = _vaultAddress;
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::78 => require(owner != _owner);
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::80 => owner = _owner;
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::85 => require(farmingAddress != _farmingAddress);
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::87 => farmingAddress = _farmingAddress;
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::92 => require(vaultAddress != _vaultAddress);
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::94 => vaultAddress = _vaultAddress;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::750 => blockTimestamp = _blockTimestamp();
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::33 => pool = _pool;
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::48 => feeConfig = _feeConfig;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::229 => prevLast.blockTimestamp = _prevLast.blockTimestamp;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::230 => prevLast.tickCumulative = _prevLast.tickCumulative;
```



### [L03] `initialize` functions can be front-run

#### Impact
See [this](https://github.com/code-423n4/2021-10-badgerdao-findings/issues/40) finding from a prior badger-dao contest for details
#### Findings:
```
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::193 => function initialize(uint160 initialPrice) external override {
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::199 => IDataStorageOperator(dataStorageOperator).initialize(timestamp, tick);
```


### [L04] `abi.encodePacked()` should not be used with dynamic types when passing the result to a hash function such as `keccak256()`

#### Impact
Use abi.encode() instead which will pad items to 32 bytes, which will prevent hash collisions (e.g. abi.encodePacked(0x123,0x456) => 0x123456 => abi.encodePacked(0x1,0x23456), but abi.encode(0x123,0x456) => 0x0...1230...456). “Unless there is a compelling reason, abi.encode should be preferred”. If there is only one argument to abi.encodePacked() it can often be cast to bytes() or bytes32() instead.
#### Findings:
```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::123 => pool = address(uint256(keccak256(abi.encodePacked(hex'ff', poolDeployer, keccak256(abi.encode(token0, token1)), POOL_INIT_CODE_HASH))));
```





#### Non-Critical Issues



### [N01] Adding a return statement when the function defines a named return variable, is redundant


#### Findings:
```
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::85 => return last;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::220 => return last;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::242 => return atOrAfter; // we're at the right boundary
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::262 => return beforeOrAt;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::395 => return index;
```



### [N02] `require()`/`revert()` statements should have descriptive reason strings


#### Findings:
```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::43 => require(msg.sender == owner);
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::60 => require(tokenA != tokenB);
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::62 => require(token0 != address(0));
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::63 => require(poolByPair[token0][token1] == address(0));
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::78 => require(owner != _owner);
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::85 => require(farmingAddress != _farmingAddress);
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::92 => require(vaultAddress != _vaultAddress);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::122 => require(_lower.initialized);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::134 => require(_upper.initialized);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::953 => require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::960 => require(msg.sender == IAlgebraFactory(factory).farmingAddress());
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::968 => require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
2022-09-quickswap/src/core/contracts/AlgebraPoolDeployer.sol::22 => require(msg.sender == factory);
2022-09-quickswap/src/core/contracts/AlgebraPoolDeployer.sol::27 => require(msg.sender == owner);
2022-09-quickswap/src/core/contracts/AlgebraPoolDeployer.sol::37 => require(_factory != address(0));
2022-09-quickswap/src/core/contracts/AlgebraPoolDeployer.sol::38 => require(factory == address(0));
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::43 => require(msg.sender == factory || msg.sender == IAlgebraFactory(factory).owner());
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::369 => require(!self[0].initialized);
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::52 => require(price > 0);
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::53 => require(liquidity > 0);
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::70 => require((product = amount * price) / amount == price); // if the product overflows, we know the denominator underflows
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::71 => require(liquidityShifted > product); // in addition, we must check that the denominator does not underflow
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::87 => require(price > quotient);
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::30 => require(priceDelta < priceUpper); // forbids underflow and 0 priceLower
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::51 => require(priceUpper >= priceLower);
```



### [N03] constants should be defined rather than using magic numbers


#### Findings:
```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::32 => 15000 - 3000, // alpha2
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::36 => 8500, // gamma2
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::247 => fees0 = uint128(FullMath.mulDiv(innerFeeGrowth0Token - _innerFeeGrowth0Token, currentLiquidity, Constants.Q128));
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::249 => uint128 fees1;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::252 => fees1 = uint128(FullMath.mulDiv(innerFeeGrowth1Token - _innerFeeGrowth1Token, currentLiquidity, Constants.Q128));
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::342 => _recalculatePosition(position, liquidityDelta, feeGrowthInside0X128, feeGrowthInside1X128);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::814 => if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::931 => totalFeeGrowth0Token += FullMath.mulDiv(paid0 - fees0, Constants.Q128, _liquidity);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::942 => fees1 = (paid1 * _communityFeeToken1) / Constants.COMMUNITY_FEE_DENOMINATOR;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::945 => totalFeeGrowth1Token += FullMath.mulDiv(paid1 - fees1, Constants.Q128, _liquidity);
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::16 => uint128 constant MAX_VOLUME_PER_LIQUIDITY = 100000 << 64; // maximum meaningful ratio of volume to liquidity
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::138 => if (volume >= 2**192) volumeShifted = (type(uint256).max) / (liquidity > 0 ? liquidity : 1);
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::52 => if (x >= 6 * uint256(g)) return alpha; // so x < 19 bits
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::53 => uint256 g8 = uint256(g)**8; // < 128 bits (8*16)
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::55 => res = (alpha * ex) / (g8 + ex); // in worst case: (16 + 155 bits) / 155 bits
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::59 => if (x >= 6 * uint256(g)) return 0; // so x < 19 bits
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::60 => uint256 g8 = uint256(g)**8; // < 128 bits (8*16)
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::62 => res = (alpha * g8) / ex; // in worst case: (16 + 128 bits) / 156 bits
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::81 => res = gHighestDegree; // g**8
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::83 => gHighestDegree /= g; // g**7
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::84 => res += xLowestDegree * gHighestDegree;
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::86 => gHighestDegree /= g; // g**6
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::87 => xLowestDegree *= x; // x**2
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::88 => res += (xLowestDegree * gHighestDegree) / 2;
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::90 => gHighestDegree /= g; // g**5
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::91 => xLowestDegree *= x; // x**3
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::92 => res += (xLowestDegree * gHighestDegree) / 6;
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::94 => gHighestDegree /= g; // g**4
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::95 => xLowestDegree *= x; // x**4
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::96 => res += (xLowestDegree * gHighestDegree) / 24;
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::98 => gHighestDegree /= g; // g**3
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::99 => xLowestDegree *= x; // x**5
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::100 => res += (xLowestDegree * gHighestDegree) / 120;
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::102 => gHighestDegree /= g; // g**2
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::103 => xLowestDegree *= x; // x**6
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::104 => res += (xLowestDegree * gHighestDegree) / 720;
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::106 => xLowestDegree *= x; // x**7
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::107 => res += (xLowestDegree * g) / 5040 + (xLowestDegree * x) / (40320);
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::6 => uint256 internal constant Q96 = 0x1000000000000000000000000;
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::7 => uint256 internal constant Q128 = 0x100000000000000000000000000000000;
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::13 => uint128 internal constant MAX_LIQUIDITY_PER_TICK = 11505743598341114571880798222544994;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::49 => int256 K = (tick1 - tick0) - (avgTick1 - avgTick0); // (k - p)*dt
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::50 => int256 B = (tick0 - avgTick0) * dt; // (b - q)*dt
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::51 => int256 sumOfSquares = (dt * (dt + 1) * (2 * dt + 1)); // sumOfSquares * 6
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::52 => int256 sumOfSequence = (dt * (dt + 1)); // sumOfSequence * 2
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::53 => volatility = uint256((K**2 * sumOfSquares + 6 * B * K * sumOfSequence + 6 * dt * B**2) / (6 * dt**2));
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::80 => last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::33 => singleBitPos := or(singleBitPos, shl(7, iszero(and(word, 0x00000000000000000000000000000000FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF))))
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::34 => singleBitPos := or(singleBitPos, shl(6, iszero(and(word, 0x0000000000000000FFFFFFFFFFFFFFFF0000000000000000FFFFFFFFFFFFFFFF))))
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::55 => word := or(word, shr(128, word))
```



### [N04] Use bit shifts in an imutable variable rather than long bit masks of a single bit, for readability


#### Findings:
```
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::7 => uint256 internal constant Q128 = 0x100000000000000000000000000000000;
```



### [N05] Use a more recent version of solidity

#### Impact
Use a solidity version of at least 0.8.12 to get string.concat() to be used instead of abi.encodePacked(<str>,<str>)
#### Findings:
```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/AlgebraPoolDeployer.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/base/PoolImmutables.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/base/PoolState.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::2 => pragma solidity =0.7.6;
```

### [N06] File is missing NatSpec


#### Findings:
```
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
```



### [N07] Use a more recent version of solidity

#### Impact
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions
#### Findings:
```
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::33 => using LowGasSafeMath for uint256;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::34 => using LowGasSafeMath for int256;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::35 => using LowGasSafeMath for uint128;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::36 => using SafeCast for uint256;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::37 => using SafeCast for int256;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::38 => using TickTable for mapping(int16 => uint256);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::39 => using TickManager for mapping(int24 => TickManager.Tick);
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::18 => using DataStorage for DataStorage.Timepoint[UINT16_MODULO];
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::10 => using LowGasSafeMath for uint256;
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::11 => using SafeCast for uint256;
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::13 => using LowGasSafeMath for int256;
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::14 => using SafeCast for int256;
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::13 => using LowGasSafeMath for uint256;
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::14 => using SafeCast for uint256;
```


### [N08] Use of sensitive/NC-inclusive terms


#### Findings:
```
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::293 => bool toggledBottom;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::294 => bool toggledTop;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::680 => bool computedLatestTimepoint; //  if we have already fetched _tickCumulative_ and _secondPerLiquidity_ from the DataOperator
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::686 => bool exactInput; // Whether the exact input or output is specified
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::695 => bool initialized; // True if the _nextTick is initialized
2022-09-quickswap/src/core/contracts/base/PoolState.sol::15 => bool unlocked; // True if the contract is unlocked, otherwise - false
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::15 => bool initialized; // whether or not the timepoint is initialized
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::27 => bool initialized; // these 8 bits are set to prevent fresh sstores when crossing newly initialized ticks
```


### [N09] `2**<n> - 1` should be re-written as `type(uint<n>).max`

#### Impact
Earlier versions of solidity can use uint<n>(-1) instead. Expressions not including the - 1 can often be re-written to accomodate the change (e.g. by using a > rather than a >=, which will also save some gas)
#### Findings:
```
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::138 => if (volume >= 2**192) volumeShifted = (type(uint256).max) / (liquidity > 0 ? liquidity : 1);
```



### [N10] Numeric values having to do with time should use time units for readability

#### Impact
There are [units](https://docs.soliditylang.org/en/latest/units-and-global-variables.html#time-units) for seconds, minutes, hours, days, and weeks
#### Findings:
```
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::12 => uint16 alpha2; // max value of the second sigmoid
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::14 => uint32 beta2; // shift along the x-axis for the second sigmoid
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::16 => uint16 gamma2; // horizontal stretch factor for the second sigmoid
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::15 => uint32 internal constant MAX_LIQUIDITY_COOLDOWN = 1 days;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::12 => uint32 public constant WINDOW = 1 days;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::18 => uint160 secondsPerLiquidityCumulative; // the seconds per liquidity since the pool was first initialized
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::25 => uint160 outerSecondsPerLiquidity; // the seconds per unit of liquidity on the _other_ side of current tick, (relative meaning)
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::26 => uint32 outerSecondsSpent; // the seconds spent on the other side of the current tick, only has relative meaning
```




