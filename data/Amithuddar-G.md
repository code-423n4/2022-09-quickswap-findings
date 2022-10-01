1)++i costs less gas than i++, especially when it’s used in for-loops (--i/i-- too)

Saves 6 gas PER LOOP



File: 2022-09-quickswap\src\core\contracts\libraries\DataStorage.sol
  307,49:     for (uint256 i = 0; i < secondsAgos.length; i++) {
  
2)Use custom errors rather than revert()/require() strings to save gas 

File: 2022-09-quickswap\src\core\contracts\AlgebraFactory.sol

  109,5:     require(uint256(alpha1) + uint256(alpha2) + uint256(baseFee) <= type(uint16).max, 'Max fee exceeded');
  110,5:     require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');


File: 2022-09-quickswap\src\core\contracts\AlgebraPool.sol
 
  60,5:     require(topTick < TickMath.MAX_TICK + 1, 'TUM');
  61,5:     require(topTick > bottomTick, 'TLU');
  62,5:     require(bottomTick > TickMath.MIN_TICK - 1, 'TLM');

  194,5:     require(globalState.price == 0, 'AI');
  224,7:       require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges
  
  434,5:     require(liquidityDesired > 0, 'IL');
  
  469,5:     require(liquidityActual > 0, 'IIL2');
  474,7:       require((amount0 = uint256(amount0Int)) <= receivedAmount0, 'IIAM2');
  475,7:       require((amount1 = uint256(amount1Int)) <= receivedAmount1, 'IIAM2');
  608,7:       require(balance0Before.add(uint256(amount0)) <= balanceToken0(), 'IIA');
  614,7:       require(balance1Before.add(uint256(amount1)) <= balanceToken1(), 'IIA');
  636,5:     require(globalState.unlocked, 'LOK');
  641,7:       require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');
  645,7:       require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');
  731,7:       require(unlocked, 'LOK');
  733,7:       require(amountRequired != 0, 'AS');
  739,9:         require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
  743,9:         require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
  898,5:     require(_liquidity > 0, 'L');
  921,5:     require(balance0Before.add(fee0) <= paid0, 'F0');
  935,5:     require(balance1Before.add(fee1) <= paid1, 'F1');
 


File: 2022-09-quickswap\src\core\contracts\DataStorageOperator.sol
  27,5:     require(msg.sender == pool, 'only pool can call this');
  
  45,5:     require(uint256(_feeConfig.alpha1) + uint256(_feeConfig.alpha2) + uint256(_feeConfig.baseFee) <= type(uint16).max, 'Max fee exceeded');
  46,5:     require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');


File: 2022-09-quickswap\src\core\contracts\base\PoolState.sol
  41,5:     require(globalState.unlocked, 'LOK');

File: 2022-09-quickswap\src\core\contracts\libraries\DataStorage.sol
  238,5:     require(lteConsideringOverflow(self[oldestIndex].blockTimestamp, target, time), 'OLD');


File: 2022-09-quickswap\src\core\contracts\libraries\TickTable.sol
  15,5:     require(tick % Constants.TICK_SPACING == 0, 'tick is not spaced'); // ensure that the tick is spaced

3) USING > 0 COSTS MORE GAS THAN != 0 WHEN USED ON A UINT IN A REQUIRE() STATEMENT

This change saves 6 gas per instance


File: 2022-09-quickswap\src\core\contracts\AlgebraPool.sol
  224,32:       require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges
  
  469,29:     require(liquidityActual > 0, 'IIL2');
  
  641,78:       require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');
  645,78:       require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');
  
  898,24:     require(_liquidity > 0, 'L');
  
File: 2022-09-quickswap\src\core\contracts\libraries\FullMath.sol
  114,27:       require(denominator > 0);

File: 2022-09-quickswap\src\core\contracts\libraries\PriceMovementMath.sol
  52,19:     require(price > 0);
  53,23:     require(liquidity > 0); 
  

4) ++i/i++ should be unchecked{++i}/unchecked{++i} when it is not possible for them to overflow,
as is the case when used in for- and while-loops   


File: 2022-09-quickswap\src\core\contracts\libraries\DataStorage.sol
  307,49:     for (uint256 i = 0; i < secondsAgos.length; i++) {  
  
5)It costs more gas to initialize variables to zero than to let the default of zero be applied

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: for (uint256 i = 0; i < numIterations; ++i) { should be replaced with for (uint256 i; i < numIterations; ++i) {  


File: 2022-09-quickswap\src\core\contracts\libraries\DataStorage.sol
  307,49:     for (uint256 i = 0; i < secondsAgos.length; i++) { 


6)<ARRAY>.LENGTH SHOULD NOT BE LOOKED UP IN EVERY LOOP OF A FOR-LOOP
The overheads outlined below are PER LOOP, 

storage arrays incur a Gwarmaccess (100 gas)
memory arrays use MLOAD (3 gas)
calldata arrays use CALLDATALOAD (3 gas)
Caching the length changes each of these to a DUP<N> (3 gas), and gets rid of the extra DUP<N> needed to store the stack offset

File: 2022-09-quickswap\src\core\contracts\libraries\DataStorage.sol
  307,49:     for (uint256 i = 0; i < secondsAgos.length; i++) { 

7)X = X + Y IS CHEAPER THAN X += Y 

File: 2022-09-quickswap\src\core\contracts\AlgebraPool.sol
  257,23:       _position.fees0 += fees0;
  258,23:       _position.fees1 += fees1;
  804,24:         amountRequired += step.output.toInt256(); // increase remaining output amount (since its negative)
  811,28:         communityFeeAmount += delta;
  814,54:       if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);
  931,28:       totalFeeGrowth0Token += FullMath.mulDiv(paid0 - fees0, Constants.Q128, _liquidity);
  945,28:       totalFeeGrowth1Token += FullMath.mulDiv(paid1 - fees1, Constants.Q128, _liquidity);
  
File: 2022-09-quickswap\src\core\contracts\libraries\AdaptiveFee.sol
  84,9:     res += xLowestDegree * gHighestDegree;
  88,9:     res += (xLowestDegree * gHighestDegree) / 2;
  92,9:     res += (xLowestDegree * gHighestDegree) / 6;
  96,9:     res += (xLowestDegree * gHighestDegree) / 24;
  100,9:     res += (xLowestDegree * gHighestDegree) / 120;
  104,9:     res += (xLowestDegree * gHighestDegree) / 720;
  107,9:     res += (xLowestDegree * g) / 5040 + (xLowestDegree * x) / (40320);


File: 2022-09-quickswap\src\core\contracts\libraries\DataStorage.sol
  79,25:     last.tickCumulative += int56(tick) * delta;
  80,40:     last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0
  81,31:     last.volatilityCumulative += uint88(_volatilityOnRange(delta, prevTick, tick, last.averageTick, averageTick)); // always fits 88 bits
  83,39:     last.volumePerLiquidityCumulative += volumePerLiquidity;
  251,33:       beforeOrAt.tickCumulative += ((atOrAfter.tickCumulative - beforeOrAt.tickCumulative) / timepointTimeDelta) * targetDelta;
  252,48:       beforeOrAt.secondsPerLiquidityCumulative += uint160(
  255,39:       beforeOrAt.volatilityCumulative += ((atOrAfter.volatilityCumulative - beforeOrAt.volatilityCumulative) / timepointTimeDelta) * targetDelta;
  256,47:       beforeOrAt.volumePerLiquidityCumulative +=

File: 2022-09-quickswap\src\core\contracts\libraries\TickTable.sol
  100,12:       tick += 1;
  112,14:         tick += int24(getSingleSignificantBit(-_row & _row)); // least significant bit
  115,14:         tick += int24(255 - bitNumber);

8)USE A MORE RECENT VERSION OF SOLIDITY

Use a solidity version of at least 0.8.2 to get compiler automatic inlining

Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads

Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings

Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value


File: 2022-09-quickswap\src\core\contracts\AlgebraFactory.sol
  2,1: pragma solidity =0.7.6;
  
File: 2022-09-quickswap\src\core\contracts\AlgebraPool.sol
  2,1: pragma solidity =0.7.6;

File: 2022-09-quickswap\src\core\contracts\AlgebraPoolDeployer.sol
  2,1: pragma solidity =0.7.6;

File: 2022-09-quickswap\src\core\contracts\DataStorageOperator.sol
  2,1: pragma solidity =0.7.6;

File: 2022-09-quickswap\src\core\contracts\base\PoolImmutables.sol
  2,1: pragma solidity =0.7.6;

File: 2022-09-quickswap\src\core\contracts\base\PoolState.sol
  2,1: pragma solidity =0.7.6;

File: 2022-09-quickswap\src\core\contracts\libraries\AdaptiveFee.sol
  2,1: pragma solidity =0.7.6;

File: 2022-09-quickswap\src\core\contracts\libraries\Constants.sol
  2,1: pragma solidity =0.7.6;

File: 2022-09-quickswap\src\core\contracts\libraries\DataStorage.sol
  2,1: pragma solidity =0.7.6;

File: 2022-09-quickswap\src\core\contracts\libraries\TickManager.sol
  2,1: pragma solidity =0.7.6;
  
File: 2022-09-quickswap\src\core\contracts\libraries\TickTable.sol
  2,1: pragma solidity =0.7.6;

File: 2022-09-quickswap\src\core\contracts\libraries\TokenDeltaMath.sol
  2,1: pragma solidity =0.7.6;  




  
9)Using private rather than public for constants, saves gas

If needed, the value can be read from the verified contract source code.
Savings are due to the compiler not having to create non-payable getter
functions for deployment calldata, and not adding another entry to the method ID table

File: 2022-09-quickswap\src\core\contracts\libraries\DataStorage.sol
  12,10:   uint32 public constant WINDOW = 1 days; 

  