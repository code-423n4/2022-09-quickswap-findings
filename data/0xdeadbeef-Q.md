## In the file AlgebraFactory.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 50: _poolDeployer
	line 59: tokenA
	line 77: _owner
	line 84: _farmingAddress
	line 91: _vaultAddress
```
## In the file AlgebraPool.sol: 
Line 416	  function mint: 
: _safemint() should be used instead of _mint() function whereever possible

these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 275: owner
	line 275: owner
	line 417: sender
	line 418: recipient
	line 418: recipient
	line 546: token
	line 418: recipient
	line 417: sender
	line 418: recipient
	line 418: recipient
	line 959: virtualPoolAddress
```
## In the file AlgebraPoolDeployer.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 45: dataStorage
	line 47: token0
	line 48: token1
	line 49: pool
```
## In the file DataStorageOperator.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 31: _pool
```
## In the file PoolState.sol: 
## In the file PoolImmutables.sol: 
these address variables are not checked whether they are address(0), address variable should check if it is zero
```
	line 29: deployer
```




