## INSTEAD OF USING `> 0 ` YOU SHOULD USE `!= 0`
In this if statement it is checking if communityFee  > 0 but communityFee is uint245 which can't be less then 0 so instead of using `> 0` use `!=0` it will save gas.
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L667
`    if (communityFee > 0) {`
There are lot of them in code i didn't mentioned all of them.

## ABI.ENCODE() is less efficient than ABI.encodedpacked()
Changing `abi.encode` function to `abi.encodePacked` can save gas since the `abi.encode` function pads extra null bytes at the end of the call data, which is unnecessary. Also, in general, `abi.encodePacked` is more gas-efficient.
 `pool = address(uint256(keccak256(abi.encodePacked(hex'ff', poolDeployer, keccak256(abi.encode(token0, token1)), POOL_INIT_CODE_HASH))));`
[You can check this for more](https://github.com/ConnorBlockchain/Solidity-Encode-Gas-Comparison)
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L123