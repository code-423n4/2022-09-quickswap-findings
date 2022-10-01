You can get cheaper for loops (at least 25 gas, however can be up to 80 gas under certain conditions), by rewriting:

        for (uint256 i = 0; i  < secondsAgos.length; /** NOTE: Removed i++ **/ ) {
                // Do the thing
                // Unchecked pre-increment is cheapest
                unchecked { ++i; }

INSTANCES:

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307