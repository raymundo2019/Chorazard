# Chorazard
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.0;


contract Token {
    mapping(address => uint) public balances;
    mapping(address => mapping(address => uint)) public allowance;
    uint public totalSupply = 200000000000000 * 10 ** 18;
    string public name = "Chorazard";
    string public symbol = "CHORD";
    uint public decimals = 18;
   
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
   
    constructor() {
        balances[msg.sender] = totalSupply;
    }
   
    function balanceOf(address owner) public view returns(uint) {
        return balances[owner];
    }
   
    function transfer(address to, uint value) public returns(bool) {
        require(to != address(0), 'Invalid address');
        require(balanceOf(msg.sender) >= value, 'Insufficient balance');
       
        balances[to] += value;
        balances[msg.sender] -= value;
        emit Transfer(msg.sender, to, value);
        return true;
    }
   
    function transferFrom(address from, address to, uint value) public returns(bool) {
        require(from != address(0), 'Invalid address');
        require(to != address(0), 'Invalid address');
        require(balanceOf(from) >= value, 'Insufficient balance');
        require(allowance[from][msg.sender] >= value, 'Insufficient allowance');
       
        balances[to] += value;
        balances[from] -= value;
        allowance[from][msg.sender] -= value;
        emit Transfer(from, to, value);
        return true;  
    }
   
    function approve(address spender, uint value) public returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;  
    }
}
