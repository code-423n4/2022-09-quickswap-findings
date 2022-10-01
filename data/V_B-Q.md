### 1. changeFeeConfiguration summation overflow

There is a function `changeFeeConfiguration` in `DataStorageOperator` contract. It contains the following logic:

```solidity=
/// @inheritdoc IDataStorageOperator
function changeFeeConfiguration(AdaptiveFee.Configuration calldata _feeConfig) external override {
  require(msg.sender == factory || msg.sender == IAlgebraFactory(factory).owner());

  require(uint256(_feeConfig.alpha1) + uint256(_feeConfig.alpha2) + uint256(_feeConfig.baseFee) <= type(uint16).max, 'Max fee exceeded');
  require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');

  feeConfig = _feeConfig;
  emit FeeConfiguration(_feeConfig);
}
```

Because in solidity compiler version `0.7.6` there is no arithmetic operations checks there is a problem of overflow when counting `uint256(_feeConfig.alpha1) + uint256(_feeConfig.alpha2) + uint256(_feeConfig.baseFee)`. Because of this, there is no strict validation of the `_feeConfig` parameters.

### 2. algebraMintCallback return value

There is a function `algebraMintCallback` in `IAlgebraMintCallback` interface. It is reasonable to add some special return value as an expected output of this function. This will protect the contract from calling fallback function which is not supposed to be used in such manner. As an example, such logic is implemented in `ERC721TokenReceiver` interface in [EIP-721 Non-Fungible Token Standard](https://eips.ethereum.org/EIPS/eip-721).

### 3. reinitialization or initialization with zero initialPrice

There is a function `initialize` in `AlgebraPool` contract. It contains the following logic:

```solidity=
/// @inheritdoc IAlgebraPoolActions
function initialize(uint160 initialPrice) external override {
  require(globalState.price == 0, 'AI');
  // getTickAtSqrtRatio checks validity of initialPrice inside
  int24 tick = TickMath.getTickAtSqrtRatio(initialPrice);

  uint32 timestamp = _blockTimestamp();
  IDataStorageOperator(dataStorageOperator).initialize(timestamp, tick);

  globalState.price = initialPrice;
  globalState.unlocked = true;
  globalState.tick = tick;

  emit Initialize(initialPrice, tick);
}
```

It is better to have a separate variable that indicates was the contract initialized or not, and a special check on a such variable inside of this function. This is so because of the possibility of incorrect initialization with zero `initialPrice` and the possibility of changing the `globalState.price` to zero value (with reinitialization after such state).

### 4. access checks in view functions

Functions `getSingleTimepoint`, `getTimepoints`, `getAverages` and `getFee` from `DataStorageOperator` contract have an access check in `onlyPool` modifier. However, all of them are `view` functions so it is reasonable to remove such access checks as they do not protect any "secret" information from other contracts.