### [G001] Don't Initialize Variables with Default Value

#### Impact

Uninitialized variables are assigned with the types default value.
Explicitly initializing a variable with it's default value costs unnecessary gas.

```solidity
// from
uint i = 0;
bool b = false;

// to
uint i;
bool b;
```

#### Findings:
```solidity
src/core/contracts/libraries/DataStorage.sol::307 => for (uint256 i = 0; i < secondsAgos.length; i++) {
```

### [G002] Cache Array Length Outside of Loops

#### Impact

Cache the length of arrays in loops. **(~6 gas per iteration)**
The overheads outlined below are PER LOOP, excluding the first loop
 - storage arrays incur a Gwarmaccess (100 gas)
 - memory arrays use MLOAD (3 gas)
 - calldata arrays use CALLDATALOAD (3 gas)

```solidity
// from
for (uint256 i; i < array.length; ++i) {

// to
uint256 length = array.length;
for (uint256 i; i < length; ++i) {
```

#### Findings:
```solidity
src/core/contracts/libraries/DataStorage.sol::307 => for (uint256 i = 0; i < secondsAgos.length; i++) {
```

### [G003] Using > 0 costs more gas than != 0 when used on a uint in a require() statement

#### Impact

When dealing with unsigned integer types, comparisons with `!= 0` are cheaper then with `> 0`. **(~6 gas per instance)**

```solidity
// from
require(amount > 0);

// to
require(amount != 0);
```

#### Findings:
```solidity
src/core/contracts/AlgebraFactory.sol::110 => require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
src/core/contracts/AlgebraPool.sol::434 => require(liquidityDesired > 0, 'IL');
src/core/contracts/AlgebraPool.sol::454 => if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');
src/core/contracts/AlgebraPool.sol::455 => if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');
src/core/contracts/AlgebraPool.sol::469 => require(liquidityActual > 0, 'IIL2');
src/core/contracts/AlgebraPool.sol::641 => require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');
src/core/contracts/AlgebraPool.sol::645 => require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');
src/core/contracts/AlgebraPool.sol::898 => require(_liquidity > 0, 'L');
src/core/contracts/DataStorageOperator.sol::46 => require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
src/core/contracts/libraries/PriceMovementMath.sol::52 => require(price > 0);
src/core/contracts/libraries/PriceMovementMath.sol::53 => require(liquidity > 0);
```

### [G004] Functions guaranteed to revert when called by normal users can be marked as payable

#### Impact

If a function modifier suck as onlyOwner | onlyAdmin is used, the function will revert if a normal user tries to pay the function.
Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.
The extra opcodes avoided are:
CALLVALUE (2 gas), DUP1(3 gas), ISZERO (3 gas), PUSH2(3 gas), JUMPI (10 gas), PUSH1(3 gas), DUPI(3 gas), REVERT(0), JUMPDEST (1 gas), POP (2 gas)
Total: **30 gas saved per call** approx to the function, in addition to the extra deployment cost.

```solidity
// from
function foo() external onlyOwner {

// to
function foo() external payable onlyOwner {
```

#### Findings:
```solidity
src/core/contracts/AlgebraFactory.sol::77 => function setOwner(address _owner) external override onlyOwner {
src/core/contracts/AlgebraFactory.sol::84 => function setFarmingAddress(address _farmingAddress) external override onlyOwner {
src/core/contracts/AlgebraFactory.sol::91 => function setVaultAddress(address _vaultAddress) external override onlyOwner {
src/core/contracts/AlgebraFactory.sol::108 => ) external override onlyOwner {
src/core/contracts/AlgebraPoolDeployer.sol::36 => function setFactory(address _factory) external override onlyOwner {
```

### [G006] ++i costs less gas than i++, especially when it's used in for-loops (--i/i-- too)

#### Impact

Use `++i` instead of `i++`, specially in loops. Find searching `i++` (~5 gas saved per iteration)
This is especially useful in for loops but this optimization can be used anywhere in your code. You can also use `unchecked{++i;}` for even more gas savings but this will not check to see if `i` overflows, which I do not recommend. For extra safety if you are worried about this, you can add a require statement after the loop checking if `i` is equal to the final incremented value. For best gas savings, use inline assembly, however this limits the functionality you can achieve. For example you cant use Solidity syntax to internally call your own contract within an assembly block and external calls must be done with the `call()` or `delegatecall()` instruction. However when applicable, inline assembly will save much more gas.

```solidity
// from
for (uint256 i; i < 100; i++) {

// to
for (uint256 i; i < 100; ++i) {
```

#### Findings:
```solidity
src/core/contracts/libraries/DataStorage.sol::307 => for (uint256 i = 0; i < secondsAgos.length; i++) {
```

### [G007] Use assembly to check for address 0

#### Impact

Checking for address 0 can be done with assembly, which is cheaper. **(~6 gas per instance)**

```solidity
// from
require(_addr != address(0));

// to
assembly {
  if iszero(_addr) {
    mstore(0x00, "zero address")
    revert(0x00, 0x20)
  }
}
```

#### Findings:
```solidity
src/core/contracts/AlgebraFactory.sol::62 => require(token0 != address(0));
src/core/contracts/AlgebraFactory.sol::63 => require(poolByPair[token0][token1] == address(0));
src/core/contracts/AlgebraPoolDeployer.sol::37 => require(_factory != address(0));
src/core/contracts/AlgebraPoolDeployer.sol::38 => require(factory == address(0));
```

### [G009] Use `private` rather than `public` for constants

#### Impact

Using `private` rather than `public` for constants, saves gas **(~3,500 gas saved on deployment per constant)**
If needed, the value can be read from the verified contract source code. Saves ~3,500 gas in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

```solidity
// from
uint256 public constant FOO = 1;

// to
uint256 private constant FOO = 1;
```

#### Findings:
```solidity
src/core/contracts/libraries/DataStorage.sol::12 => uint32 public constant WINDOW = 1 days;
```

### [G010] Use multiple `require()` statements instead of `&&`

#### Impact

Using multiple `require()` statements instead of `&&` saves gas **(~50 gas fee per require statement broken down)**

```solidity
// from
require(a && b && c);

// to
require(a);
require(b);
require(c);
```

#### Findings:
```solidity
src/core/contracts/AlgebraFactory.sol::110 => require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
src/core/contracts/AlgebraPool.sol::739 => require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
src/core/contracts/AlgebraPool.sol::743 => require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
src/core/contracts/AlgebraPool.sol::953 => require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));
src/core/contracts/AlgebraPool.sol::968 => require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
src/core/contracts/DataStorageOperator.sol::46 => require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
```

### [G012] Use custom errors instead of revert strings

#### Impact

Use custom errors instead of revert strings. (~50 gas saved per call, ~5,500 gas saved on deployment)
Custom errors from Solidity 0.8.4 and upwards are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met) while providing the same amount of information, as explained [here](https://blog.soliditylang.org/2021/04/21/custom-errors/)

It not only saves gas upon deployment - ~5500 gas saved per custom error instead of a require statement, but it is also cheaper in a function call, custom errors save ~50 gas each time they're hit by avoiding having to allocate and store the revert string.

```solidity
// from
require(msg.sender != owner)

// to
error Unauthorized();
if (msg.sender != owner) {
  revert Unauthorized();
}
```

#### Findings:
```solidity
src/core/contracts/AlgebraFactory.sol::43 => require(msg.sender == owner);
src/core/contracts/AlgebraFactory.sol::60 => require(tokenA != tokenB);
src/core/contracts/AlgebraFactory.sol::62 => require(token0 != address(0));
src/core/contracts/AlgebraFactory.sol::63 => require(poolByPair[token0][token1] == address(0));
src/core/contracts/AlgebraFactory.sol::78 => require(owner != _owner);
src/core/contracts/AlgebraFactory.sol::85 => require(farmingAddress != _farmingAddress);
src/core/contracts/AlgebraFactory.sol::92 => require(vaultAddress != _vaultAddress);
src/core/contracts/AlgebraFactory.sol::109 => require(uint256(alpha1) + uint256(alpha2) + uint256(baseFee) <= type(uint16).max, 'Max fee exceeded');
src/core/contracts/AlgebraFactory.sol::110 => require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
src/core/contracts/AlgebraPool.sol::55 => require(msg.sender == IAlgebraFactory(factory).owner());
src/core/contracts/AlgebraPool.sol::60 => require(topTick < TickMath.MAX_TICK + 1, 'TUM');
src/core/contracts/AlgebraPool.sol::61 => require(topTick > bottomTick, 'TLU');
src/core/contracts/AlgebraPool.sol::62 => require(bottomTick > TickMath.MIN_TICK - 1, 'TLM');
src/core/contracts/AlgebraPool.sol::122 => require(_lower.initialized);
src/core/contracts/AlgebraPool.sol::134 => require(_upper.initialized);
src/core/contracts/AlgebraPool.sol::194 => require(globalState.price == 0, 'AI');
src/core/contracts/AlgebraPool.sol::229 => require((_blockTimestamp() - lastLiquidityAddTimestamp) >= _liquidityCooldown);
src/core/contracts/AlgebraPool.sol::434 => require(liquidityDesired > 0, 'IL');
src/core/contracts/AlgebraPool.sol::454 => if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');
src/core/contracts/AlgebraPool.sol::455 => if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');
src/core/contracts/AlgebraPool.sol::469 => require(liquidityActual > 0, 'IIL2');
src/core/contracts/AlgebraPool.sol::474 => require((amount0 = uint256(amount0Int)) <= receivedAmount0, 'IIAM2');
src/core/contracts/AlgebraPool.sol::475 => require((amount1 = uint256(amount1Int)) <= receivedAmount1, 'IIAM2');
src/core/contracts/AlgebraPool.sol::608 => require(balance0Before.add(uint256(amount0)) <= balanceToken0(), 'IIA');
src/core/contracts/AlgebraPool.sol::614 => require(balance1Before.add(uint256(amount1)) <= balanceToken1(), 'IIA');
src/core/contracts/AlgebraPool.sol::636 => require(globalState.unlocked, 'LOK');
src/core/contracts/AlgebraPool.sol::641 => require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');
src/core/contracts/AlgebraPool.sol::645 => require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');
src/core/contracts/AlgebraPool.sol::731 => require(unlocked, 'LOK');
src/core/contracts/AlgebraPool.sol::733 => require(amountRequired != 0, 'AS');
src/core/contracts/AlgebraPool.sol::739 => require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');
src/core/contracts/AlgebraPool.sol::743 => require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');
src/core/contracts/AlgebraPool.sol::898 => require(_liquidity > 0, 'L');
src/core/contracts/AlgebraPool.sol::921 => require(balance0Before.add(fee0) <= paid0, 'F0');
src/core/contracts/AlgebraPool.sol::935 => require(balance1Before.add(fee1) <= paid1, 'F1');
src/core/contracts/AlgebraPool.sol::953 => require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));
src/core/contracts/AlgebraPool.sol::960 => require(msg.sender == IAlgebraFactory(factory).farmingAddress());
src/core/contracts/AlgebraPool.sol::968 => require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
src/core/contracts/AlgebraPoolDeployer.sol::22 => require(msg.sender == factory);
src/core/contracts/AlgebraPoolDeployer.sol::27 => require(msg.sender == owner);
src/core/contracts/AlgebraPoolDeployer.sol::37 => require(_factory != address(0));
src/core/contracts/AlgebraPoolDeployer.sol::38 => require(factory == address(0));
src/core/contracts/DataStorageOperator.sol::27 => require(msg.sender == pool, 'only pool can call this');
src/core/contracts/DataStorageOperator.sol::43 => require(msg.sender == factory || msg.sender == IAlgebraFactory(factory).owner());
src/core/contracts/DataStorageOperator.sol::45 => require(uint256(_feeConfig.alpha1) + uint256(_feeConfig.alpha2) + uint256(_feeConfig.baseFee) <= type(uint16).max, 'Max fee exceeded');
src/core/contracts/DataStorageOperator.sol::46 => require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
src/core/contracts/base/PoolState.sol::41 => require(globalState.unlocked, 'LOK');
src/core/contracts/libraries/DataStorage.sol::238 => require(lteConsideringOverflow(self[oldestIndex].blockTimestamp, target, time), 'OLD');
src/core/contracts/libraries/DataStorage.sol::369 => require(!self[0].initialized);
src/core/contracts/libraries/PriceMovementMath.sol::52 => require(price > 0);
src/core/contracts/libraries/PriceMovementMath.sol::53 => require(liquidity > 0);
src/core/contracts/libraries/PriceMovementMath.sol::87 => require(price > quotient);
src/core/contracts/libraries/TickManager.sol::96 => require(liquidityTotalAfter < Constants.MAX_LIQUIDITY_PER_TICK + 1, 'LO');
src/core/contracts/libraries/TokenDeltaMath.sol::51 => require(priceUpper >= priceLower);
```

