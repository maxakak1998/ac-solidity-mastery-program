# **Contractor**

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.24;

contract Calculator {
    event Substract(uint value);
    event Add(uint value);
    event Multiply(uint value);

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

    function add(uint value) public onlyPositiveNumber(value) {
        result += value;
    }
    function substract(
        uint value
    ) public onlyPositiveNumber(value) lessOrLessThanTheResult(value) {
        result -= value;
    }
    function multiply(uint value) public onlyPositiveNumber(value) {
        result *= value;
    }
}
``` 



# **Ignitigion lock module**
 
```solidity
import { buildModule } from "@nomicfoundation/hardhat-ignition/modules";

const LockModule = buildModule("LockModule", (m) => {


  const calculator = m.contract("Calculator", [0]);


  return { calculator };
});

export default LockModule;
```



# **Smart Contract Address**
 0x0de22BC31C51D9E4f0119Ae8637b99163244372d


 ## **Research**

 - Arithmetic Overflow and Underflow
 - Hacker can make the unlock_time zero using **Arithmetic Overflow or Underflow**
 - This issue occurs at Solidity version less than 0.8
 - Using SafeMath library for Solidity version less than 0.8




