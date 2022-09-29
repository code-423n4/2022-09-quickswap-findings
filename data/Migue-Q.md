There are several require() sentences without message that explain revert's root cause. 

One I would like to point is at DataStorageOperator contract:
`` 

    function changeFeeConfiguration(AdaptiveFee.Configuration calldata _feeConfig) external override {
          require(msg.sender == factory || msg.sender == IAlgebraFactory(factory).owner());
          (....)
    }

``
I consider this relevant because it is an external call and I didn't get any explanation in the documentation that caller must be the factory. 

You can find others similar in AlgebraFactory contract. The previous suggestion applies to the following methods:
setOwner, setFarmingAddress, setVaultAddress.

Consider those validation will cause revert and it is not documented anywhere. So I should read the code to understand what is going on in case a revert.

I understand the responsibility could be split between the caller and the protocol but the protocol should manage it by itself.