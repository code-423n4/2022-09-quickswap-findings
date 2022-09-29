# Gas Report

- [G-01: `AlgebraPoolDeployer.owner` can be immutable](#g-01-algebrapooldeployerowner-can-be-immutable)
- [G-02: if clause in `AlgebraPool.collect()` can be split up to save gas](#g-02-if-clause-in-algebrapoolcollect-can-be-split-up-to-save-gas)

## G-01: `AlgebraPoolDeployer.owner` can be immutable

The value is only set once in the constructor

## G-02: if clause in `AlgebraPool.collect()` can be split up to save gas

```sol
if (amount0 | amount1 != 0) {
    position.fees0 = positionFees0 - amount0;
    position.fees1 = positionFees1 - amount1;

    if (amount0 > 0) TransferHelper.safeTransfer(token0, recipient, amount0);
    if (amount1 > 0) TransferHelper.safeTransfer(token1, recipient, amount1);
}
```

By splitting it up you save one if clause:

```sol
if (amount0 > 0) {
    position.fees0 = positionFees0 - amount0;
    TransferHelper.safeTransfer(token0, recipient, amount0);
}
if (amount1 > 0) {
    position.fees1 = positionFees1 - amount1;
    TransferHelper.safeTransfer(token1, recipient, amount1);
}
```

