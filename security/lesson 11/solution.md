```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.24;

interface IStorage {
    function store(uint256 value) external;

    function retrieve() external view returns (uint256);
}

contract Uint256Storage {
    uint256 data;

    function store(uint256 value) public {
        data = value;
    }

    function retrieve() public view returns (uint256) {
        return data;
    }
}

contract SimpleStorage {
    function store(address storageContract, uint256 data) public {
        IStorage(storageContract).store(data);
    }
    function retrieve(address storageContract) public view returns (uint256) {
        return IStorage(storageContract).retrieve();
    }
}
```