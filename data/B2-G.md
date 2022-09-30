## AN ARRAY’S LENGTH SHOULD BE CACHED TO SAVE GAS IN FOR-LOOPS
Even memory arrays incur the overhead of bit tests and bit shifts to calculate the array length. Storage array length checks incur an extra Gwarmaccess (100 gas) PER-LOOP.

    File : contracts/libraries/DataStorage.sol    #1
    307     for (uint256 i = 0; i < secondsAgos.length; i++) {

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307

## ++i costs less gas than i++, especially when it’s used in for-loops (--i/i-- too)
Saves 6 gas PER LOOP

    File : contracts/libraries/DataStorage.sol   #1
    307     for (uint256 i = 0; i < secondsAgos.length; i++) {

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307

## It costs more gas to initialize variables to zero than to let the default of zero be applied

    File : contracts/libraries/DataStorage.sol   #1
    307     for (uint256 i = 0; i < secondsAgos.length; i++) {

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L307

## Using > 0 costs more gas than != 0 when used on a uint in a require() statement
This change saves 6 gas per instance

    File: contracts/AlgebraPool.sol   #1
    224      require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L224

      File: contracts/AlgebraPool.sol   #2
      434     require(liquidityDesired > 0, 'IL');

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L434

      File: contracts/AlgebraPool.sol   #3
      469    require(liquidityActual > 0, 'IIL2');

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L469

      File: contracts/libraries/PriceMovementMath.sol   #4
      52    require(price > 0);

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L52

      File: contracts/libraries/PriceMovementMath.sol   #5
      53    require(liquidity > 0);

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L53

## Splitting require() statements that use && saves gas
Instead of using the && operator in a single require statement to check multiple conditions, I suggest using multiple require statements with 1 condition per require statement (saving 3 gas per &):

    File: contracts/AlgebraFactory.sol   #1
    110    require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L110

      File: contracts/DataStorageOperator.sol   #2
      46    require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L46

## Inline a modifier that's only used once
As `onlyFactory()`is only used once in this contract (in `function deploy()`), it should get inlined to save gas without losing readability:

    File: contracts/AlgebraPoolDeployer.sol   #1
    49   ) external override onlyFactory returns (address pool) {//@audit onlyFactory modifier only used only here: inline it}

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L49

## Internal functions only called once can be inlined to save gas
Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

    File: contracts/AlgebraFactory.sol   #1
    122  function computeAddress(address token0, address token1) internal view returns (address pool) {

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L122

      File: contracts/AlgebraPool.sol   #2
      215  function _recalculatePosition(
      216          Position storage _position,
      217          int128 liquidityDelta,
      218          uint256 innerFeeGrowth0Token,
      219         uint256 innerFeeGrowth1Token
      220      ) internal {
      

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L215-L220

      File: contracts/libraries/DataStorage.sol   #3
      32  function _volatilityOnRange(
      33      int256 dt,
      34      int256 tick0,
      35      int256 tick1,
      36      int256 avgTick0,
      37      int256 avgTick1
      38    ) internal pure returns (uint256 volatility) {

*https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L32-L38

## Use custom errors rather than revert()/require() strings to save deployment gas

    File: contracts/AlgebraFactory.sol   #1
    109    require(uint256(alpha1) + uint256(alpha2) + uint256(baseFee) <= type(uint16).max, 'Max fee exceeded');

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L109

      File: contracts/AlgebraFactory.sol   #2
      110    require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L110

      File: contracts/DataStorageOperator.sol   #3
      45    require(uint256(_feeConfig.alpha1) + uint256(_feeConfig.alpha2) + uint256(_feeConfig.baseFee) <= type(uint16).max, 'Max fee exceeded');

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L45

      File: contracts/DataStorageOperator.sol   #4
      46    require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L46

## Internal functions not called by the contract should be removed to save deployment gas
If the functions are required by an interface, the contract should inherit from that interface and use the override keyword

    File: contracts/libraries/AdaptiveFee.sol   #1
    25   function getFee(
    26      uint88 volatility,
    27      uint256 volumePerLiquidity,
    28      Configuration memory config
    29    ) internal pure returns (uint16 fee) {

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L25-L29

      File: contracts/libraries/DataStorage.sol   #2
      277  function getTimepoints(
      278      Timepoint[UINT16_MODULO] storage self,
      279       uint32 time,
      280       uint32[] memory secondsAgos,
      281       int24 tick,
      282       uint16 index,
      283       uint128 liquidity
      284        )
      285           internal
      286           view
      287            returns (

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L277-287

##  Using private rather than public for constants, saves gas
If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table

    File: contracts/DataStorageOperator.sol   #1
    15  uint256 constant UINT16_MODULO = 65536;

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L15

      File: contracts/libraries/DataStorage.sol   #2
      12  uint32 public constant WINDOW = 1 days;

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L12

## Use a more recent version of solidity
Use a solidity version of at least 0.8.0 to get overflow protection without SafeMath
Use a solidity version of at least 0.8.2 to get compiler automatic inlining
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

    File: contracts/AlgebraFactory.sol   #1
    2      pragma solidity =0.7.6;

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L2

      File: contracts/AlgebraPool.sol   #2
      2      pragma solidity =0.7.6;

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L2

      File: contracts/AlgebraPoolDeployer.sol   #3
      2      pragma solidity =0.7.6;

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L2

      File: contracts/DataStorageOperator.sol   #4
      2      pragma solidity =0.7.6;

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L2

      File: contracts/libraries/AdaptiveFee.sol  #5
      2      pragma solidity =0.7.6;

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L2

      File: contracts/libraries/Constants.sol #6
      2      pragma solidity =0.7.6;

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/Constants.sol#L2

      File: contracts/libraries/DataStorage.sol #7
      2      pragma solidity =0.7.6

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L2

      File: contracts/libraries/PriceMovementMath.sol   #8
      2      pragma solidity =0.7.6

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L2

      File: contracts/libraries/TickManager.sol   #9
      2      pragma solidity =0.7.6

* https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L2
