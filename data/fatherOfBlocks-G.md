**Algebra Factory**
- L43/60/62/63/78/85/92/109/110 - It is less expensive to use ifs and custom errors instead of requires.


**AlgebraPoolDeployer**
- L22/27/37/38 - It is less expensive to use ifs and custom errors instead of requires.


**DataStorageOperator**
- L27/43/45/46 - It is less expensive to use ifs and custom errors instead of requires.

- L138/139 - It is less expensive to do uint256() != 0, than uint256() > 0;


**libraries/DataStorage**
- L80 - It is less expensive to make uint() != 0, than uint256() > 0;

- L51/52/119/165/172/179/184/228/301/307/335/400/411 - Instead of doing "a = b + 1;" or "a = a - 1;" or "x -= 1;" it is less expensive to do --a or a = ++b;

- L307 - When we initialize a variable and we want to set its default value, it is not necessary to set it, since it has that value by default.

- L294/295/296/297/307 - When we are going through an array in a for loop, it is less expensive to create a uint variable and store the length there and not be in each iteration consulting the length.

- L238/369 - It is less expensive to use ifs and custom errors instead of requires.


**libraries/PriceMovementMath.sol**
- L52/53/70/71/87 - It is less expensive to use ifs and custom errors instead of requires.

- L52/53 - It is less expensive to make uint256() != 0, than uint256() > 0;


**libraries/TokenDeltaMath**
- L30/51 - It is less expensive to use ifs and custom errors instead of requires.


**base/PoolState**
- L41 - It is less expensive to use ifs and custom errors instead of requires.

- L15/42/44 - It is less expensive to use 0 and 1 of uint256 instead of bool with true and false.


**AlgebraPool**
- L55/60/61/62/122/134/224/229/434/454/455/469/474/475/608/614/636/641/645/731/733/739/743/898/921 /935/953/960/968 - It is less expensive to use ifs and custom errors instead of requires.

- L224/228/237/434/451/452/454/455/469/505/506/617/667/808/814/898/904/911/924/927/938/941 - Less expensive to do uint256 () != 0, which uint256() > 0;
