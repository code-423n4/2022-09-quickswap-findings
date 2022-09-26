
## Missing Slippage Protection

Missing slipage protection may lead to losing assets while swapping them. Without slipage protection the swapper is allowed to give much less worth of target tokens than it should in a fair swap.
   to Missing slippage protection at:
 
### Code instance:

        no slippage protection at swapSupportingFeeOnInputTokens at AlgebraPool.sol at line 640



## Unsafe Cast

use openzeppilin's safeCast in:
 
### Code instances:

        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L25 : unsafe cast uint16(sumOfSigmoids)
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/SafeCast.sol#L12 : unsafe cast uint160(y)
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L148 : unsafe cast uint16(current)
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L131 : unsafe cast uint128(volumeShifted)



## Two arrays length mismatch


The functions below fail to perform input validation on arrays to verify the lengths match.
A mismatch could lead to an exception or undefined behavior.
Consider making this a medium risk please.
    
    
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L277 getTimepoints ['self', 'secondsAgos']




## Div by 0


Division by 0 can lead to accidentally revert,
(An example of a similar issue - https://github.com/code-423n4/2021-10-defiprotocol-findings/issues/84)

### Code instances:

        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L353 time, oldest, oldestTimestamp might be 0
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/FullMath.sol#L113 a might be 0
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L414 blockTimestamp, last might be 0
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L62 amount might be 0
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L256 time might be 0
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L67 amount might be 0
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L255 time might be 0




## Not verified owner


        owner param should be validated to make sure the owner address is not address(0).
        Otherwise if not given the right input all only owner accessible functions will be unaccessible.
        
        
### Code instance:

        AlgebraFactory.sol.setOwner _owner



## Named return issue

Users can mistakenly think that the return value is the named return, but it is actually the actualreturn statement that comes after. To know that the user needs to read the code and is confusing.
Furthermore, removing either the actual return or the named return will save gas. 

### Code instances:

        AlgebraPool.sol, timepoints
        DataStorageOperator.sol, calculateVolumePerLiquidity
        PriceMovementMath.sol, getNewPriceAfterInput
        AlgebraPool.sol, swapSupportingFeeOnInputTokens
        TickTable.sol, getMostSignificantBit
        DataStorageOperator.sol, getFee
        DataStorageOperator.sol, getAverages
        DataStorage.sol, write
        TickManager.sol, cross
        FullMath.sol, mulDiv
        AlgebraPool.sol, _writeTimepoint
        DataStorageOperator.sol, write
        DataStorage.sol, binarySearch
        TickTable.sol, nextTickInTheSameRow
        DataStorage.sol, getAverages
        PriceMovementMath.sol, getNewPrice
        PriceMovementMath.sol, getNewPriceAfterOutput
        AdaptiveFee.sol, getFee
        DataStorageOperator.sol, getTimepoints
        DataStorage.sol, getSingleTimepoint
        AlgebraPool.sol, _getSingleTimepoint
        AlgebraPool.sol, getInnerCumulatives
        AdaptiveFee.sol, sigmoid



## Two Steps Verification before Transferring Ownership

The following contracts have a function that allows them an admin to change it to a different address. If the admin accidentally uses an invalid address for which they do not have the private key, then the system gets locked.
It is important to have two steps admin change where the first is announcing a pending new admin and the new address should then claim its ownership. 
A similar issue was reported in a previous contest and was assigned a severity of medium: [code-423n4/2021-06-realitycards-findings#105](https://github.com/code-423n4/2021-06-realitycards-findings/issues/105) 

### Code instances:

        IAlgebraFactory.sol
        AlgebraFactory.sol



## Assert instead require to validate user inputs


        From solidity docs: Properly functioning code should never reach a failing assert statement; if this happens there is a bug in your contract which you should fix.
        With assert the user pays the gas and with require it doesn't. The ETH network gas isn't cheap and users can see it as a scam.
        
### Code instance:

        DataStorage.sol : reachable assert in line 189



## Never used parameters

Those are functions and parameters pairs that the function doesn't use the parameter. In case those functions are external/public this is even worst since the user is required to put value that never used and can misslead him and waste its time. 

### Code instances:

        AlgebraPoolDeployer.sol: function deploy parameter dataStorage isn't used. (deploy is external)
        AlgebraPoolDeployer.sol: function deploy parameter _factory isn't used. (deploy is external)
        AlgebraPoolDeployer.sol: function deploy parameter token0 isn't used. (deploy is external)
        AlgebraPoolDeployer.sol: function deploy parameter token1 isn't used. (deploy is external)



## Check transfer receiver is not 0 to avoid burned money


Transferring tokens to the zero address is usually prohibited to accidentally avoid "burning" tokens by sending them to an unrecoverable zero address.


### Code instances:

        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L479
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L656
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L604
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L505
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L906
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L548



## Missing commenting


        The following functions are missing commenting as describe below:
        
### Code instance:

        DataStorage.sol, _getAverageTick (internal), parameters self, time, tick, index, oldestIndex, lastTimestamp, lastTickCumulative not commented



## Add a timelock

To give more trust to users: functions that set key/critical variables should be put behind a timelock.

### Code instances:

        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L91
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L959
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L98
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L967
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L77
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L84
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L952
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol#L36





## Missing fee parameter validation


Some fee parameters of functions are not checked for invalid values. Validate the parameters:
        
        
### Code instances:

        PriceMovementMath.movePriceTowardsTarget (fee)
        TickManager.getInnerFeeGrowth (totalFeeGrowth1Token)
        TickManager.update (totalFeeGrowth0Token)
        AlgebraPool.setCommunityFee (communityFee0)
        TickManager.getInnerFeeGrowth (totalFeeGrowth0Token)
        AlgebraPool.setCommunityFee (communityFee1)
        TickManager.cross (totalFeeGrowth0Token)
        TickManager.cross (totalFeeGrowth1Token)
        DataStorageOperator.changeFeeConfiguration (_feeConfig)
        TickManager.update (totalFeeGrowth1Token)
        AlgebraPool._recalculatePosition (innerFeeGrowth1Token)
        AlgebraFactory.setBaseFeeConfiguration (baseFee)
        AlgebraPool._recalculatePosition (innerFeeGrowth0Token)



## Require with empty message

The following requires are with empty messages. 
This is very important to add a message for any require. So the user has enough information to know the reason of failure.
### Code instances:

        Solidity file: AlgebraFactory.sol, In line 62 with Empty Require message.
        Solidity file: AlgebraPoolDeployer.sol, In line 38 with Empty Require message.
        Solidity file: AlgebraFactory.sol, In line 63 with Empty Require message.
        Solidity file: SafeCast.sol, In line 27 with Empty Require message.
        Solidity file: PriceMovementMath.sol, In line 53 with Empty Require message.
        Solidity file: DataStorageOperator.sol, In line 43 with Empty Require message.
        Solidity file: LowGasSafeMath.sol, In line 46 with Empty Require message.
        Solidity file: AlgebraPool.sol, In line 953 with Empty Require message.
        Solidity file: AlgebraPoolDeployer.sol, In line 37 with Empty Require message.
        Solidity file: AlgebraFactory.sol, In line 85 with Empty Require message.
        Solidity file: SafeCast.sol, In line 20 with Empty Require message.
        Solidity file: AlgebraPool.sol, In line 960 with Empty Require message.
        Solidity file: DataStorage.sol, In line 369 with Empty Require message.
        Solidity file: AlgebraFactory.sol, In line 78 with Empty Require message.
        Solidity file: TokenDeltaMath.sol, In line 51 with Empty Require message.
        Solidity file: AlgebraPool.sol, In line 968 with Empty Require message.
        Solidity file: PriceMovementMath.sol, In line 52 with Empty Require message.
        Solidity file: FullMath.sol, In line 33 with Empty Require message.
        Solidity file: AlgebraFactory.sol, In line 92 with Empty Require message.
        Solidity file: LowGasSafeMath.sol, In line 30 with Empty Require message.
        Solidity file: TokenDeltaMath.sol, In line 30 with Empty Require message.
        Solidity file: LowGasSafeMath.sol, In line 54 with Empty Require message.
        Solidity file: LowGasSafeMath.sol, In line 14 with Empty Require message.
        Solidity file: LowGasSafeMath.sol, In line 38 with Empty Require message.
        Solidity file: AlgebraFactory.sol, In line 60 with Empty Require message.
        Solidity file: LowGasSafeMath.sol, In line 22 with Empty Require message.
        Solidity file: SafeCast.sol, In line 13 with Empty Require message.



## Require with not comprehensive message

The following requires has a non comprehensive messages. 
This is very important to add a comprehensive message for any require. Such that the user has enough 
information to know the reason of failure: 

### Code instances:

        Solidity file: AlgebraPool.sol, In line 898 with Require message: L
        Solidity file: TickMath.sol, In line 29 with Require message: T
        Solidity file: TransferHelper.sol, In line 22 with Require message: TF
        Solidity file: TickMath.sol, In line 67 with Require message: R
        Solidity file: AlgebraPool.sol, In line 921 with Require message: F0
        Solidity file: AlgebraPool.sol, In line 636 with Require message: LOK
        Solidity file: TickManager.sol, In line 96 with Require message: LO
        Solidity file: AlgebraPool.sol, In line 194 with Require message: AI
        Solidity file: DataStorage.sol, In line 238 with Require message: OLD
        Solidity file: AlgebraPool.sol, In line 935 with Require message: F1
        Solidity file: AlgebraPool.sol, In line 469 with Require message: IIL2
        Solidity file: AlgebraPool.sol, In line 434 with Require message: IL



## Not verified input


external / public functions parameters should be validated to make sure the address is not 0.
Otherwise if not given the right input it can mistakenly lead to loss of user funds.    

### Code instances:

        AlgebraPool.sol.swapSupportingFeeOnInputTokens sender
        AlgebraPool.sol.swapSupportingFeeOnInputTokens recipient
        AlgebraPoolDeployer.sol.deploy _factory
        AlgebraFactory.sol.createPool tokenB
        AlgebraPoolDeployer.sol.deploy token0
        AlgebraFactory.sol.setFarmingAddress _farmingAddress
        AlgebraPoolDeployer.sol.setFactory _factory
        AlgebraPool.sol.collect recipient
        AlgebraPool.sol.mint sender
        AlgebraFactory.sol.setVaultAddress _vaultAddress
        AlgebraPool.sol.setIncentive virtualPoolAddress
        AlgebraPool.sol.flash recipient
        AlgebraPool.sol.swap recipient
        AlgebraFactory.sol.createPool tokenA
        AlgebraPoolDeployer.sol.deploy dataStorage
        AlgebraPool.sol.mint recipient
        AlgebraPoolDeployer.sol.deploy token1



## Solidity compiler versions mismatch


The project is compiled with different versions of solidity, which is not recommended because it can lead to undefined behaviors.
        
### Code instance:

        



## Use safe math for solidity version <8

You should use safe math for solidity version <8 since there is no default over/under flow check it suchversions of solidity.
### Code instances:

        The contract IAlgebraPoolState.sol doesn't use safe math and is of solidity version < 8
        The contract DataStorageOperator.sol doesn't use safe math and is of solidity version < 8
        The contract AdaptiveFee.sol doesn't use safe math and is of solidity version < 8
        The contract TickMath.sol doesn't use safe math and is of solidity version < 8
        The contract IAlgebraPoolActions.sol doesn't use safe math and is of solidity version < 8