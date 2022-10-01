## Summary

### Gas Optimizations
| |Issue|Instances|Total Gas Saved|
|-|:-|:-:|:-:|
| [G&#x2011;01] | State variables only set in the constructor should be declared `immutable` | 2 | 4194 |
| [G&#x2011;02] | Using `calldata` instead of `memory` for read-only arguments in `external` functions saves gas | 1 | 120 |
| [G&#x2011;03] | Using `storage` instead of `memory` for structs/arrays saves gas | 1 | 4200 |
| [G&#x2011;04] | Avoid contract existence checks by using low level calls | 24 | 2400 |
| [G&#x2011;05] | State variables should be cached in stack variables rather than re-reading them from storage | 7 | 679 |
| [G&#x2011;06] | `internal` functions only called once can be inlined to save gas | 2 | 40 |
| [G&#x2011;07] | `<array>.length` should not be looked up in every loop of a `for`-loop | 1 | 3 |
| [G&#x2011;08] | Use a more recent version of solidity | 13 | - |
| [G&#x2011;09] | Using `> 0` costs more gas than `!= 0` when used on a `uint` in a `require()` statement | 6 | 36 |
| [G&#x2011;10] | `>=` costs less gas than `>` | 2 | 6 |
| [G&#x2011;11] | `++i` costs less gas than `i++`, especially when it's used in `for`-loops (`--i`/`i--` too) | 1 | 5 |
| [G&#x2011;12] | Splitting `require()` statements that use `&&` saves gas | 6 | 18 |
| [G&#x2011;13] | Using `private` rather than `public` for constants, saves gas | 1 | - |
| [G&#x2011;14] | Division by two should use bit shifting | 1 | 20 |
| [G&#x2011;15] | Use custom errors rather than `revert()`/`require()` strings to save gas | 32 | - |
| [G&#x2011;16] | Functions guaranteed to revert when called by normal users can be marked `payable` | 17 | 357 |

Total: 117 instances over 16 issues with **12078 gas** saved

Gas totals use lower bounds of ranges and count two iterations of each `for`-loop. All values above are runtime, not deployment, values; deployment values are listed in the individual issue descriptions


## Gas Optimizations

### [G&#x2011;01]  State variables only set in the constructor should be declared `immutable`
Avoids a Gsset (**20000 gas**) in the constructor, and replaces the first access in each transaction (Gcoldsload - **2100 gas**) and each access thereafter (Gwarmacces - **100 gas**) with a `PUSH32` (**3 gas**). 

While `string`s are not value types, and therefore cannot be `immutable`/`constant` if not hard-coded outside of the constructor, the same behavior can be achieved by making the current contract `abstract` with `virtual` functions for the `string` accessors, and having a child contract override the functions with the hard-coded implementation-specific values.

*There are 2 instances of this issue:*
```solidity
File: src/core/contracts/AlgebraPoolDeployer.sol

/// @audit owner (access)
27:       require(msg.sender == owner);

/// @audit owner (constructor)
32:       owner = msg.sender;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L27

### [G&#x2011;02]  Using `calldata` instead of `memory` for read-only arguments in `external` functions saves gas
When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the `memory` index. **Each iteration of this for-loop costs at least 60 gas** (i.e. `60 * <mem_array>.length`). Using `calldata` directly, obliviates the need for such a loop in the contract code and runtime execution. Note that even if an interface defines a function as having `memory` arguments, it's still valid for implementation contracs to use `calldata` arguments instead. 

If the array is passed to an `internal` function which passes the array to another internal function where the array is modified and therefore `memory` is used in the `external` call, it's still more gass-efficient to use `calldata` when the `external` function uses modifiers, since the modifiers may prevent the internal functions from being called. Structs have the same overhead as an array of length one

Note that I've also flagged instances where the function is `public` but can be marked as `external` since it's not called by the contract, and cases where a constructor is involved

*There is 1 instance of this issue:*
```solidity
File: src/core/contracts/DataStorageOperator.sol

/// @audit secondsAgos
88      function getTimepoints(
89        uint32 time,
90        uint32[] memory secondsAgos,
91        int24 tick,
92        uint16 index,
93        uint128 liquidity
94      )
95        external
96        view
97        override
98        onlyPool
99        returns (
100         int56[] memory tickCumulatives,
101         uint160[] memory secondsPerLiquidityCumulatives,
102         uint112[] memory volatilityCumulatives,
103:        uint256[] memory volumePerAvgLiquiditys

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L88-L103

### [G&#x2011;03]  Using `storage` instead of `memory` for structs/arrays saves gas
When fetching data from a storage location, assigning the data to a `memory` variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (**2100 gas**) for *each* field of the struct/array. If the fields are read from the new memory variable, they incur an additional `MLOAD` rather than a cheap stack read. Instead of declearing the variable with the `memory` keyword, declaring the variable with the `storage` keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a `memory` variable, is if the full struct/array is being returned by the function, is being passed to a function that requires `memory`, or if the array/struct is being read from another `memory` array/struct

*There is 1 instance of this issue:*
```solidity
File: src/core/contracts/libraries/DataStorage.sol

397:      Timepoint memory last = _last;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L397

### [G&#x2011;04]  Avoid contract existence checks by using low level calls
Prior to 0.8.10 the compiler inserted extra code, including `EXTCODESIZE` (**100 gas**), to check for contract existence for external function calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value. Similar behavior can be achieved in earlier versions by using low-level calls, since low level calls never check for contract existence

*There are 24 instances of this issue:*
```solidity
File: src/core/contracts/AlgebraFactory.sol

/// @audit deploy()
69:       pool = IAlgebraPoolDeployer(poolDeployer).deploy(address(dataStorage), address(this), token0, token1);

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L69

```solidity
File: src/core/contracts/AlgebraPool.sol

/// @audit owner()
55:       require(msg.sender == IAlgebraFactory(factory).owner());

/// @audit balanceOf()
71:       return IERC20Minimal(token0).balanceOf(address(this));

/// @audit balanceOf()
75:       return IERC20Minimal(token1).balanceOf(address(this));

/// @audit timepoints()
93:       return IDataStorageOperator(dataStorageOperator).timepoints(index);

/// @audit getTimepoints()
183:        IDataStorageOperator(dataStorageOperator).getTimepoints(

/// @audit algebraMintCallback()
453:        IAlgebraMintCallback(msg.sender).algebraMintCallback(amount0, amount1, data);

/// @audit getFee()
542:      newFee = IDataStorageOperator(dataStorageOperator).getFee(_time, _tick, _index, _liquidity);

/// @audit vaultAddress()
547:      address vault = IAlgebraFactory(factory).vaultAddress();

/// @audit write()
558:      return IDataStorageOperator(dataStorageOperator).write(timepointIndex, blockTimestamp, tick, liquidity, volumePerLiquidityInBlock);

/// @audit getSingleTimepoint()
577:      return IDataStorageOperator(dataStorageOperator).getSingleTimepoint(blockTimestamp, secondsAgo, startTick, timepointIndex, liquidityStart);

/// @audit algebraSwapCallback()
585:      IAlgebraSwapCallback(msg.sender).algebraSwapCallback(amount0, amount1, data);

/// @audit increaseCumulative()
753:          IAlgebraVirtualPool.Status _status = IAlgebraVirtualPool(activeIncentive).increaseCumulative(blockTimestamp);

/// @audit cross()
833:              IAlgebraVirtualPool(activeIncentive).cross(step.nextTick, zeroToOne);

/// @audit calculateVolumePerLiquidity()
880:        cache.volumePerLiquidityInBlock + IDataStorageOperator(dataStorageOperator).calculateVolumePerLiquidity(currentLiquidity, amount0, amount1)

/// @audit algebraFlashCallback()
916:      IAlgebraFlashCallback(msg.sender).algebraFlashCallback(fee0, fee1, data);

/// @audit vaultAddress()
918:      address vault = IAlgebraFactory(factory).vaultAddress();

/// @audit farmingAddress()
960:      require(msg.sender == IAlgebraFactory(factory).farmingAddress());

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L55

```solidity
File: src/core/contracts/base/PoolImmutables.sol

/// @audit parameters()
30:       (dataStorageOperator, factory, token0, token1) = IAlgebraPoolDeployer(deployer).parameters();

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolImmutables.sol#L30

```solidity
File: src/core/contracts/DataStorageOperator.sol

/// @audit owner()
43:       require(msg.sender == factory || msg.sender == IAlgebraFactory(factory).owner());

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L43

```solidity
File: src/core/contracts/libraries/TokenDeltaMath.sol

/// @audit toInt256()
67:         ? getToken0Delta(priceLower, priceUpper, uint128(liquidity), true).toInt256()

/// @audit toInt256()
68:         : -getToken0Delta(priceLower, priceUpper, uint128(-liquidity), false).toInt256();

/// @audit toInt256()
82:         ? getToken1Delta(priceLower, priceUpper, uint128(liquidity), true).toInt256()

/// @audit toInt256()
83:         : -getToken1Delta(priceLower, priceUpper, uint128(-liquidity), false).toInt256();

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TokenDeltaMath.sol#L67

### [G&#x2011;05]  State variables should be cached in stack variables rather than re-reading them from storage
The instances below point to the second+ access of a state variable within a function. Caching of a state variable replaces each Gwarmaccess (**100 gas**) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

*There are 7 instances of this issue:*
```solidity
File: src/core/contracts/AlgebraPool.sol

/// @audit totalFeeGrowth0Token on line 740
/// @audit totalFeeGrowth1Token on line 744
829:              cache.totalFeeGrowthB = zeroToOne ? totalFeeGrowth1Token : totalFeeGrowth0Token;

/// @audit liquidity on line 297
354:          uint128 liquidityBefore = liquidity;

/// @audit liquidity on line 736
/// @audit volumePerLiquidityInBlock on line 736
878:      (liquidity, volumePerLiquidityInBlock) = (

/// @audit activeIncentive on line 752
753:          IAlgebraVirtualPool.Status _status = IAlgebraVirtualPool(activeIncentive).increaseCumulative(blockTimestamp);

/// @audit activeIncentive on line 755
833:              IAlgebraVirtualPool(activeIncentive).cross(step.nextTick, zeroToOne);

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L829

### [G&#x2011;06]  `internal` functions only called once can be inlined to save gas
Not inlining costs **20 to 40 gas** because of two extra `JUMP` instructions and additional stack operations needed for function calls.

*There are 2 instances of this issue:*
```solidity
File: src/core/contracts/AlgebraFactory.sol

122:    function computeAddress(address token0, address token1) internal view returns (address pool) {

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L122

```solidity
File: src/core/contracts/AlgebraPool.sol

215     function _recalculatePosition(
216       Position storage _position,
217       int128 liquidityDelta,
218       uint256 innerFeeGrowth0Token,
219:      uint256 innerFeeGrowth1Token

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L215-L219

### [G&#x2011;07]  `<array>.length` should not be looked up in every loop of a `for`-loop
The overheads outlined below are _PER LOOP_, excluding the first loop
* storage arrays incur a Gwarmaccess (**100 gas**)
* memory arrays use `MLOAD` (**3 gas**)
* calldata arrays use `CALLDATALOAD` (**3 gas**)

Caching the length changes each of these to a `DUP<N>` (**3 gas**), and gets rid of the extra `DUP<N>` needed to store the stack offset

*There is 1 instance of this issue:*
```solidity
File: src/core/contracts/libraries/DataStorage.sol

307:      for (uint256 i = 0; i < secondsAgos.length; i++) {

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L307

### [G&#x2011;08]  Use a more recent version of solidity
Use a solidity version of at least 0.8.0 to get overflow protection without `SafeMath`
Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than `revert()/require()` strings
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

*There are 13 instances of this issue:*
```solidity
File: src/core/contracts/AlgebraFactory.sol

2:    pragma solidity =0.7.6;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L2

```solidity
File: src/core/contracts/AlgebraPoolDeployer.sol

2:    pragma solidity =0.7.6;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L2

```solidity
File: src/core/contracts/AlgebraPool.sol

2:    pragma solidity =0.7.6;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L2

```solidity
File: src/core/contracts/base/PoolImmutables.sol

2:    pragma solidity =0.7.6;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolImmutables.sol#L2

```solidity
File: src/core/contracts/base/PoolState.sol

2:    pragma solidity =0.7.6;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolState.sol#L2

```solidity
File: src/core/contracts/DataStorageOperator.sol

2:    pragma solidity =0.7.6;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L2

```solidity
File: src/core/contracts/libraries/AdaptiveFee.sol

2:    pragma solidity =0.7.6;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L2

```solidity
File: src/core/contracts/libraries/Constants.sol

2:    pragma solidity =0.7.6;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/Constants.sol#L2

```solidity
File: src/core/contracts/libraries/DataStorage.sol

2:    pragma solidity =0.7.6;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L2

```solidity
File: src/core/contracts/libraries/PriceMovementMath.sol

2:    pragma solidity =0.7.6;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L2

```solidity
File: src/core/contracts/libraries/TickManager.sol

2:    pragma solidity =0.7.6;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickManager.sol#L2

```solidity
File: src/core/contracts/libraries/TickTable.sol

2:    pragma solidity =0.7.6;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickTable.sol#L2

```solidity
File: src/core/contracts/libraries/TokenDeltaMath.sol

2:    pragma solidity =0.7.6;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TokenDeltaMath.sol#L2

### [G&#x2011;09]  Using `> 0` costs more gas than `!= 0` when used on a `uint` in a `require()` statement
This change saves **[6 gas](https://aws1.discourse-cdn.com/business6/uploads/zeppelin/original/2X/3/363a367d6d68851f27d2679d10706cd16d788b96.png)** per instance. The optimization works until solidity version [0.8.13](https://gist.github.com/IllIllI000/bf2c3120f24a69e489f12b3213c06c94) where there is a regression in gas costs.

*There are 6 instances of this issue:*
```solidity
File: src/core/contracts/AlgebraPool.sol

224:        require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges

434:      require(liquidityDesired > 0, 'IL');

469:      require(liquidityActual > 0, 'IIL2');

898:      require(_liquidity > 0, 'L');

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L224

```solidity
File: src/core/contracts/libraries/PriceMovementMath.sol

52:       require(price > 0);

53:       require(liquidity > 0);

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L52

### [G&#x2011;10]  `>=` costs less gas than `>`
The compiler uses opcodes `GT` and `ISZERO` for solidity code that uses `>`, but only requires `LT` for `>=`, [which saves **3 gas**](https://gist.github.com/IllIllI000/3dc79d25acccfa16dee4e83ffdc6ffde)

*There are 2 instances of this issue:*
```solidity
File: src/core/contracts/AlgebraPool.sol

498:      amount0 = amount0Requested > positionFees0 ? positionFees0 : amount0Requested;

499:      amount1 = amount1Requested > positionFees1 ? positionFees1 : amount1Requested;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L498

### [G&#x2011;11]  `++i` costs less gas than `i++`, especially when it's used in `for`-loops (`--i`/`i--` too)
Saves **5 gas per loop**

*There is 1 instance of this issue:*
```solidity
File: src/core/contracts/libraries/DataStorage.sol

307:      for (uint256 i = 0; i < secondsAgos.length; i++) {

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L307

### [G&#x2011;12]  Splitting `require()` statements that use `&&` saves gas
See [this issue](https://github.com/code-423n4/2022-01-xdefi-findings/issues/128) which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper by **3 gas**

*There are 6 instances of this issue:*
```solidity
File: src/core/contracts/AlgebraFactory.sol

110:      require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L110

```solidity
File: src/core/contracts/AlgebraPool.sol

739:          require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');

743:          require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');

953:      require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));

968:      require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L739

```solidity
File: src/core/contracts/DataStorageOperator.sol

46:       require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L46

### [G&#x2011;13]  Using `private` rather than `public` for constants, saves gas
If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that [returns a tuple](https://github.com/code-423n4/2022-08-frax/blob/90f55a9ce4e25bceed3a74290b854341d8de6afa/src/contracts/FraxlendPair.sol#L156-L178) of the values of all currently-public constants. Saves **3406-3606 gas** in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

*There is 1 instance of this issue:*
```solidity
File: src/core/contracts/libraries/DataStorage.sol

12:     uint32 public constant WINDOW = 1 days;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L12

### [G&#x2011;14]  Division by two should use bit shifting
`<x> / 2` is the same as `<x> >> 1`. While the compiler uses the `SHR` opcode to accomplish both, the version that uses division incurs an overhead of [**20 gas**](https://gist.github.com/IllIllI000/ec0e4e6c4f52a6bca158f137a3afd4ff) due to `JUMP`s to and from a compiler utility function that introduces checks which can be avoided by using `unchecked {}` around the division by two

*There is 1 instance of this issue:*
```solidity
File: src/core/contracts/libraries/AdaptiveFee.sol

88:       res += (xLowestDegree * gHighestDegree) / 2;

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/AdaptiveFee.sol#L88

### [G&#x2011;15]  Use custom errors rather than `revert()`/`require()` strings to save gas
Custom errors are available from solidity version 0.8.4. Custom errors save [**~50 gas**](https://gist.github.com/IllIllI000/ad1bd0d29a0101b25e57c293b4b0c746) each time they're hit by [avoiding having to allocate and store the revert string](https://blog.soliditylang.org/2021/04/21/custom-errors/#errors-in-depth). Not defining the strings also save deployment gas

*There are 32 instances of this issue:*
```solidity
File: src/core/contracts/AlgebraFactory.sol

109:      require(uint256(alpha1) + uint256(alpha2) + uint256(baseFee) <= type(uint16).max, 'Max fee exceeded');

110:      require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L109

```solidity
File: src/core/contracts/AlgebraPool.sol

60:       require(topTick < TickMath.MAX_TICK + 1, 'TUM');

61:       require(topTick > bottomTick, 'TLU');

62:       require(bottomTick > TickMath.MIN_TICK - 1, 'TLM');

194:      require(globalState.price == 0, 'AI');

224:        require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges

434:      require(liquidityDesired > 0, 'IL');

454:        if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');

455:        if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');

469:      require(liquidityActual > 0, 'IIL2');

474:        require((amount0 = uint256(amount0Int)) <= receivedAmount0, 'IIAM2');

475:        require((amount1 = uint256(amount1Int)) <= receivedAmount1, 'IIAM2');

608:        require(balance0Before.add(uint256(amount0)) <= balanceToken0(), 'IIA');

614:        require(balance1Before.add(uint256(amount1)) <= balanceToken1(), 'IIA');

636:      require(globalState.unlocked, 'LOK');

641:        require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');

645:        require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');

731:        require(unlocked, 'LOK');

733:        require(amountRequired != 0, 'AS');

739:          require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');

743:          require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');

898:      require(_liquidity > 0, 'L');

921:      require(balance0Before.add(fee0) <= paid0, 'F0');

935:      require(balance1Before.add(fee1) <= paid1, 'F1');

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L60

```solidity
File: src/core/contracts/base/PoolState.sol

41:       require(globalState.unlocked, 'LOK');

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/base/PoolState.sol#L41

```solidity
File: src/core/contracts/DataStorageOperator.sol

27:       require(msg.sender == pool, 'only pool can call this');

45:       require(uint256(_feeConfig.alpha1) + uint256(_feeConfig.alpha2) + uint256(_feeConfig.baseFee) <= type(uint16).max, 'Max fee exceeded');

46:       require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L27

```solidity
File: src/core/contracts/libraries/DataStorage.sol

238:      require(lteConsideringOverflow(self[oldestIndex].blockTimestamp, target, time), 'OLD');

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L238

```solidity
File: src/core/contracts/libraries/TickManager.sol

96:       require(liquidityTotalAfter < Constants.MAX_LIQUIDITY_PER_TICK + 1, 'LO');

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickManager.sol#L96

```solidity
File: src/core/contracts/libraries/TickTable.sol

15:       require(tick % Constants.TICK_SPACING == 0, 'tick is not spaced'); // ensure that the tick is spaced

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/TickTable.sol#L15

### [G&#x2011;16]  Functions guaranteed to revert when called by normal users can be marked `payable`
If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are 
`CALLVALUE`(2),`DUP1`(3),`ISZERO`(3),`PUSH2`(3),`JUMPI`(10),`PUSH1`(3),`DUP1`(3),`REVERT`(0),`JUMPDEST`(1),`POP`(2), which costs an average of about **21 gas per call** to the function, in addition to the extra deployment cost

*There are 17 instances of this issue:*
```solidity
File: src/core/contracts/AlgebraFactory.sol

77:     function setOwner(address _owner) external override onlyOwner {

84:     function setFarmingAddress(address _farmingAddress) external override onlyOwner {

91:     function setVaultAddress(address _vaultAddress) external override onlyOwner {

98      function setBaseFeeConfiguration(
99        uint16 alpha1,
100       uint16 alpha2,
101       uint32 beta1,
102       uint32 beta2,
103       uint16 gamma1,
104       uint16 gamma2,
105       uint32 volumeBeta,
106       uint16 volumeGamma,
107       uint16 baseFee
108:    ) external override onlyOwner {

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L77

```solidity
File: src/core/contracts/AlgebraPoolDeployer.sol

36:     function setFactory(address _factory) external override onlyOwner {

44      function deploy(
45        address dataStorage,
46        address _factory,
47        address token0,
48        address token1
49:     ) external override onlyFactory returns (address pool) {

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPoolDeployer.sol#L36

```solidity
File: src/core/contracts/AlgebraPool.sol

103     function getInnerCumulatives(int24 bottomTick, int24 topTick)
104       external
105       view
106       override
107       onlyValidTicks(bottomTick, topTick)
108       returns (
109         int56 innerTickCumulative,
110         uint160 innerSecondsSpentPerLiquidity,
111:        uint32 innerSecondsSpent

416     function mint(
417       address sender,
418       address recipient,
419       int24 bottomTick,
420       int24 topTick,
421       uint128 liquidityDesired,
422       bytes calldata data
423     )
424       external
425       override
426       lock
427       onlyValidTicks(bottomTick, topTick)
428       returns (
429         uint256 amount0,
430         uint256 amount1,
431:        uint128 liquidityActual

513     function burn(
514       int24 bottomTick,
515       int24 topTick,
516       uint128 amount
517:    ) external override lock onlyValidTicks(bottomTick, topTick) returns (uint256 amount0, uint256 amount1) {

952:    function setCommunityFee(uint8 communityFee0, uint8 communityFee1) external override lock onlyFactoryOwner {

967:    function setLiquidityCooldown(uint32 newLiquidityCooldown) external override onlyFactoryOwner {

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L103-L111

```solidity
File: src/core/contracts/DataStorageOperator.sol

37:     function initialize(uint32 time, int24 tick) external override onlyPool {

53      function getSingleTimepoint(
54        uint32 time,
55        uint32 secondsAgo,
56        int24 tick,
57        uint16 index,
58        uint128 liquidity
59      )
60        external
61        view
62        override
63        onlyPool
64        returns (
65          int56 tickCumulative,
66          uint160 secondsPerLiquidityCumulative,
67          uint112 volatilityCumulative,
68:         uint256 volumePerAvgLiquidity

88      function getTimepoints(
89        uint32 time,
90        uint32[] memory secondsAgos,
91        int24 tick,
92        uint16 index,
93        uint128 liquidity
94      )
95        external
96        view
97        override
98        onlyPool
99        returns (
100         int56[] memory tickCumulatives,
101         uint160[] memory secondsPerLiquidityCumulatives,
102         uint112[] memory volatilityCumulatives,
103:        uint256[] memory volumePerAvgLiquiditys

110     function getAverages(
111       uint32 time,
112       int24 tick,
113       uint16 index,
114       uint128 liquidity
115:    ) external view override onlyPool returns (uint112 TWVolatilityAverage, uint256 TWVolumePerLiqAverage) {

120     function write(
121       uint16 index,
122       uint32 blockTimestamp,
123       int24 tick,
124       uint128 liquidity,
125       uint128 volumePerLiquidity
126:    ) external override onlyPool returns (uint16 indexUpdated) {

150     function getFee(
151       uint32 _time,
152       int24 _tick,
153       uint16 _index,
154       uint128 _liquidity
155:    ) external view override onlyPool returns (uint16 fee) {

```
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L37
