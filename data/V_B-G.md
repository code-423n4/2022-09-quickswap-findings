### 1. access checks in view functions

Functions `getSingleTimepoint`, `getTimepoints`, `getAverages` and `getFee` from `DataStorageOperator` contract have an access check in `onlyPool` modifier. However, all of them are `view` functions so it is reasonable to remove such access checks as they do not protect any "secret" information from other contracts. This will reduce gas consumption.