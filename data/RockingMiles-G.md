## More efficient Struct packing of SwapCalculationCache in the contract AlgebraPool.sol


        The following structs could change the order of their stored elements to decrease memory uses.
        and number of occupied slots. Therefore will save gas at every store and load from memory.
        
In AlgebraPool.sol, SwapCalculationCache is optimized to: 8 slots from: 9 slots.
The new order of types (you choose the actual variables):
        1. uint256
        2. int256
        3. int256
        4. uint256
        5. uint256
        6. IAlgebraVirtualPool.Status
        7. uint160
        8. int56
        9. int24
        10. uint16
        11. uint128
        12. uint16
        13. bool
        14. bool






## State variables that could be set immutable

In the following files there are state variables that could be set immutable to save gas. 

### Code instance:

        owner in AlgebraPoolDeployer.sol



## Unused state variables

Unused state variables are gas consuming at deployment (since they are located in storage) and are 
a bad code practice. Removing those variables will decrease deployment gas cost and improve code quality. 
This is a full list of all the unused storage variables we found in your code base. 

### Code instances:

        AlgebraPoolDeployer.sol, parameters
        Constants.sol, RESOLUTION
        Constants.sol, Q96
        Constants.sol, MAX_LIQUIDITY_COOLDOWN
        Constants.sol, COMMUNITY_FEE_DENOMINATOR
        Constants.sol, MAX_COMMUNITY_FEE
        Constants.sol, BASE_FEE
        PoolState.sol, tickTable
        PoolState.sol, liquidityCooldown
        PoolState.sol, totalFeeGrowth0Token
        PoolState.sol, volumePerLiquidityInBlock
        PoolState.sol, activeIncentive
        Constants.sol, Q128
        PoolState.sol, ticks
        PoolState.sol, totalFeeGrowth1Token
        Constants.sol, MAX_LIQUIDITY_PER_TICK



## Caching array length can save gas


Caching the array length is more gas efficient.
This is because access to a local variable in solidity is more efficient than query storage / calldata / memory.
We recommend to change from:    

    for (uint256 i=0; i<array.length; i++) { ... }

to: 

    uint len = array.length  
    for (uint256 i=0; i<len; i++) { ... }


### Code instance:

        DataStorage.sol, secondsAgos, 307



## Prefix increments are cheaper than postfix increments

Prefix increments are cheaper than postfix increments. 
Further more, using unchecked {++x} is even more gas efficient, and the gas saving accumulates every iteration and can make a real change
There is no risk of overflow caused by increamenting the iteration index in for loops (the `++i` in `for (uint256 i = 0; i < numIterations; ++i)`).
But increments perform overflow checks that are not necessary in this case.
These functions use not using prefix increments (`++x`) or not using the unchecked keyword: 

### Code instance:

        change to prefix increment and unchecked: DataStorage.sol, i, 307



## Unnecessary index init


In for loops you initialize the index to start from 0, but it already initialized to 0 in default and this assignment cost gas. 
It is more clear and gas efficient to declare without assigning 0 and will have the same meaning:

### Code instance:

        DataStorage.sol, 307



## Rearrange state variables

You can change the order of the storage variables to decrease memory uses.

### Code instance:

In Constants.sol,rearranging the storage fields can optimize to: 4 slots from: 5 slots.
The new order of types (you choose the actual variables):
        1. uint256
        2. uint256
        3. uint256
        4. uint128
        5. uint32
        6. int24
        7. uint16
        8. uint8
        9. uint8




## Use != 0 instead of > 0


Using != 0 is slightly cheaper than > 0. (see https://github.com/code-423n4/2021-12-maple-findings/issues/75 for similar issue)


### Code instances:

        AlgebraPool.sol, 734: change 'amountRequired > 0' to 'amountRequired != 0'
        AlgebraPool.sol, 911: change 'amount1 > 0' to 'amount1 != 0'
        PriceMovementMath.sol, 52: change 'price > 0' to 'price != 0'
        AlgebraPool.sol, 237: change 'liquidityDelta > 0' to 'liquidityDelta != 0'
        AlgebraPool.sol, 645: change 'balance > 0' to 'balance != 0'
        FullMath.sol, 114: change 'denominator > 0' to 'denominator != 0'
        DataStorageOperator.sol, 138: change 'liquidity > 0' to 'liquidity != 0'
        PriceMovementMath.sol, 53: change 'liquidity > 0' to 'liquidity != 0'
        AlgebraPool.sol, 451: change 'balance > 0' to 'balance != 0'
        AlgebraPool.sol, 454: change 'balance > 0' to 'balance != 0'
        AlgebraPool.sol, 455: change 'balance > 0' to 'balance != 0'
        DataStorage.sol, 80: change 'liquidity > 0' to 'liquidity != 0'
        TickMath.sol, 52: change 'tick > 0' to 'tick != 0'
        AlgebraPool.sol, 434: change 'liquidityDesired > 0' to 'liquidityDesired != 0'
        AlgebraPool.sol, 641: change 'balance > 0' to 'balance != 0'
        AlgebraPool.sol, 452: change 'balance > 0' to 'balance != 0'
        DataStorageOperator.sol, 139: change 'liquidity > 0' to 'liquidity != 0'
        AlgebraPool.sol, 904: change 'amount0 > 0' to 'amount0 != 0'



## Unnecessary cast


    
### Code instances:

        int128 TokenDeltaMath.sol.getToken1Delta - unnecessary casting int128(liquidity)
        int128 LiquidityMath.sol.addDelta - unnecessary casting int128(y)
        int128 TokenDeltaMath.sol.getToken0Delta - unnecessary casting int128(liquidity)
        int256 PriceMovementMath.sol.movePriceTowardsTarget - unnecessary casting int256(amountAvailable)



## Consider inline the following functions to save gas


    You can inline the following functions instead of writing a specific function to save gas.
    (see https://github.com/code-423n4/2021-11-nested-findings/issues/167 for a similar issue.)

    
### Code instances

        AlgebraPool.sol, balanceToken1, { return IERC20Minimal(token1).balanceOf(address(this)); }
        AlgebraPool.sol, balanceToken0, { return IERC20Minimal(token0).balanceOf(address(this)); }
        PoolState.sol, _blockTimestamp, { return uint32(block.timestamp); // truncation is desired }
        PriceMovementMath.sol, getNewPriceAfterInput, { return getNewPrice(price, liquidity, input, zeroToOne, true); }



## Inline one time use functions


The following functions are used exactly once. Therefore you can inline them and save gas and improve code clearness.
    

### Code instances:

        FullMath.sol, mulDiv
        AlgebraPool.sol, _recalculatePosition
        DataStorage.sol, _volatilityOnRange
        DataStorage.sol, binarySearch
        LowGasSafeMath.sol, sub
        LowGasSafeMath.sol, add
        PriceMovementMath.sol, getNewPriceAfterInput
        PriceMovementMath.sol, getNewPriceAfterOutput
        TickTable.sol, getMostSignificantBit



## Upgrade pragma to at least 0.8.4


Using newer compiler versions and the optimizer gives gas optimizations
and additional safety checks are available for free.

The advantages of versions 0.8.* over <0.8.0 are:

        1. Safemath by default from 0.8.0 (can be more gas efficient than library based safemath.)
        2. Low level inliner : from 0.8.2, leads to cheaper runtime gas. Especially relevant when the contract has small functions. For example, OpenZeppelin libraries typically have a lot of small helper functions and if they are not inlined, they cost an additional 20 to 40 gas because of 2 extra jump instructions and additional stack operations needed for function calls.
        3. Optimizer improvements in packed structs: Before 0.8.3, storing packed structs, in some cases used an additional storage read operation. After EIP-2929, if the slot was already cold, this means unnecessary stack operations and extra deploy time costs. However, if the slot was already warm, this means additional cost of 100 gas alongside the same unnecessary stack operations and extra deploy time costs.
        4. Custom errors from 0.8.4, leads to cheaper deploy time cost and run time cost. Note: the run time cost is only relevant when the revert condition is met. In short, replace revert strings by custom errors.
    
### Code instances:

        IAlgebraFactory.sol
        AlgebraPool.sol
        IAlgebraPoolActions.sol
        Sqrt.sol
        AlgebraFactory.sol
        DataStorage.sol
        IAlgebraPoolState.sol
        IAlgebraPoolImmutables.sol
        AdaptiveFee.sol
        IAlgebraPool.sol
        PoolState.sol
        DataStorageOperator.sol
        TransferHelper.sol
        PoolImmutables.sol
        IAlgebraPoolDerivedState.sol
        IERC20Minimal.sol
        TickTable.sol
        Constants.sol
        TickManager.sol
        IAlgebraFlashCallback.sol
        IAlgebraPoolPermissionedActions.sol
        TokenDeltaMath.sol
        IAlgebraPoolDeployer.sol
        AlgebraPoolDeployer.sol
        IAlgebraVirtualPool.sol
        IAlgebraSwapCallback.sol
        IDataStorageOperator.sol
        TickMath.sol
        LowGasSafeMath.sol
        PriceMovementMath.sol
        FullMath.sol
        IAlgebraPoolEvents.sol
        SafeCast.sol
        IAlgebraMintCallback.sol
        LiquidityMath.sol



## Do not cache msg.sender


We recommend not to cache msg.sender since calling it is 2 gas while reading a variable is more.


### Code instances:

        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L52
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L33
        https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol#L33

