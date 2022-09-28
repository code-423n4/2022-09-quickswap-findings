**AlgebraFactory**
- L2 - All the audited files use the pragma solidity =0.7.6; statement. This implies that an old solidity version is being used which may lead into hitting already fixed bugs.

- L60/62/63/78/85/92 - When we use a require and throw an exception it is important to show a message, this is important because it makes the user better understand the reason why it is reverted.

- L23/84 - The variable farmingAddress is created and a setter function is created, but it is not used at all. In the event that a contract inherits this contract and needs that variable, it should be created in that contract.


**AlgebraPoolDeployer**
- L2 - All the audited files use the pragma solidity =0.7.6; statement. This implies that an old solidity version is being used which may lead into hitting already fixed bugs.

- L22/27/37/38 - When we use a require and throw an exception it is important to show a message, this is important because it makes the user better understand the reason why it is reverted.

- L36/38 - If the factory can only be set once, it is less expensive to set it directly in the constructor and not create a function to be used only once.

- L21/26 - It is not necessary to create a modifier if it is only going to be used once.


**DataStorageOperator**
- L2 - All the audited files use the pragma solidity =0.7.6; statement. This implies that an old solidity version is being used which may lead into hitting already fixed bugs.

- L12 - The constants file is imported, but it is not used throughout the contract.

- L43 - When we use a require and throw an exception it is important to show a message, this is important because it makes the user better understand the reason why it is reverted.


**libraries/AdaptiveFee**
- L2 - All the audited files use the pragma solidity =0.7.6; statement. This implies that an old solidity version is being used which may lead into hitting already fixed bugs.

- L4 - The constants file is imported, but it is not used throughout the contract.


**libraries/Constant**
- L2 - All the audited files use the pragma solidity =0.7.6; statement. This implies that an old solidity version is being used which may lead into hitting already fixed bugs.


**libraries/DataStorage**
- L2 - All the audited files use the pragma solidity =0.7.6; statement. This implies that an old solidity version is being used which may lead into hitting already fixed bugs.

- L4 - The FullMath file is imported, but it is not used in the entire contract.

- L369 - When we use a require and throw an exception it is important to show a message, this is important because it makes the user better understand the reason why it is reverted.


**libraries/PriceMovementMath.sol**
- L2 - All the audited files use the pragma solidity =0.7.6; statement. This implies that an old solidity version is being used which may lead into hitting already fixed bugs.

- L52/53/70/71/87 - When we use a require and throw an exception it is important to show a message, this is important because it makes the user understand better the reason why it is reverted.

- L10/11 - The LowGasSafeMath and SafeCast libraries are used but their libraries are not imported, this would not compile and would generate problems in the deploy.


**libraries/TickManager**
- L2 - All the audited files use the pragma solidity =0.7.6; statement. This implies that an old solidity version is being used which may lead into hitting already fixed bugs.


**libraries/TickTable**
- L2 - All the audited files use the pragma solidity =0.7.6; statement. This implies that an old solidity version is being used which may lead into hitting already fixed bugs.


**libraries/TokenDeltaMath**
- L2 - All the audited files use the pragma solidity =0.7.6; statement. This implies that an old solidity version is being used which may lead into hitting already fixed bugs.

- L30/51 - When we use a require and throw an exception it is important to show a message, this is important because it makes the user better understand the reason why it is reverted.


**base/PoolImmutables**
- L2 - All the audited files use the pragma solidity =0.7.6; statement. This implies that an old solidity version is being used which may lead into hitting already fixed bugs.


**base/PoolState**
- L2 - All the audited files use the pragma solidity =0.7.6; statement. This implies that an old solidity version is being used which may lead into hitting already fixed bugs.


**AlgebraPool**
- L2 - All the audited files use the pragma solidity =0.7.6; statement. This implies that an old solidity version is being used which may lead into hitting already fixed bugs.

- L25 - The IAlgebraPoolDeployer file is imported, but it is not used in the entire contract.

- L55/122/134/229/953/960/968 - When we use a require and throw an exception it is important to show a message, this is important because it makes the user understand better the reason why it is reverted.
