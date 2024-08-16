```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.24;


abstract contract AbstractStorage{

    uint256 internal score;

    function store(uint256 value) public{
            score=value;
    }

    function retrieve() public view virtual returns (uint256);
}

contract SimpleStorage is AbstractStorage{
    function retrieve() public view override returns (uint256){
        return score;
    }
}
```