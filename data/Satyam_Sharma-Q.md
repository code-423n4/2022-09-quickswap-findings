Issue 1.
check require statement required to chek if provided variable not equal to zero...
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L367-L371
    int24 bottomTick,
    int24 topTick,
    int128 liquidityDelta,
    int24 currentTick,
    uint160 currentPrice
In https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L382-L388
if (currentTick < bottomTick) {
      amount0 = TokenDeltaMath.getToken0Delta(TickMath.getSqrtRatioAtTick(bottomTick), TickMath.getSqrtRatioAtTick(topTick), liquidityDelta);
    } else if (currentTick < topTick) {
      amount0 = TokenDeltaMath.getToken0Delta(currentPrice, TickMath.getSqrtRatioAtTick(topTick), liquidityDelta);
      amount1 = TokenDeltaMath.getToken1Delta(TickMath.getSqrtRatioAtTick(bottomTick), currentPrice, liquidityDelta);

      globalLiquidityDelta = liquidityDelta;

condition will not proceed will all the parameter are set equal all if all the parameter are set  equal 0..

Issue 2.
Zero-address checks are a best-practise for input validation of critical address parameters. 
While the codebase applies this to most addresses in setters...
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L546
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L275
In address parameter of function _payCommunityFee make a zero address check!!!

Issue 3.
You Should declare bool exactInput on L686 outside of the struct inside a function local variable to strictly true or false otherwise it will set its default value to false..
through which other condition/statement using this parameter can get harmful effect of using this bool parameter with its default value false!!
https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L686