### `DataStorage.getAverages()` contains unnecessary storage read

Location: [1](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/DataStorage.sol#L326)

#### Description

`DataStorage.getAverages()` contains a check to handle the case that the next index in an array of data has intentionally overflowed to 0.

```
uint16 oldestIndex;
Timepoint storage oldest = self[0];
uint16 nextIndex = index + 1; // considering overflow

if (self[nextIndex].initialized) {
    oldest = self[nextIndex];
    oldestIndex = nextIndex;
}
```

If the if case is true, then the array index has overflowed and thus `oldest` already points to the correct struct in the array. Thus the storage read is unnecessary, as is the assignment.

#### Suggested course of action

The warden ran the entire test suite and no failed tests were detected. As a result, it seems safe to remove the unneeded assignment `oldest = self[nextIndex];`

### Use multiple `require` statements instead of evaluating multiple conditions with `&&`

Locations:
[1](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/DataStorageOperator.sol#L46)
[2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L739)
[3](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L743)
[4](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L953)
[5](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L968)
[6](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L110)

#### Description
Using two (or more) `require` statements instead of a single `require` with an internal logical AND check is slightly cheaper and eases understanding of the checks.

#### Suggested course of action

Break up checks.

Note that the examples require enhanced deployment costs due to potentially repeating the same revert string. It could be beneficial to make the strings unique to enhance error reporting, or only return a string on the final check.

### Replace `> 0` checks with `!= 0` in `require` statements

Locations:
[1](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L224)
[2](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L434)
[3](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L469)
[4](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraPool.sol#L898)
[5](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L52)
[6](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/libraries/PriceMovementMath.sol#L53)

#### Description
When testing unsigned integers for non-zero values, it is slightly cheaper to use `!= 0` than to use `> 0`.

#### Suggested course of action

Implement the described change.

###  Remove `computeAddress` function and replace it's call with inlined computation, saving 48 gas per deploy.

Location: [1](https://github.com/code-423n4/2022-09-quickswap/blob/15ea643c85ed936a92d2676a7aabf739b210af39/src/core/contracts/AlgebraFactory.sol#L122)

#### Description

The function `AlgebraFactory.createPool()` makes a call to `AlgebraFactory.computeAddress()`. This function is only referenced once in the project, thus inflating the deployment cost of the contract.

#### Suggested course of action

Re-write `createPool` to inline the computation of the address, similar to the following:

```
    function createPool(address tokenA, address tokenB) external override returns (address pool) {
        require(tokenA != tokenB);
        (address token0, address token1) = tokenA < tokenB ? (tokenA, tokenB) : (tokenB, tokenA);
        require(token0 != address(0));
        require(poolByPair[token0][token1] == address(0));

        IDataStorageOperator dataStorage = 
            new DataStorageOperator(address(uint256(keccak256(abi.encodePacked( hex'ff', poolDeployer, keccak256(abi.encode(token0, token1)), POOL_INIT_CODE_HASH )))));

        dataStorage.changeFeeConfiguration(baseFeeConfiguration);

        pool = IAlgebraPoolDeployer(poolDeployer).deploy(address(dataStorage), address(this), token0, token1);

        poolByPair[token0][token1] = pool; // to avoid future addresses comparing we are populating the mapping twice
        poolByPair[token1][token0] = pool;
        emit Pool(token0, token1, pool);
    }
```
Additionally, it would be best to move `POOL_INIT_CODE_HASH` to the top of the contract where the other global values are, or move it into `Constants.sol`.