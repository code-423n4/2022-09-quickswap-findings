1.

It is best practice and also unnecessary to initialize variables in for loops as they get set to 0 by default in:

Contract: DataStorage.sol

	line 307
	
Recommendation:

	for (uint256 i; i < secondsAgos.length; i++) {
	
2.

Grammer issues in comments in:

Contract: AlgebraPool.sol

	line 46 & 47

	"a" should be "an"

	line 255

	there should be an "a" before "fee"

	line 535

	there should be a "to" after "according"

	line 634

	"less" should  be "fewer"

	"then" should be "than"

	line 682

	"trough" should be "through"

	line 698

	"token" should be "tokens"

	line 872

	"then" should be "than"
	
Contract: DataStorageOperator.sol DataStorage.sol

	line 72

	"overflow" should be "overflowed"

Contract: AdaptiveFee.sol

	line 7

	"fee based" should be hyphened into "fee-based"

	there should be a "the" in front of "combination"

	line 22

	"fee based" should be hyphened into "fee-based"

	there should be a "the" in front of "formula"

Contract: DataStorage.sol

	line 7

	there should be an "and" before "volatility"

	line 24

	"1 sec" should be hyphened into "1-sec"

	line 58

	there should be a "the" in front of "creation"

	there should be an "a" in front of "new"

	line 89

	there should be a "," after "a"

	line 193, 199, 269 & 393

	"an" should be "a"
 
	line 216

	there should be a "the" before "target" and after "than"

	line 222

	there should be an "a" in front of "new"

	line 300

	"overflow" should be "overflowed"
	
Contract: PriceMovementMath.sol

	line 125

	the "," after "in" is unnecessary

	line 167

	there should be an "a" in front "fee"
	
Contract: TickManager.sol

	line 25

	there should be  a "the" in front of "current"

	line 70 & 128

	there should be "a" in front of "tick"
	
Contract: TickTable.sol

	line 99

	the "," after "next" is unnecessary
	
Contract: TokenDeltaMath.sol

	line 11

	there should be a "the" in front of "square"

Contract: PoolState.sol

	line 13

	the second "fee" should be "fees"
	
3.

SafeMath / overflow checks not needed in:

Contract: Technically speaking, all contracts for the sake of code correlation
	among other mentioned in Recommendation, but specifically in:

	Contract: AlgebraPool.sol , DataStorageOperator.sol & DataStorage.sol
	
	Since Solidity 0.8, the overflow/underflow check is implemented on the language level 
	Context: It adds the validation to the bytecode during compilation
	You don't need the SafeMath library for Solidity 0.8+

Recommendation:

	Consider upgrading contracts under scope to 
	pragma solidity ^0.8;
	to protect against any over/underflow
	
	This will also help with future updates to current unknown bugs (if any) to be automatically fixed