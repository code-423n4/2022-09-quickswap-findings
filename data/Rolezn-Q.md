Total Low Severity Findings: 5
Total Non-Critical Severity Findings: 6

## (1) Missing Checks for Address(0x0) 

Severity: Low

## Proof Of Concept


	function createPool(address tokenA, address tokenB) external override returns (address pool) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L59

	function setOwner(address _owner) external override onlyOwner {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L77

	function setFarmingAddress(address _farmingAddress) external override onlyOwner {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L84

	function setVaultAddress(address _vaultAddress) external override onlyOwner {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L91

	function deploy(
    address dataStorage,
    address _factory,
    address token0,
    address token1
  ) external override onlyFactory returns (address pool) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol#L44

## Recommended Mitigation Steps

Consider adding zero-address checks in the mentioned codebase.



## (2) Users can call public funcions that create something

Severity: Low

Users can potentially call functions that create and make it potentially malicicous XXX WORK ON DESCRIPTION XXX

## Proof Of Concept


	function createPool(address tokenA, address tokenB) external override returns (address pool) {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L59

	function getOrCreatePosition(
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L403

	function createNewTimepoint(
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L66



## Recommended Mitigation Steps

Consider adding a mapping of who can call these create functions to avoid malicious use

## (3) Low level calls with solidity version under 0.8.14 can result in optimizer bug

Severity: Low

Algebra contracts are using low level calls with solidity version before 0.8.14 which can result in optimizer bug.
https://medium.com/certora/overly-optimistic-optimizer-certora-bug-disclosure-2101e3f7994d

## Proof of Concept

For example a low level call in AlgebraPool.sol:

	pragma solidity =0.7.6;
	…
	  function getOrCreatePosition(
	    address owner,
	    int24 bottomTick,
	    int24 topTick[]
	  ) private view returns (Position storage) {
	    bytes32 key;
	    assembly {
	      key := or(shl(24, or(shl(24, owner), and(bottomTick, 0xFFFFFF))), and(topTick, 0xFFFFFF))
	    }
	    return positions[key];
	  }


POC can be found in the above medium reference url.
## Recommended Mitigation Steps
Consider upgrading to at least solidity v0.8.15.

## (4) Critical Changes Should Use Two-step Procedure

Severity: Low

The critical procedures should be two step process.

## Proof of Concept

	function setOwner(address _owner) external override onlyOwner {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L77

	function setFarmingAddress(address _farmingAddress) external override onlyOwner {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L84

	function setVaultAddress(address _vaultAddress) external override onlyOwner {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L91
	
	function setBaseFeeConfiguration(
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L98

	function setFactory(address _factory) external override onlyOwner {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol#L36

	function setCommunityFee(uint8 communityFee0, uint8 communityFee1) external override lock onlyFactoryOwner {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L952

	function setIncentive(address virtualPoolAddress) external override {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L959

	function setLiquidityCooldown(uint32 newLiquidityCooldown) external override onlyFactoryOwner {
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L967


## Recommended Mitigation Steps
Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.

## (5) Use SafeMath 
Severity: Low

The Algebra contracts are heavily math-based contracts, it is recommended to use SafeMath to avoid potential overflows/underflows.

## Recommended mitigation steps
Apply safemath or move to solidity 0.8.x

## (6) Variable Names That Consist Of All Capital Letters Should Be Reserved For Const/immutable Variables

Severity: Non-Critical

If the variable needs to be different based on which class it comes from, a view/pure function should be used instead.

## Proof Of Concept

	bytes32 internal constant POOL_INIT_CODE_HASH = 0x6ec6c9c8091d160c0aa74b2b14ba9c1717e95093bd3ac085cee99a49aab294a4;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L116




## (7) Missing parameter validation

Severity: Non-Critical

Some parameters of constructors are not checked for invalid values.

## Proof Of Concept

	address _poolDeployer
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L50

	address _vaultAddress
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L50

	address _pool
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L31

	address deployer
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolImmutables.sol#L29



## Recommended Mitigation Steps

Validate the parameters.


## (8) Non-usage of specific imports

Severity: Non-Critical

The current form of relative path import is not recommended for use because it can unpredictably pollute the namespace.
Instead, the Solidity docs recommend specifying imported symbols explicitly.
https://docs.soliditylang.org/en/v0.8.15/layout-of-source-files.html#importing-other-source-files

## Proof Of Concept


	import './interfaces/IAlgebraFactory.sol';
	import './interfaces/IAlgebraPoolDeployer.sol';
	import './interfaces/IDataStorageOperator.sol';
	import './libraries/AdaptiveFee.sol';
	import './DataStorageOperator.sol';
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L5-L9

	import './interfaces/IAlgebraPool.sol';
	import './interfaces/IDataStorageOperator.sol';
	import './interfaces/IAlgebraVirtualPool.sol';
	import './base/PoolState.sol';
	import './base/PoolImmutables.sol';
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L4-L9

	import './libraries/TokenDeltaMath.sol';
	import './libraries/PriceMovementMath.sol';
	import './libraries/TickManager.sol';
	import './libraries/TickTable.sol';
	import './libraries/LowGasSafeMath.sol';
	import './libraries/SafeCast.sol';
	import './libraries/FullMath.sol';
	import './libraries/Constants.sol';
	import './libraries/TransferHelper.sol';
	import './libraries/TickMath.sol';
	import './libraries/LiquidityMath.sol';
	import './interfaces/IAlgebraPoolDeployer.sol';
	import './interfaces/IAlgebraFactory.sol';
	import './interfaces/IERC20Minimal.sol';
	import './interfaces/callback/IAlgebraMintCallback.sol';
	import './interfaces/callback/IAlgebraSwapCallback.sol';
	import './interfaces/callback/IAlgebraFlashCallback.sol';
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L11-L30

	import './interfaces/IAlgebraPoolDeployer.sol';
	import './AlgebraPool.sol';
	import './interfaces/IAlgebraFactory.sol';
	import './interfaces/IDataStorageOperator.sol';
	import './libraries/DataStorage.sol';
	import './libraries/Sqrt.sol';
	import './libraries/AdaptiveFee.sol';
	import './libraries/Constants.sol';
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L4-L12

	import '../interfaces/pool/IAlgebraPoolImmutables.sol';
	import '../interfaces/IAlgebraPoolDeployer.sol';
	import '../libraries/Constants.sol';
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolImmutables.sol#L4-L6

	import '../interfaces/pool/IAlgebraPoolState.sol';
	import '../libraries/TickManager.sol';
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol#L4-L5

	import './Constants.sol';
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L4

	import './FullMath.sol';
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L4

	import './FullMath.sol';
	import './TokenDeltaMath.sol';
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L4-L5

	import './LowGasSafeMath.sol';
	import './SafeCast.sol';
	import './LiquidityMath.sol';
	import './Constants.sol';
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L4-L8

	import './Constants.sol';
	import './TickMath.sol';
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L4-L5

	import './LowGasSafeMath.sol';
	import './SafeCast.sol';
	import './FullMath.sol';
	import './Constants.sol';
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TokenDeltaMath.sol#L4-L8




## Recommended Mitigation Steps

Use specific imports syntax per solidity docs recommendation.



## (9) Use a more recent version of Solidity

Severity: Non-Critical

Use a solidity version of at least 0.8.4 to get bytes.concat() instead of abi.encodePacked(<bytes>,<bytes>)
Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>,<str>)
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions

## Proof Of Concept


	Found old version 0.7.6 of Solidity
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L2

	Found old version 0.7.6 of Solidity
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L2

	Found old version 0.7.6 of Solidity
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol#L2

	Found old version 0.7.6 of Solidity
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L2

	Found old version 0.7.6 of Solidity
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolImmutables.sol#L2

	Found old version 0.7.6 of Solidity
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/base/PoolState.sol#L2

	Found old version 0.7.6 of Solidity
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/AdaptiveFee.sol#L2

	Found old version 0.7.6 of Solidity
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/Constants.sol#L2

	Found old version 0.7.6 of Solidity
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L2

	Found old version 0.7.6 of Solidity
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L2

	Found old version 0.7.6 of Solidity
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickManager.sol#L2

	Found old version 0.7.6 of Solidity
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TickTable.sol#L2

	Found old version 0.7.6 of Solidity
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TokenDeltaMath.sol#L2



## Recommended Mitigation Steps

Consider updating to a more recent solidity version.



## (10) Require()/revert() Statements Should Have Descriptive Reason Strings

Severity: Non-Critical

## Proof Of Concept


	require(tokenA != tokenB);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L60

	require(token0 != address(0));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L62

	require(poolByPair[token0][token1] == address(0));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L63

	require(tokenA != tokenB);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L60

	require(token0 != address(0));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L62

	require(poolByPair[token0][token1] == address(0));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L63

	require(tokenA != tokenB);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L60

	require(token0 != address(0));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L62

	require(poolByPair[token0][token1] == address(0));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L63

	require(owner != _owner);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L78

	require(farmingAddress != _farmingAddress);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L85

	require(vaultAddress != _vaultAddress);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraFactory.sol#L92

	require(_lower.initialized);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L122

	require(_upper.initialized);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L134

	require(_lower.initialized);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L122

	require(_upper.initialized);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L134

	require((_blockTimestamp() - lastLiquidityAddTimestamp) >= _liquidityCooldown);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L229

	require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L953

	require(msg.sender == IAlgebraFactory(factory).farmingAddress());
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L960

	require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPool.sol#L968

	require(_factory != address(0));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol#L37

	require(factory == address(0));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol#L38

	require(_factory != address(0));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol#L37

	require(factory == address(0));
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/AlgebraPoolDeployer.sol#L38

	require(msg.sender == factory || msg.sender == IAlgebraFactory(factory).owner());
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L43

	require(!self[0].initialized);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/DataStorage.sol#L369

	require(price > 0);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L52

	require(liquidity > 0);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L53

	require((product = amount * price) / amount == price); // if the product overflows, we know the denominator underflows
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L70

	require(liquidityShifted > product); // in addition, we must check that the denominator does not underflow
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L71

	require(price > quotient);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L87

	require(price > 0);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L52

	require(liquidity > 0);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L53

	require((product = amount * price) / amount == price); // if the product overflows, we know the denominator underflows
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L70

	require(liquidityShifted > product); // in addition, we must check that the denominator does not underflow
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L71

	require(price > quotient);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L87

	require(price > 0);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L52

	require(liquidity > 0);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L53

	require((product = amount * price) / amount == price); // if the product overflows, we know the denominator underflows
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L70

	require(liquidityShifted > product); // in addition, we must check that the denominator does not underflow
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L71

	require(price > quotient);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L87

	require(price > 0);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L52

	require(liquidity > 0);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L53

	require((product = amount * price) / amount == price); // if the product overflows, we know the denominator underflows
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L70

	require(liquidityShifted > product); // in addition, we must check that the denominator does not underflow
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L71

	require(price > quotient);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L87

	require(price > 0);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L52

	require(liquidity > 0);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L53

	require((product = amount * price) / amount == price); // if the product overflows, we know the denominator underflows
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L70

	require(liquidityShifted > product); // in addition, we must check that the denominator does not underflow
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L71

	require(price > quotient);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/PriceMovementMath.sol#L87

	require(priceDelta < priceUpper); // forbids underflow and 0 priceLower
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TokenDeltaMath.sol#L30

	require(priceUpper >= priceLower);
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/libraries/TokenDeltaMath.sol#L51






## (11) Large multiples of ten should use scientific notation

Severity: Non-Critical

Use (e.g. 1e6) rather than decimal literals (e.g. 100000), for better code readability.

## Proof Of Concept


  uint128 constant MAX_VOLUME_PER_LIQUIDITY = 100000 << 64;
https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts/DataStorageOperator.sol#L16


