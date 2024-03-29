## USE CALLDATA INSTEAD OF MEMORY

When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the `memory` index. Each iteration of this for-loop costs at least 60 gas (i.e. `60 * <mem_array>.length`). Using `calldata` directly, obliviates the need for such a loop in the contract code and runtime execution.

When arguments are read-only on external functions, the data location should be `calldata`

3 Instances:
-DataStorageOperator.sol line 90
-AdaptiveFee.sol line 28
-DataStorage line 280



## <ARRAY>.LENGTH SHOULD NOT BE LOOKED UP IN EVERY LOOP OF A FOR-LOOP

Reading array length at each iteration of the loop consumes more gas than necessary.
In the best case scenario (length read on a memory variable), caching the array length in the stack saves around 3 gas per iteration. In the worst case scenario (external calls at each iteration), the amount of gas wasted can be massive.

Consider storing the array’s length in a variable before the for-loop, and use this new variable instead

1 Instance:
-DataStorage.sol line 307


## <X> += <Y> COSTS MORE GAS THAN <X> = <X> + <Y> FOR STATE VARIABLES

4 Instances:
-AlgebraPool.sol lines 257, 258, 931, 945



## USE MULTIPLE `REQUIRE()` STATMENTS INSTEAD OF `REQUIRE(EXPRESSION && EXPRESSION && ...)`
1 Instance:
-AlgebraFactory.sol line 110
-AlgebraPool.sol lines 953, 968
-DataStorageOperator.sol line 46 


## IT COSTS MORE GAS TO INITIALIZE VARIABLES WITH THEIR DEFAULT VALUE THAN LETTING THE DEFAULT VALUE BE APPLIED

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: for (uint256 i = 0; i < numIterations; ++i) { should be replaced with for (uint256 i; i < numIterations; ++i) {

1 Instance:
-DataStorage.sol line 307


## `>=` COSTS LESS GAS THAN `>`

The compiler uses opcodes `GT` and `ISZERO` for solidity code that uses `>`, but only requires `LT` for `>=`, which saves 3 gas

27 Instances:
-AlgebraPool.sol lines 224, 228, 237, 434, 451, 452, 454, 455, 505, 506, 617,  641, 645, 667, 734, 808, 814, 898, 904, 911, 924, 927, 938, 941
-DataStorageOperator.sol line 138


## `++I/I++` SHOULD BE `UNCHECKED{++I}`/`UNCHECKED{I++}` WHEN IT IS NOT POSSIBLE FOR THEM TO OVERFLOW, AS IS THE CASE WHEN USED IN FOR- AND WHILE-LOOPS

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop
`
   for (uint256 i = 0; i < orders.length; /** NOTE: Removed i++ **/ ) {
           // Do the thing
           // Unchecked pre-increment is cheapest
           unchecked { ++i; }   
}  `
1 Instance:
-DataStorage.sol line 307


## USING `PRIVATE` RATHER THAN `PUBLIC` FOR CONSTANTS, SAVES GAS

If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table

1 Instance:
-DataStorage.sol line 12

## USE A MORE RECENT VERSION OF SOLIDITY

Use a solidity version above 0.8.0


