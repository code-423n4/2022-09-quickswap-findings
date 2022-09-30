# Gas Report
## Optimizations found [5]

### [G-01] Use != 0 instead of > 0 for Unsigned Integer Comparison [ⓘ](https://github.com/devanshbatham/Solidity-Gas-Optimization-Tips#27--use--0-instead-of--0-for-unsigned-integer-comparison)

#### Findings:

*/src/core/contracts/AlgebraPool.sol*
Line(s): [224](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L224), [228](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L228), [237](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L237) (NOT liquidityDelta), [434](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L434), [451](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L451), [452](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L452), [454](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L454) (`(amount0 != 0)`), [455](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L455) (`(amount0 != 0)`), [469](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L469), [505](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L505), [506](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L506), [617](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L617), [667](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L667), [808](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L808), [814](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L814), [898](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L898), [904](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L904), [911](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L911), [924](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L924), [927](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L927), [938](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L938), [941](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L941)
```
224:	require(currentLiquidity > 0, 'NP');
228:	if (_liquidityCooldown > 0) {
237:	liquidityNext > 0 ? (liquidityDelta > 0 ? _blockTimestamp() : lastLiquidityAddTimestamp) : 0
434:	require(liquidityDesired > 0, 'IL');
451:	if (amount0 > 0) receivedAmount0 = balanceToken0();
452:	if (amount1 > 0) receivedAmount1 = balanceToken1();
454:	if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');
455:	if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');
469:	require(liquidityActual > 0, 'IIL2');
505:	if (amount0 > 0) TransferHelper.safeTransfer(token0, recipient, amount0);
506:	if (amount1 > 0) TransferHelper.safeTransfer(token1, recipient, amount1);
617:	if (communityFee > 0) {
667:	if (communityFee > 0) {
808:	if (cache.communityFee > 0) {
814:	if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);
898:	require(_liquidity > 0, 'L');
904:	if (amount0 > 0) {
911:	if (amount1 > 0) {
924:	if (paid0 > 0) {
927:	if (_communityFeeToken0 > 0) {
938:	if (paid1 > 0) {
941:	if (_communityFeeToken1 > 0) {
```

### [G-02] Break Apart Require Statments Instead of && [ⓘ](https://yos.io/2021/05/17/gas-efficient-solidity/#tip-11-splitting-require-statements-that-use--saves-gas)

#### Findings:

*/src/core/contracts/AlgebraFactory.sol*
Line(s): [110](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L110)
```
110:	require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
```

*/src/core/contracts/AlgebraPool.sol*
Line(s): [739](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L739), [743](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L743), [953](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L953), [968](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L968)
```
739:	require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
743:	require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
953:	require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));
968:	require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
```


### [G-03] Unless Used for Variable Packing uint8 May Be More Expensive Than Using uint256 [ⓘ](https://yos.io/2021/05/17/gas-efficient-solidity/#tip-15-usage-of-uint8-may-increase-gas-cost)

#### Findings:

*/src/core/contracts/AlgebraPool.sol*
Line(s): [925](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L925), [939](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L939)
```
925:	uint8 _communityFeeToken0 = globalState.communityFeeToken0;
939:	uint8 _communityFeeToken1 = globalState.communityFeeToken1;
```

*/src/core/contracts/libraries/Constants.sol*
Line(s): [5](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/Constants.sol#L5), [16](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/Constants.sol#L16)
```
5:	uint8 internal constant RESOLUTION = 96;
16:	uint8 internal constant MAX_COMMUNITY_FEE = 250;
```

*/src/core/contracts/libraries/TickTable.sol*
Line(s): [18](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L18), [84](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L84), [102](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L102)
```
18:	uint8 bitNumber;
84:	uint8 bitNumber;
102:	uint8 bitNumber;
```

*/src/core/contracts/base/PoolState.sol*
Line(s): [13](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolState.sol#L13), [14](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolState.sol#L14)
```
13:	uint8 communityFeeToken0;
14:	uint8 communityFeeToken1;
```

### [G-04] Variables Only Used Through Initialization in Constructor Should Be Immutable [ⓘ](https://yos.io/2021/05/17/gas-efficient-solidity/#tip-7-use-constant-and-immutable-keywords)

*src/core/contracts/AlgebraPoolDeployer.sol*
Line(s): [13](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L19)
```
19:	address private owner;
```

### [G-05] Make Constants Private to Prevent Unneeded Getters

*src/core/contracts/DataStorageOperator.sol*
Line(s): [15](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L15), [16](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L16)
```
15:	uint256 constant UINT16_MODULO = 65536;
16:	uint128 constant MAX_VOLUME_PER_LIQUIDITY = 100000 << 64;
```