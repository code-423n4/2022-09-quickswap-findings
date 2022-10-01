 ABI.ENCODEPACKED() SHOULD NOT BE USED WITH DYNAMIC TYPES WHEN PASSING THE RESULT TO A HASH FUNCTION SUCH AS KECCAK256()

Use abi.encode() instead which will pad items to 32 bytes, which will prevent hash collisions (e.g. abi.encodePacked(0x123,0x456) => 0x123456 => abi.encodePacked(0x1,0x23456), but abi.encode(0x123,0x456) => 0x0...1230...456). “Unless there is a compelling reason, abi.encode should be preferred”. If there is only one argument to abi.encodePacked() it can often be cast to bytes() or bytes32() instead. 

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L123