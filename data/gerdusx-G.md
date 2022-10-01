 ## Gas Optimizations
 ### [G-1] Use prefix not postfix in loops
 Using a prefix increment (++i) instead of a postfix increment (i++) saves gas for each loop cycle and so can have a big gas impact when the loop executes on a large number of elements. 

 There is 1 occurrence:
```
 DataStorage.sol:307	     for (uint256 i = 0; i < secondsAgos.length; i++) {
```
 ### [G-2] Redundant zero initialization
 Solidity does not recognize null as a value, so uint variables are initialized to zero. Setting a uint variable to zero is redundant and can waste gas.

Remove the redundant zero initialization

````uint256 amount;```` 

 There is 1 occurrence:
```
 DataStorage.sol:307	     for (uint256 i = 0; i < secondsAgos.length; i++) {
```
