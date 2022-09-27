## ABI.ENCODE() is less efficient than ABI.encodedpacked()
Changing abi.encode function to abi.encodePacked can save gas since the abi.encode function pads extra null bytes at the end of the call data, which is unnecessary. Also, in general, abi.encodePacked is more gas-efficient.
 `pool = address(uint256(keccak256(abi.encodePacked(hex'ff', poolDeployer, keccak256(abi.encode(token0, token1)), POOL_INIT_CODE_HASH))));`
[You can check this for more](https://github.com/ConnorBlockchain/Solidity-Encode-Gas-Comparison)