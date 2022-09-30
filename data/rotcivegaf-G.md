# Gas report

| G-N    |Issue|Instances|
|:------:|:----|:-------:|
| [G&#x2011;01] | `++i` costs less gas than `i++`, especially when it's used in `for`-loops (`--i`/`i--` too) | 1 |
| [G&#x2011;04] | `<array>.length` should not be looked up in every loop of a `for`-loop | 1 |

Total:  instances over  issues

## [G-01] `++i` costs less gas than `i++`, especially when it's used in `for`-loops (`--i`/`i--` too)

Saves 5 gas per loop

```solidity
File: /src/core/contracts/libraries/DataStorage.sol

307    for (uint256 i = 0; i < secondsAgos.length; i++) {
```

## [G‑02] `<array>.length` should not be looked up in every loop of a `for`-loop

The overheads outlined below are PER LOOP, excluding the first loop

 * storage arrays incur a Gwarmaccess (100 gas)
 * memory arrays use `MLOAD` (3 gas)
 * calldata arrays use `CALLDATALOAD` (3 gas)

Caching the length changes each of these to a `DUP<N>` (3 gas), and gets rid of the extra `DUP<N>` needed to store the stack offset

```solidity
File: /src/core/contracts/libraries/DataStorage.sol

307    for (uint256 i = 0; i < secondsAgos.length; i++) {
```

Also can used in

```solidity
File: /src/core/contracts/libraries/DataStorage.sol

294    tickCumulatives = new int56[](secondsAgos.length);
295    secondsPerLiquidityCumulatives = new uint160[](secondsAgos.length);
296    volatilityCumulatives = new uint112[](secondsAgos.length);
297    volumePerAvgLiquiditys = new uint256[](secondsAgos.length);
```