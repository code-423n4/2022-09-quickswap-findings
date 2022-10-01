    1.  Do not use '+=' ('_position.fees0 += fees0', found line 257 and 258 of ./core/contract/AlgebraPool.sol). Instead use "_position.fees0 = _position.fees0 + fees0" in order to save some gas.
    3.  Use '> 0' instead of '!= 0' to save some gas. ('require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0')' should be 'require(gamma1 > 0 && gamma2 > 0 && volumeGamma > 0, 'Gammas must be > 0')', found line 110 in /core/contracts/AlgebraFactory.sol) (saves 36 gas per use).
    4.  Use assembly to write storage value. Line 358 in _updatePositionTicksAndFees() of ./core/contracts/AlgebraPool.sol, instead of "globalState.timepointIndex = newTimepointIndex;", use  assembly {sstore(globalState.timepointIndex, newTimepointIndex)} to save gas.
    5.  Do not use '-=' ('amountRequired -= (step.input + step.feeAmount).toInt256();' line 801 ./core/contract/AlgebraPool.sol). Instead use "amountRequired = amountRequired - (step.input + step.feeAmount).toInt256()";
    6.  Splitting require() that use '&&' saves gas (found line 110 of ./src/core/contracts/AlgebraFactory.sol).
    7.  Using uint smaller than 32 bytes incurs overhead. Each operation involving a uint8 costs an extra 22-28 gas (found line 925 in ./core/contracts/AlgebraPool.sol).
    8.  Use private rather than public for constants, for exemple the poolDeployer address of AlgebraFactory (found 20 of /Users/olivierdemeaux/Desktop/Code/Auditing/2022-09-quickswap/src/core/contracts/AlgebraFactory.sol).