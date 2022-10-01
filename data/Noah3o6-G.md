-> X = X + Y IS CHEAPER THAN X += Y (same for X = X - Y IS CHEAPER THAN X -= Y)

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#:~:text=amountRequired%20%2D%3D%20(step.input%20%2B%20step.feeAmount).toInt256()%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#:~:text=amountRequired%20%2B%3D%20step.output.toInt256()%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#:~:text=step.feeAmount%20%2D%3D%20delta%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#:~:text=communityFeeAmount%20%2B%3D%20delta%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#:~:text=)%20cache.totalFeeGrowth-,%2B%3D,-FullMath.mulDiv
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#:~:text=paid0%20%2D%3D%20balance0Before%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#:~:text=totalFeeGrowth0Token%20%2B%3D%20FullMath.mulDiv(paid0%20%2D%20fees0%2C%20Constants.Q128%2C%20_liquidity)%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#:~:text=paid1%20%2D%3D%20balance1Before%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#:~:text=totalFeeGrowth1Token%20%2B%3D%20FullMath.mulDiv(paid1%20%2D%20fees1%2C%20Constants.Q128%2C%20_liquidity)%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#:~:text=res%20%2B%3D%20xLowestDegree%20*%20gHighestDegree%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#:~:text=res%20%2B%3D%20(xLowestDegree%20*%20gHighestDegree)%20/%202%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#:~:text=res%20%2B%3D%20(xLowestDegree%20*%20gHighestDegree)%20/%206%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#:~:text=res%20%2B%3D%20(xLowestDegree%20*%20gHighestDegree)%20/%2024%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#:~:text=res%20%2B%3D%20(xLowestDegree%20*%20gHighestDegree)%20/%20120%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#:~:text=res%20%2B%3D%20(xLowestDegree%20*%20gHighestDegree)%20/%20720%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#:~:text=res%20%2B%3D%20(xLowestDegree%20*%20g)%20/%205040%20%2B%20(xLowestDegree%20*%20x)%20/%20(40320)%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#:~:text=last.tickCumulative%20%2B%3D%20int56(tick)%20*%20delta%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#:~:text=last.secondsPerLiquidityCumulative-,%2B%3D,-((uint160(delta
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#:~:text=last.volatilityCumulative-,%2B%3D,-uint88(_volatilityOnRange
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#:~:text=last.volumePerLiquidityCumulative-,%2B%3D,-volumePerLiquidity%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#:~:text=%2D%20WINDOW%2C%20time))%20%7B-,index%20%2D%3D%201%3B,-//%20considering%20underflow
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#:~:text=beforeOrAt.tickCumulative-,%2B%3D,-((atOrAfter.tickCumulative%20%2D
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#:~:text=beforeOrAt.secondsPerLiquidityCumulative-,%2B%3D,-uint160(
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#:~:text=beforeOrAt.volatilityCumulative-,%2B%3D,-((atOrAfter.volatilityCumulative%20%2D
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#:~:text=beforeOrAt.volumePerLiquidityCumulative-,%2B%3D,-((atOrAfter.volumePerLiquidityCumulative%20%2D
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#:~:text=innerFeeGrowth0Token%20%2D%3D%20upper.outerFeeGrowth0Token%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#:~:text=innerFeeGrowth1Token%20%2D%3D%20upper.outerFeeGrowth1Token%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#:~:text=tick%20%2D%3D%20int24(255%20%2D%20getMostSignificantBit(_row))%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#:~:text=tick%20%2D%3D%20int24(bitNumber)%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#:~:text=state%20doesn%27t%20matter-,tick%20%2B%3D%201%3B,-int16%20rowNumber%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#:~:text=tick%20%2B%3D%20int24(getSingleSignificantBit(%2D_row%20%26%20_row))%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#:~:text=tick%20%2B%3D%20int24(255%20%2D%20bitNumber)%3B


->USING > 0 COSTS MORE GAS THAN != 0 WHEN USED ON A UINT IN A REQUIRE() STATEMENT

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#:~:text=require(_liquidity%20%3E%200%2C%20%27L%27)%3B


->SPLITTING REQUIRE() STATEMENTS THAT USE && SAVES GAS

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#:~:text=require(gamma1%20!%3D%200%20%26%26%20gamma2%20!%3D%200%20%26%26%20volumeGamma%20!%3D%200%2C%20%27Gammas%20must%20be%20%3E%200%27)%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#:~:text=require(limitSqrtPrice%20%3C%20currentPrice%20%26%26%20limitSqrtPrice%20%3E%20TickMath.MIN_SQRT_RATIO%2C%20%27SPL%27)%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#:~:text=require(limitSqrtPrice%20%3E%20currentPrice%20%26%26%20limitSqrtPrice%20%3C%20TickMath.MAX_SQRT_RATIO%2C%20%27SPL%27)%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#:~:text=require((communityFee0%20%3C%3D,(communityFee0%2C%20communityFee1)%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#:~:text=require(newLiquidityCooldown%20%3C%3D,liquidityCooldown%20%3D%20newLiquidityCooldown%3B
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#:~:text=require(_feeConfig.gamma1%20!%3D%200%20%26%26%20_feeConfig.gamma2%20!%3D%200%20%26%26%20_feeConfig.volumeGamma%20!%3D%200%2C%20%27Gammas%20must%20be%20%3E%200%27)%3B


-> ++i costs less gas compared to i++ or i += 1 (Also --i costs less gas compared to i--- or i -= 1)

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#:~:text=secondsAgos.length%3B-,i%2B%2B),-%7B


-> USAGE OF UINTS/INTS SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD
When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#:~:text=function%20setBaseFeeConfiguration(-,uint16,-alpha1%2C
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#:~:text=uint16%20alpha1%2C-,uint16,-alpha2%2C
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#:~:text=uint32%20beta2%2C-,uint16,-gamma1%2C
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#:~:text=uint16%20gamma1%2C-,uint16,-gamma2%2C
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#:~:text=uint32%20volumeBeta%2C-,uint16,-volumeGamma%2C
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#:~:text=uint16%20volumeGamma%2C-,uint16,-baseFee
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#:~:text=liquidityBefore%20%3D%20liquidity%3B-,uint16,-newTimepointIndex%20%3D%20_writeTimepoint
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#:~:text=%7D-,uint16,-newTimepointIndex%20%3D%20_writeTimepoint
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#:~:text=%3E%200)%20%7B-,uint8,-_communityFeeToken0%20%3D%20globalState
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#:~:text=uint32%20secondsAgo%2C-,int24,-tick%2C
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#:~:text=int24%20_tick%2C-,uint16,-_index%2C
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#:~:text=struct%20Configuration%20%7B-,uint16,-alpha1%3B%20//%20max
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/Constants.sol#:~:text=library%20Constants%20%7B-,uint8,-internal%20constant%20RESOLUTION
