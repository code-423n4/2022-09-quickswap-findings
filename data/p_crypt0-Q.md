# QA
### [Informational] Redundant IF statement in collect()


In AlgebraPool.sol, there is an excessive compulsion to check for fee reductions.

The initial if statement:

````
if (amount0 | amount1 != 0) {
      position.fees0 = positionFees0 - amount0;
      position.fees1 = positionFees1 - amount1;
}
if (amount0 > 0) {
	TransferHelper.safeTransfer(token0, recipient, amount0);
}	
if (amount1 > 0) {
	TransferHelper.safeTransfer(token1, recipient, amount1);
}
````

[Original code is here](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L488-L510)

	  
	  Can become:
	  
````
	 if (amount0 > 0) {
	 position.fees0 = positionFees0 - amount0;
	 TransferHelper.safeTransfer(token0, recipient, amount0);
	 }
	 
	 if (amount1 > 0) {
	 position.fees1 = positionFees1 - amount1;
	 
	 TransferHelper.safeTransfer(token0, recipient, amount0);
	 }
````

This is both more simple to read and more effiicient since it deals with one less if statement.

The bitwise ``| ``  check in the first if statement is redundant, since it's essentially saying: 

*if amount0, or amount1, contains anything which isn't 0x000000 then update fees*.

Getting rid of this check would not impact the function of the code, since the amounts are checked to be above zero straight after.