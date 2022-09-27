1. ++i  use less gas per iteration than  i++
2.  since the variable i is initialized to 0 by defualt , initializing it manually is considered as antipatttern and increase gas 



DataStorage.sol: 307 : 

 for (uint256 i = 0; i < secondsAgos.length; i++) { 

 should be changed to 

 for (uint256 i ; i < secondsAgos.length; ++i) {
