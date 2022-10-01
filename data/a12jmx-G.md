Changing i++ to ++i in the for loop of:

Contract: DataStorage.sol

	line 307

	will save around 5 gas per iteration.
	
Recommendation:

	for (uint256 i = 0; i < secondsAgos.length; ++i) {