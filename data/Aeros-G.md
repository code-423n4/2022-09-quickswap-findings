### Gas Optimizations:

Splitting up *require()/revert()* statements that use *&&* saves gas.

Places where *require()* statements exist with *&&* (and should be split up):

* src/core/contracts/AlgebraFactory.sol (line 110)
* src/core/contracts/AlgebraPool.sol (lines 739, 743, 953)
* src/core/contracts/DataStorageOperator.sol (line 46)

In **src/core/contracts/libraries/DataStorage.sol**, the *getTimePoints()* method calls *secondsAgos.length* multiple times (including in a for loop). It should be cached in a stack variable rather than being accessed every time (especially in the loop).

Original:

    function getTimepoints(
        Timepoint[UINT16_MODULO] storage self,
        uint32 time,
        uint32[] memory secondsAgos,
        int24 tick,
        uint16 index,
        uint128 liquidity
      )
        internal
        view
        returns (
          int56[] memory tickCumulatives,
          uint160[] memory secondsPerLiquidityCumulatives,
          uint112[] memory volatilityCumulatives,
          uint256[] memory volumePerAvgLiquiditys
        )
      {
        tickCumulatives = new int56[](secondsAgos.length); // since this is called 4 times, it should be cached in stack variable
        secondsPerLiquidityCumulatives = new uint160[](secondsAgos.length);
        volatilityCumulatives = new uint112[](secondsAgos.length);
        volumePerAvgLiquiditys = new uint256[](secondsAgos.length);
 
        uint16 oldestIndex;
        // check if we have overflow in the past
        uint16 nextIndex = index + 1; // considering overflow
        if (self[nextIndex].initialized) {
          oldestIndex = nextIndex;
        }
 
        Timepoint memory current;
        for (uint256 i = 0; i < secondsAgos.length; i++) {
          // .length should not be called on every loop, cache it in a stack variable
          current = getSingleTimepoint(self, time, secondsAgos[i], tick, index, oldestIndex, liquidity);
          (tickCumulatives[i], secondsPerLiquidityCumulatives[i], volatilityCumulatives[i], volumePerAvgLiquiditys[i]) = (
            current.tickCumulative,
            current.secondsPerLiquidityCumulative,
            current.volatilityCumulative,
            current.volumePerLiquidityCumulative
          );
        }
      }


Fix:

    function getTimepoints(
        Timepoint[UINT16_MODULO] storage self,
        uint32 time,
        uint32[] memory secondsAgos,
        int24 tick,
        uint16 index,
        uint128 liquidity
      )
        internal
        view
        returns (
          int56[] memory tickCumulatives,
          uint160[] memory secondsPerLiquidityCumulatives,
          uint112[] memory volatilityCumulatives,
          uint256[] memory volumePerAvgLiquiditys
        )
      {
        uint256 secondsAgosLength = secondsAgos.length;
        tickCumulatives = new int56[](secondsAgosLength);
        secondsPerLiquidityCumulatives = new uint160[](secondsAgosLength);
        volatilityCumulatives = new uint112[](secondsAgosLength);
        volumePerAvgLiquiditys = new uint256[](secondsAgosLength);
     
        uint16 oldestIndex;
        // check if we have overflow in the past
        uint16 nextIndex = index + 1; // considering overflow
        if (self[nextIndex].initialized) {
          oldestIndex = nextIndex;
        }
 
        Timepoint memory current;
        for (uint256 i = 0; i < secondsAgosLength; i++) {
          current = getSingleTimepoint(self, time, secondsAgos[i], tick, index, oldestIndex, liquidity);
          (tickCumulatives[i], secondsPerLiquidityCumulatives[i], volatilityCumulatives[i], volumePerAvgLiquiditys[i]) = (
            current.tickCumulative,
            current.secondsPerLiquidityCumulative,
            current.volatilityCumulative,
            current.volumePerLiquidityCumulative
          );
        }
      }



In **src/core/contracts/libraries/DataStorage.sol** (at line 307), *i++* is used. *++i* uses less gas, and so it could be switched to this. 
