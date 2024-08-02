### **Smart Contract Address**
0x512215d00Ac09edD2483a5EBE798F0695cF8A215


### **Contract**
```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.24;

// Uncomment this line to use console.log
import "hardhat/console.sol";
struct LockArgs {
    uint unlockTime;
    uint256 initLockingAmount;
}
contract Lock {
    LockArgs public args;
    address payable public owner;

    uint  minBalance = 0.1 ether;

    event Withdrawal(uint amount, uint when);
    event Deposit(uint amount, uint when);

    error InvalidUnlocktime(uint unlockTime, uint currentTime);

    modifier onlyOwner() {
        require(msg.sender == owner, "You aren't the owner");
        _;
    }

    constructor(LockArgs memory _args) payable {
        require(
            block.timestamp < _args.unlockTime,
            "Unlock time should be in the future"
        );
        require(
            _args.initLockingAmount >= minBalance,
            string.concat(
                "Your ETH balance must be euqal or greater than ",
                uint2str(minBalance),
                " gwei\n"
            )
        );

        args = _args;

        owner = payable(msg.sender);
    }

    function withdraw() public payable onlyOwner {
        // Uncomment this line, and the import of "hardhat/console.sol", to print a log in your terminal
        // console.log("Unlock time is %o and block timestamp is %o", unlockTime, block.timestamp);

        if (block.timestamp < args.unlockTime)
            revert InvalidUnlocktime(args.unlockTime, block.timestamp);
        uint balance=getAddressBalance();

        require(balance>=minBalance);
        
        uint amount=msg.value;
        
        emit Withdrawal(amount, block.timestamp);

        owner.transfer(amount);
    }
    function uint2str(uint256 _i) internal  pure returns (string memory str) {
        if (_i == 0) {
            return "0";
        }
        uint256 j = _i;
        uint256 length;
        while (j != 0) {
            length++;
            j /= 10;
        }
        bytes memory bstr = new bytes(length);
        uint256 k = length;
        j = _i;
        while (j != 0) {
            bstr[--k] = bytes1(uint8(48 + (j % 10)));
            j /= 10;
        }
        str = string(bstr);
    }


    function setMinBalance(uint256 min)external{
        minBalance=min;
    }

     function getAddressBalance()public view returns (uint256 value){
            return  msg.sender.balance;
    }
}
```


### **Deploy.t**
```typescript
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
      const Lock = await ethers.getContractFactory("Lock");
      const lock = await Lock.deploy({
        unlockTime:JAN_1ST_2030,
        initLockingAmount:ownerBalance
      });
      
      console.table({
     lock:await lock.getAddress(),
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


