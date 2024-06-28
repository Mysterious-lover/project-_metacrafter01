// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SimpleToken {
    address public owner;
    mapping(address => uint256) public balances;
    uint256 public totalSupply;

    event Transfer(address indexed from, address indexed to, uint256 value);

    modifier onlyOwner() {
        require(msg.sender == owner, "Caller is not the owner");
        _;
    }

    constructor() {
        owner = msg.sender;
    }

    function mint(address to, uint256 amount) public onlyOwner {
        require(to != address(0), "Invalid address");
        totalSupply += amount;
        balances[to] += amount;
        assert(totalSupply >= amount); // Internal consistency check
    }

    function transfer(address to, uint256 amount) public {
        require(to != address(0), "Invalid address");
        require(balances[msg.sender] >= amount, "Insufficient balance");
        
        balances[msg.sender] -= amount;
        balances[to] += amount;

        emit Transfer(msg.sender, to, amount);
        
        if (balances[msg.sender] < amount) {
            revert("Transfer failed");
        }
    }

    function balanceOf(address account) public view returns (uint256) {
        return balances[account];
    }
}
