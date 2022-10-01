## [NAZ-L1] Missing Equivalence Checks in Setters
**Severity**: Low
**Context**: [`AlgebraPool.sol#L952`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L952), [`AlgebraPool.sol#L959`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L959), [`AlgebraPool.sol#L967`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L967)

**Description**:
Setter functions are missing checks to validate if the new value being set is the same as the current value already set in the contract. Such checks will showcase mismatches between on-chain and off-chain states.

**Recommendation**:
This may hinder detecting discrepancies between on-chain and off-chain states leading to flawed assumptions of on-chain state and protocol behavior.


## [NAZ-L2] Missing Zero-address Validation
**Severity**: Low
**Context**: [`AlgebraPoolFactory.sol#L77`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L77), [`AlgebraPoolFactory.sol#L84`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L84), [`AlgebraPoolFactory.sol#L91`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L91), [`AlgebraPool.sol#L959`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L959), [`DataStorageOperator.sol#L31`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L31), [`PoolImmutables.sol#L29`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolImmutables.sol#L29)

**Description**:
Lack of zero-address validation on address parameters may lead to transaction reverts, waste gas, require resubmission of transactions and may even force contract redeployments in certain cases within the protocol.

**Recommendation**:
Consider adding explicit zero-address validation on input parameters of address type.


## [NAZ-L3] Missing Events In Initialize Functions
**Severity**: Low
**Context**: [`DataStorage.sol#L364`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L364)

**Description**:
None of the initialize functions emit emit init-specific events. They all however have the initializer modifier (from Initializable) so that they can be called only once. Off-chain monitoring of calls to these critical functions is not possible.

**Recommendation**:
It is recommended to emit events in your initialization functions.


## [NAZ-N1] Missing Visibility
**Severity**: Informational
**Context**: [`DataStorageOperator.sol#L15-L16`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L15-L16)

**Description**:
It's best practice to explicitly mark visibility of state variables.

**Recommendation**:
Consider adding the missing visibility to the state variables.


## [NAZ-N2] Line Length
**Severity**: Informational
**Context**: [`AlgebraPoolFactory.sol#L112`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L112), [`AlgebraPool.sol#L221`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L221), [`AlgebraPool.sol#L287`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L287), [`AlgebraPool.sol#L297`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L297), [`AlgebraPool.sol#L345`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L345), [`AlgebraPool.sol#L352`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L352), [`AlgebraPool.sol#L355`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L355), [`AlgebraPool.sol#L383`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L383), [`AlgebraPool.sol#L392`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L392), [`AlgebraPool.sol#L472`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L472), [`AlgebraPool.sol#L529`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L529), [`AlgebraPool.sol#L558`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L558), [`AlgebraPool.sol#L577`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L577), [`AlgebraPool.sol#L601`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L601), [`AlgebraPool.sol#L654`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L654), [`AlgebraPool.sol#L680`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L680), [`AlgebraPool.sol#L802`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L802), [`AlgebraPool.sol#L805`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L805), [`AlgebraPool.sol#L814`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L814), [`AlgebraPool.sol#L872-L873`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L872-L873), [`AlgebraPool.sol#L876`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L876), [`AlgebraPool.sol#L880`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L880), [`DataStorageOperator.sol#L45`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L45), [`DataStorageOperator.sol#L78`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L78), [`AdaptiveFee.sol#L38`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L38), [`AdaptiveFee.sol#L76`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol#L76), [`DataStorage.sol#L56-L57`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.solL56-L57), [`DataStorage.sol#L80-L81`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L80-L81), [`DataStorage.sol#L133`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L133), [`DataStorage.sol#L156`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L156), [`DataStorage.sol#L223`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L223), [`DataStorage.sol#L231`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L231), [`DataStorage.sol#L251`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L251), [`DataStorage.sol#L253`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L253), [`DataStorage.sol#L255`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L255), [`DataStorage.sol#L265`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L265), [`DataStorage.sol#L274-L276`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L274-L276), [`DataStorage.sol#L360`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L360), [`DataStorage.sol#L376`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L376), [`DataStorage.sol#L408`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L408), [`DataStorage.sol#L414`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L414), [`DataStorage.sol#L417`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L417), [`PriceMovementMath.sol#L64`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L64), [`PriceMovementMath.sol#L70`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L70), [`PriceMovementMath.sol#L80`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L80), [`PriceMovementMath.sol#L126`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L126), [`PriceMovementMath.sol#L153`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L153), [`PriceMovementMath.sol#L176`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol#L176), [`TickManager.sol#L25`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L25), [`TickManager.sol#L37-L38`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L37-L38), [`TickManager.sol#L70`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L70), [`TickManager.sol#L128`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L128), [`TickTable.sol#L33-L39`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L33-L39), [`TokenDeltaMath.sol#L53`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TokenDeltaMath.sol#L53)

**Description**:
Max line length must be no more than 120 but many lines are extended past this length.

**Recommendation**:
Consider cutting down the line length below 120.


## [NAZ-N3] Function && Variable Naming Convention
**Severity** Informational
**Context**: [`AlgebraPoolFactory.sol#L116`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L116), [`AlgebraPoolFactory.sol#L122`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol#L122), [`AlgebraPool.sol#L70`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L70), [`AlgebraPool.sol#L74`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L74), [`AlgebraPool.sol#L403`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L403), [`AlgebraPoolDeployer.sol#L18-L19`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol#L18-L19), [`DataStorageOperator.sol#L23-L24`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L23-L24), [`DataStorageOperator.sol#L115`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L115), [`Constants.sol#L5-L17`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/Constants.sol#L5-L17), [`DataStorage.sol#L13`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L13), [`DataStorage.sol#L32`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L32), [`DataStorage.sol#L49-L50`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L49-L50), [`DataStorage.sol#L66`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L66), [`DataStorage.sol#L94`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L94), [`DataStorage.sol#L105`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L105), [`DataStorage.sol#L148`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L148), [`TickTable.sol#L121`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L121), [`PoolState.sol#L27`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolState.sol#L27)

**Description**:
The linked variables do not conform to the standard naming convention of Solidity whereby functions and variable names(local and state) utilize the `mixedCase` format unless variables are declared as `constant` in which case they utilize the `UPPER_CASE_WITH_UNDERSCORES` format. Private variables and functions should lead with an `_underscore`.

**Recommendation**:
Consider naming conventions utilized by the linked statements are adjusted to reflect the correct type of declaration according to the [Solidity style guide](https://docs.soliditylang.org/en/latest/style-guide.html). 


## [NAZ-N4] Code Structure Deviates From Best-Practice
**Severity**: Informational
**Context**: [`AlgebraPool.sol#L79`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L79), [`AlgebraPool.sol#L96`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L96), [`AlgebraPool.sol#L262`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L262), [`AlgebraPool.sol#L675`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L657), [`AlgebraPool.sol#L692`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol#L692), [`DataStorageOperator.sol#L18`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L18), [`DataStorage.sol#L14`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L14), [`TickManager.sol#L78`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol#L78), [`TickTable.sol#L68`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol#L68), [`PoolImmutables.sol#L29`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolImmutables.sol#L29)

**Description**:
The best-practice layout for a contract should follow the following order: state variables, events, modifiers, constructor and functions. Function ordering helps readers identify which functions they can call and find constructor and fallback functions easier.  Functions should be grouped according to their visibility and ordered as: constructor, receive function (if exists), fallback function (if exists), external, public, internal, private. Functions should then further be ordered with view functions coming after the non-view labeled ones.

**Recommendation**:
Consider adopting recommended best-practice for code structure and layout.


## [NAZ-N5] Use Underscores for Number Literals
**Severity**: Informational
**Context**: [`DataStorageOperator.sol#L15-16`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol#L15-L16), [`Constants.sol#L17`](https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/Constants.sol#L17)

**Description**:
There are multiple occasions where certain numbers have been hardcoded, either in variables or in the code itself. Large numbers can become hard to read.

**Recommendation**:
Consider using underscores for number literals to improve its readability.


## [NAZ-N6] Unclear Revert Messages
**Severity** Informational
**Context**: [`All Contracts`](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts)

**Description**:
All revert messages across every contract are unclear which can lead to confusion. Unclear revert messages may cause misunderstandings on reverted transactions.

**Recommendation**: 
Consider making revert messages more clear.


## [NAZ-N7] Older Version Pragma
**Severity**: Informational
**Context**: [`All Contracts`](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts)

**Description**:
Using very old versions of Solidity prevents benefits of bug fixes and newer security checks. Using the latest versions might make contracts susceptible to undiscovered compiler bugs. 

**Recommendation**:
Consider using the most recent version.


## [NAZ-N8] Missing or Incomplete NatSpec
**Severity**: Informational
**Context**: [`All Contracts`](https://github.com/code-423n4/2022-09-quickswap/tree/main/src/core/contracts)

**Description**:
Some functions are missing @notice/@dev NatSpec comments for the function, @param for all/some of their parameters and @return for return values. Given that NatSpec is an important part of code documentation, this affects code comprehension, auditability and usability.

**Recommendation**:
Consider adding in full NatSpec comments for all functions to have complete code documentation for future use.