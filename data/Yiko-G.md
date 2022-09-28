In AlgebraPool.sol
When comparing uints to 0 "> 0" widely used. As uints are always equal to or bigger than 0, if "!= 0" is used instead it could save gas. Some lines of this case are below:
https://github.com/code-423n4/2022-09-quickswap/blob/59c93c65443f9dec68473ca82740abfd8dd47ac3/src/core/contracts/AlgebraPool.sol#L224
https://github.com/code-423n4/2022-09-quickswap/blob/59c93c65443f9dec68473ca82740abfd8dd47ac3/src/core/contracts/AlgebraPool.sol#L228
https://github.com/code-423n4/2022-09-quickswap/blob/59c93c65443f9dec68473ca82740abfd8dd47ac3/src/core/contracts/AlgebraPool.sol#L434

In DataStorage.sol, at line 307
Using "++i" instead of "i++" might save gas.
https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L307