# Table of contents

- **[[0x0] Disclaimer](#0x0)**
- **[[G-01] Try ++i instead of i++](G-01)**
- **[[G-02] Try `unchecked{++i}` instead of `i++` in loops](G-02)**
- **[[G-03] Consider `a = a + b` instead of `a += b`](G-03)**
- **[[G-04] Consider marking onlyOwner functions as payable](G-04)**
- **[[G-05] Use binary shifting instead of `a / 2^x, x > 0`](G-05)**
- **[[G-06] Cache state variables, `MLOAD` << `SLOAD`](G-06)**
- **[[G-07] Add `require()` before some computations, if it makes sense](G-07)**
- **[[G-10] `Internal` functions can be inlined](G-10)**
- **[[G-11] Cache the array.length into a variable](G-11)**
- **[[G-12] Unused the named return variables](G-12)**
- **[[G-15] Use `private/internal` for `constants/immutable` variables instead of `public`](G-15)**
- **[[G-17] Use const values instead of `type(uint256).max`](G-17)**
- **[[G-19] Use `uint` instead of `uint8`, `uint64`, if it's possible](G-19)**
- **[[G-21] Use `calldataload` instead of `mload`](G-21)**


## Disclaimer<a name="0x0"></a>

- Please, consider everything described below as a general recommendation. These notes will represent potential possibilities to optimize gas consumption. It's okay, if something is not suitable in your case. Just let me know the reason in the comments section. Enjoy!

## **[G-01] Try ++i instead of i++**<a name="G-01"></a>

### ***Description:***

  - In case of `i++`, the compiler has to to create a temp variable to return `i` (if there's a need) and then `i` gets incremented.  
  - In case of `++i`, the compiler just simply returns already incremented value.

### ***All occurances:***

  - Contracts:
  
    ```Solidity
      file: contracts/libraries/DataStorage.sol 
      ...............................
      
        // Lines: [307-307]
          for (uint256 i = 0; i < secondsAgos.length; i++) {}

    ```

## **[G-02] Try `unchecked{++i};` instead of `i++;`/`++i;` in loops**<a name="G-02"></a>

### ***Description:***

  - Since [Solidity 0.8.0](https://github.com/ethereum/solidity/releases/tag/v0.8.0), all arithmetic operations revert on over- and underflow by default Therefore, `unchecked` box can be used to prevent all unnecessary checks, if it's no a way to get a reversion.
  
  - There are ~1e80 atoms in the universe, so 2^256 is closed to that number, therefore it's no a way to be overflowed, because of the gas limit as well. 

### ***All occurances:***

- Contracts:

    ```Solidity
      file: contracts/libraries/DataStorage.sol 
      ...............................
      
        // Lines: [307-307]
          for (uint256 i = 0; i < secondsAgos.length; i++) {}

    ```

## **[G-03] Consider `a = a + b` instead of `a += b`**<a name="G-03"></a>

### ***Description:***

- It has an impact on the deployment cost and the cost for distinct transaction as well.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: contracts/AlgebraPool.sol 
      ...............................
      
        // Lines: [257-258]
          _position.fees0 += fees0;
          _position.fees1 += fees1;

        // Lines: [801-801]
          amountRequired -= (step.input + step.feeAmount).toInt256(); // decrease remaining input amount

        // Lines: [804-804]
          amountRequired += step.output.toInt256(); // increase remaining output amount (since its negative)

        // Lines: [810-811]
          step.feeAmount -= delta;
          communityFeeAmount += delta;

        // Lines: [814-814]
          if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);

        // Lines: [922-922]
            paid0 -= balance0Before;

        // Lines: [936-936]
            paid1 -= balance1Before;

        // Lines: [931-931]
          totalFeeGrowth0Token += FullMath.mulDiv(paid0 - fees0, Constants.Q128, _liquidity);

        // Lines: [945-945]
          totalFeeGrowth1Token += FullMath.mulDiv(paid1 - fees1, Constants.Q128, _liquidity);

      file: contracts/libraries/AdaptiveFee.sol
      ...........................................

        // Lines: [83-107]
            gHighestDegree /= g; // g**7
            res += xLowestDegree * gHighestDegree;

            gHighestDegree /= g; // g**6
            xLowestDegree *= x; // x**2
            res += (xLowestDegree * gHighestDegree) / 2;

            gHighestDegree /= g; // g**5
            xLowestDegree *= x; // x**3
            res += (xLowestDegree * gHighestDegree) / 6;

            gHighestDegree /= g; // g**4
            xLowestDegree *= x; // x**4
            res += (xLowestDegree * gHighestDegree) / 24;

            gHighestDegree /= g; // g**3
            xLowestDegree *= x; // x**5
            res += (xLowestDegree * gHighestDegree) / 120;

            gHighestDegree /= g; // g**2
            xLowestDegree *= x; // x**6
            res += (xLowestDegree * gHighestDegree) / 720;

            xLowestDegree *= x; // x**7
            res += (xLowestDegree * g) / 5040 + (xLowestDegree * x) / (40320);

      file: contracts/libraries/DataStorage.sol 
      ..........................................
        // Lines: [79-81]
            last.tickCumulative += int56(tick) * delta;
            last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0
            last.volatilityCumulative += uint88(_volatilityOnRange(delta, prevTick, tick, last.averageTick, averageTick)); // always fits 88 bits

        // Lines: [119-119]
            index -= 1; // considering underflow

        // Lines: [83-83]
            last.volumePerLiquidityCumulative += volumePerLiquidity;

        // Lines: [251-255]
            beforeOrAt.tickCumulative += ((atOrAfter.tickCumulative - beforeOrAt.tickCumulative) / timepointTimeDelta) * targetDelta;
            beforeOrAt.secondsPerLiquidityCumulative += uint160(
              (uint256(atOrAfter.secondsPerLiquidityCumulative - beforeOrAt.secondsPerLiquidityCumulative) * targetDelta) / timepointTimeDelta
            );
            beforeOrAt.volatilityCumulative += ((atOrAfter.volatilityCumulative - beforeOrAt.volatilityCumulative) / timepointTimeDelta) * targetDelta;


      file: contracts/libraries/TickManager.sol 
      ..........................................
        // Lines: [58-58]
          innerFeeGrowth0Token -= upper.outerFeeGrowth0Token;
          innerFeeGrowth1Token -= upper.outerFeeGrowth1Token;

      file: contracts/libraries/TickTable.sol 
      ..........................................
        // Lines: [112-112]
          tick += int24(getSingleSignificantBit(-_row & _row)); // least significant bit

        // Lines: [95-95]
          tick -= int24(bitNumber);

        // Lines: [92-92]
          tick -= int24(255 - getMostSignificantBit(_row));

        // Lines: [100-100]
            tick += 1;

        // Lines: [115-115]
          tick += int24(255 - bitNumber);

        // Lines: [24-24]
          self[rowNumber] ^= 1 << bitNumber;

    
    ```

## **[G-04] Consider marking onlyOwner functions as payable**<a name="G-04"></a>

### ***Description:***

- This one is a bit questionable, but you can try that out. So, the compiler adds some extra conditions in case of non-payable, but we know that `onlyOwner` modifier will be reverted, if the user invoke following methods.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: contracts/AlgebraPool.sol 
      ...............................
      
        // Lines: [77-77]
          function setOwner(address _owner) external override onlyOwner {}
        ```

## **[G-05] Use binary shifting instead of `a / 2^x, x > 0` or `a * 2^x, x > 0`**<a name="G-05"></a>

### ***Description:***

- It's also pretty impactful one, especially in loops.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: contracts/libraries/AdaptiveFee.sol 
      .........................................
      
        // Lines: [88-88]
          res += (xLowestDegree * gHighestDegree) / 2;
      ```

## **[G-06] Cache state variables, `MLOAD` << `SLOAD`**<a name="G-06"></a>

### ***Description:***

- `MLOAD` costs only 3 units of gas, `SLOAD`(warm access) is about 100 units. Therefore, cache, when it's possible.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: contracts/AlgebraPool.sol 
      ...............................

        // Lines: [116-116]
          // Comment: it's possible to cache _lower into the memory, since there are only read operations. 
              TickManager.Tick storage _lower = ticks[bottomTick]; 

        // Lines: [127-127]
          // Comment: the same for `_upper` 
              TickManager.Tick storage _upper = ticks[topTick];

        // Lines: [496-496]
          // Comment: there is an ability to push position into the memory to have a read calls from.
            (uint128 positionFees0, uint128 positionFees1) = (position.fees0, position.fees1); 

        // Lines: [529-529]
          // Comment: the same here
            (position.fees0, position.fees1) = (position.fees0.add128(uint128(amount0)), position.fees1.add128(uint128(amount1))); 

        ```

## **[G-07] Add `require() before some computations, if it makes sense`**<a name="G-07"></a>


### ***Description:***

- Everyting above `require()` takes some gas for execution, therefore if the statement reverts gas will not be retrieved.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: src/core/contracts/AlgebraPool.sol
      ...............................
      
        // Lines: [122-122]
            require(_lower.initialized);

        // Lines: [134-134]
            require(_upper.initialized);
    ```


## **[G-10] `Internal` functions can be inlined**<a name="G-10"></a>

### ***Description:***

- It takes some extra `JUMP`s which costs around 40-50 gas uints. In loops it will save significant amount of gas.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: src/core/contracts/base/PoolState.sol 
      ...............................
      
        // Lines: [49-49]
          function _blockTimestamp() internal view virtual returns (uint32) {
            return uint32(block.timestamp); // truncation is desired
          }
    ```

## **[G-11] Cache the array.length into a variable**<a name="G-11"></a>

### ***Description:***

- Reduces the extra `DUP<X>` for every iteration.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: src/core/contracts/libraries/DataStorage.sol 
      ...................................................
      
        // Lines: [294-297]
            tickCumulatives = new int56[](secondsAgos.length);
            secondsPerLiquidityCumulatives = new uint160[](secondsAgos.length);
            volatilityCumulatives = new uint112[](secondsAgos.length);
            volumePerAvgLiquiditys = new uint256[](secondsAgos.length);
    ```

## **[G-12] Unused the named return variables**<a name="G-12"></a>

### ***Description:***

- Unnecessary gas usage.

### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: src/core/contracts/AlgebraPool.sol 
      ........................................
      
        // Lines: [79-94]
          function timepoints(uint256 index)
            external
            view
            override
            returns (
              bool initialized,
              uint32 blockTimestamp,
              int56 tickCumulative,
              uint160 secondsPerLiquidityCumulative,
              uint88 volatilityCumulative,
              int24 averageTick,
              uint144 volumePerLiquidityCumulative
            )
          {
            return IDataStorageOperator(dataStorageOperator).timepoints(index);
          }
    ```


## **[G-15] Use `private/internal` for `constants/immutable/state` variables instead of `public`**<a name="G-15"></a>

### ***Description:***

- Optimization comes from not creating a getter function for each `public` instance.
  
### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: src/core/contracts/AlgebraFactory.sol 
      ...............................
      
        // Lines: [17-26]
          address public override owner;

          /// @inheritdoc IAlgebraFactory
          address public immutable override poolDeployer;

          /// @inheritdoc IAlgebraFactory
          address public override farmingAddress;

          /// @inheritdoc IAlgebraFactory
          address public override vaultAddress;
    ```


## **[G-17] Use const values instead of `type(uint256).max`**<a name="G-17"></a>

### ***Description:***

- Not sure about readability, but it might be tangible in loops.
  
### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: src/core/contracts/AlgebraFactory.sol 
      ...............................
      
        // Lines: [109-109]
          require(uint256(alpha1) + uint256(alpha2) + uint256(baseFee) <= type(uint16).max, 'Max fee exceeded');
    ```

## **[G-19] Use `uint` instead of `uint8`, `uint64`, if it's possible**<a name="G-19"></a>

### ***Description:***

- I deployed a contract with only a single error and function and compared between:
  - `error VaultAlreadyUnderAuction(bytes12 vaultId, address witch);`.
  - `error VaultAlreadyUnderAuction(bytes32 vaultId, address witch);`.
- The second one is saving about ~10k units of gas for deployment and 10k per each transaction.
- However, defaining further errors with !`bytes32` doesn't give a huge optimization.  
- The stack slots are 32bytes, just use 32bytes, if you are not trying to tight up the storage.
  
### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: src/core/contracts/AlgebraPool.sol 
      ........................................
      
        // Lines: [925-925]
          uint8 _communityFeeToken0 = globalState.communityFeeToken0;


      ```
## **[G-21] Use `calldataload` instead of `mload`**<a name="G-21"></a>

### ***Description:***

- After Berlin hard fork, to load a non-zero byte from calldata dropped from 64 units of gas to 16 units, therefore if you do not modify args, use a calldata instead of memory. Here you need to explicitly specify `calldataload`, or replace `memory` with `calldata`. If the args are pretty huge, allocating args in memory will cost a lot. 
  
### ***All occurances:***

- Contracts:
  
    ```Solidity
      file: src/core/contracts/DataStorageOperator.sol 
      ...............................
      
        // Lines: [88-107]
          // Comment: It's possible to use calldata for secondAgos and calldata for return variables. In this case, return variables are unused, but it's possible to return via calldata some vars. 

          function getTimepoints(
            uint32 time,
            uint32[] memory secondsAgos,
            int24 tick,
            uint16 index,
            uint128 liquidity
          )
            external
            view
            override
            onlyPool
            returns (
              int56[] memory tickCumulatives,
              uint160[] memory secondsPerLiquidityCumulatives,
              uint112[] memory volatilityCumulatives,
              uint256[] memory volumePerAvgLiquiditys
            )
          {
            return timepoints.getTimepoints(time, secondsAgos, tick, index, liquidity);
          }


      ```
## Kudos for the quality of the code! It's pretty easy to explore
