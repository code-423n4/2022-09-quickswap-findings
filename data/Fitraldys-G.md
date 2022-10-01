1. Useage of uint / int smaller than 32 bytes incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

POC :

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L99
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L100
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L103
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L104
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L106
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L107
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L276
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L277


2. use `abi.encodepacked` for gas optimization

Changing the `abi.encode` function to `abi.encodePacked` can save gas since the `abi.encode` function pads extra null bytes at the end of the call data, which is unnecessary. Also, in general, `abi.encodePacked` is more gas-efficient.

POC :

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L123
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L51


3. != is more efficient than > in require

`!= 0` cost less gas compare to `> 0` for unsigned integers in require statement with optimizer enable save 6 gas

because for uint the minimum value would be 0 and never a negative value, that mean check `> 0` is essentially checking that the value is not 0.

resource : https://twitter.com/gzeon/status/1485428085885640706

i suggest changing `> 0` to `!= 0`

POC :

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L434
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L469
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L898

4. Splitting `require()` statements that use && save gas

see https://github.com/code-423n4/2022-01-xdefi-findings/issues/128 which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper

POC :

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L739
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L743
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L953


5. cheaper foor loops

You can get cheaper for loops (at least 25 gas, however can be up to 80 gas under certain conditions), by rewriting:

```
for (uint256 i = 0; i < orders.length; /** NOTE: Removed i++ **/ ) {
        // Do the thing
        // Unchecked pre-increment is cheapest
        unchecked { ++i; }
}     
```

POC :

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307
