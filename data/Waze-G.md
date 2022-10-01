#1 Use require instead &&
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L110

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L46

use require instead of && can save  gas cost. we suggest to change it
example
before

    require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');

after

    require(gamma1 != 0, 'Gammas must be > 0');
    require(gamma2 != 0, 'Gammas must be > 0');
    require(volumeGamma != 0, 'Gammas must be > 0');

#2 Unsigned integer

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L898

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L52-L53

for unsigned integer, >0 is less efficient then !=0, so use !=0 instead of >0.
apply to others.

#3 Looping

https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L307

default uint is 0 so remove unnecassary explicit can reduce gas.
caching the secondsAgos.length in looping can reduce gas it caused access to a local variable is more cheap than query storage / calldata / memory in solidity.
pre increment e.g ++i more cheaper gas than post increment e.g i++. i suggest to use pre increment.

#4 Use  x = x + y or  x = x - y more cheap than x += y or x -= y for state variables
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L257-L258

We suggest to change the state to x = x + y or x = x - y for saving gas when possible.