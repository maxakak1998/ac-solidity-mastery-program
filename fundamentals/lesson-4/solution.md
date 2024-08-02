### **Contract**

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.24;
import "hardhat/console.sol";

contract Calculator {
    error UnSupportedOperator(uint8 operator );
    uint public result;
    constructor(uint _result) {
        result = _result;
    }

    modifier onlyPositiveNumber(uint value) {
        require(value > 0, "Amount must be positive");
        _;
    }

    modifier lessOrLessThanTheResult(uint value) {
        require(
            value <= result,
            "The value must equal or less than the result"
        );
        _;
    }

    function add(uint value) external onlyPositiveNumber(value) {
        result += value;
    }
    function substract(
        uint value
    ) external onlyPositiveNumber(value) lessOrLessThanTheResult(value) {
        result -= value;
    }
}

contract AdvancedCalculator is Calculator{
    constructor(uint _result)Calculator(_result){}

      function multiply(uint value) external onlyPositiveNumber(value) {
        result *= value;
    }


    function performOperation(uint256 a,uint256 b,uint8 operator)external onlyPositiveNumber(a)onlyPositiveNumber(b) {
        if(operator == 1){
            console.log("+");
            result= a+b;
        }else if(operator ==2){
            console.log("-");
            result=a-b;
        }else if(operator ==3){
            console.log("*");
            result=a*b;
        }else if(operator == 4){
            console.log("/");
            result=a/b;
        }else{
            console.log("null");
            revert UnSupportedOperator(operator);
        }

    }

}
```

### **Deploy**
```ts
import { ethers } from "hardhat";

const JAN_1ST_2030 = 1722410595;
const ONE_GWEI: bigint = 1_000_000_000n;
const ONE_HUNDRED_GWEI: bigint = 100_000_000_000n;
const WEI: bigint = 1_000_000_000_000_000_000n;


async function main() {
  const [owner] = await ethers.getSigners();
  console.log("Deploying contracts with the account: ", owner.address);
  const ownerBalance = await owner.provider.getBalance(owner);

  console.log("Account balance: ", (ownerBalance).toString());

  const deploy = async () => {

    const Calculator = await ethers.getContractFactory("Calculator");
    const calculator = await Calculator.deploy(0);
    const AdvancedCalculator = await ethers.getContractFactory("AdvancedCalculator");
    const avancedCaculator = await AdvancedCalculator.deploy(0);
    console.table({
      calculator: await calculator.getAddress(),
      avancedCaculator: await avancedCaculator.getAddress(),
    });

  };
  await deploy();
}
main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```
