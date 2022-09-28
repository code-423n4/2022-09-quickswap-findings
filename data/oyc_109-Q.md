## [L-01] abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256()

Use abi.encode() instead which will pad items to 32 bytes, which will prevent hash collisions (e.g. abi.encodePacked(0x123,0x456) => 0x123456 => abi.encodePacked(0x1,0x23456), but abi.encode(0x123,0x456) => 0x0...1230...456). Unless there is a compelling reason, abi.encode should be preferred. If there is only one argument to abi.encodePacked() it can often be cast to bytes() or bytes32() instead.

```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::123 => pool = address(uint256(keccak256(abi.encodePacked(hex'ff', poolDeployer, keccak256(abi.encode(token0, token1)), POOL_INIT_CODE_HASH))));
```

## [L-02] require() should be used instead of assert()

require() should be used for checking error conditions on inputs and return values while assert() should be used for invariant checking

```
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::190 => assert(false);
```

## [N-01] Use a more recent version of solidity

Use a solidity version of at least 0.8.4 to get bytes.concat() instead of abi.encodePacked(<bytes>,<bytes>)
Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>,<str>)
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions

```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/AlgebraPoolDeployer.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::2 => pragma solidity =0.7.6;
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::2 => pragma solidity =0.7.6;
```

## [N-02] Large multiples of ten should use scientific notation

Use (e.g. 1e6) rather than decimal literals (e.g. 1000000), for better code readability

```
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::16 => uint128 constant MAX_VOLUME_PER_LIQUIDITY = 100000 << 64; // maximum meaningful ratio of volume to liquidity
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::6 => uint256 internal constant Q96 = 0x1000000000000000000000000;
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::7 => uint256 internal constant Q128 = 0x100000000000000000000000000000000;
```

## [N-03] require()/revert() statements should have descriptive reason strings

Descriptive reason strings should be used so that users can troubleshot any reverted calls

```
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::43 => require(msg.sender == owner);
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::60 => require(tokenA != tokenB);
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::62 => require(token0 != address(0));
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::63 => require(poolByPair[token0][token1] == address(0));
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::78 => require(owner != _owner);
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::85 => require(farmingAddress != _farmingAddress);
2022-09-quickswap/src/core/contracts/AlgebraFactory.sol::92 => require(vaultAddress != _vaultAddress);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::55 => require(msg.sender == IAlgebraFactory(factory).owner());
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::122 => require(_lower.initialized);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::134 => require(_upper.initialized);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::229 => require((_blockTimestamp() - lastLiquidityAddTimestamp) >= _liquidityCooldown);
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::953 => require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::960 => require(msg.sender == IAlgebraFactory(factory).farmingAddress());
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::968 => require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
2022-09-quickswap/src/core/contracts/AlgebraPoolDeployer.sol::22 => require(msg.sender == factory);
2022-09-quickswap/src/core/contracts/AlgebraPoolDeployer.sol::27 => require(msg.sender == owner);
2022-09-quickswap/src/core/contracts/AlgebraPoolDeployer.sol::37 => require(_factory != address(0));
2022-09-quickswap/src/core/contracts/AlgebraPoolDeployer.sol::38 => require(factory == address(0));
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::43 => require(msg.sender == factory || msg.sender == IAlgebraFactory(factory).owner());
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::369 => require(!self[0].initialized);
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::52 => require(price > 0);
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::53 => require(liquidity > 0);
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::87 => require(price > quotient);
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::30 => require(priceDelta < priceUpper); // forbids underflow and 0 priceLower
2022-09-quickswap/src/core/contracts/libraries/TokenDeltaMath.sol::51 => require(priceUpper >= priceLower);
```

## [N-04] Missing NatSpec

Code should include NatSpec

```
2022-09-quickswap/src/core/contracts/libraries/Constants.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
```

## [N-06] Adding a return statement when the function defines a named return variable, is redundant

It is not necessary to have both a named return and a return statement.

```
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::407 => ) private view returns (Position storage) {
2022-09-quickswap/src/core/contracts/AlgebraPool.sol::557 => ) private returns (uint16 newTimepointIndex) {
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::115 => ) external view override onlyPool returns (uint112 TWVolatilityAverage, uint256 TWVolumePerLiqAverage) {
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::126 => ) external override onlyPool returns (uint16 indexUpdated) {
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::135 => ) external pure override returns (uint128 volumePerLiquidity) {
2022-09-quickswap/src/core/contracts/DataStorageOperator.sol::155 => ) external view override onlyPool returns (uint16 fee) {
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::29 => ) internal pure returns (uint16 fee) {
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::49 => ) internal pure returns (uint256 res) {
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::154 => ) private view returns (Timepoint storage beforeOrAt, Timepoint storage atOrAfter) {
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::213 => ) internal view returns (Timepoint memory targetTimepoint) {
2022-09-quickswap/src/core/contracts/libraries/DataStorage.sol::391 => ) internal returns (uint16 indexUpdated) {
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::25 => ) internal pure returns (uint160 resultPrice) {
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::41 => ) internal pure returns (uint160 resultPrice) {
2022-09-quickswap/src/core/contracts/libraries/PriceMovementMath.sol::51 => ) internal pure returns (uint160 resultPrice) {
2022-09-quickswap/src/core/contracts/libraries/TickManager.sol::137 => ) internal returns (int128 liquidityDelta) {
2022-09-quickswap/src/core/contracts/libraries/TickTable.sol::46 => function getMostSignificantBit(uint256 word) internal pure returns (uint8 mostBitPos) {
```

## [N-05] Lines are too long

Usually lines in source code are limited to 80 characters. Today's screens are much larger so it's reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that length

```
2022-09-quickswap/src/core/contracts/libraries/AdaptiveFee.sol::38 => return uint16(config.baseFee + sigmoid(volumePerLiquidity, config.volumeGamma, uint16(sumOfSigmoids), config.volumeBeta)); // safe since alpha1 + alpha2 + baseFee _must_ be <= type(uint16).max
```
