++i 
use less gas per iteration than 
 i++

DataStorage.sol: 307 : 

 for (uint256 i = 0; i < secondsAgos.length; i++) { 

 should be changed to 

 for (uint256 i = 0; i < secondsAgos.length; ++i) {
