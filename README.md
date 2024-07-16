# Mio-Dio-Token
 Criando a primeira criptomoeda da rede Ethereum

 ## Objetivo
 Estudar o método de criação e principapis funções de um smart contract da rede Ethereum.

 ## Tecnologias utilizadas
 Ide Remix https://remix.ethereum.org/
 Truffle https://archive.trufflesuite.com/
 Ganache https://archive.trufflesuite.com/ganache/
 Metamask https://metamask.io/

## Smart contract
```solidity
 // SPDX-License-Identifier: GPL-3.0

pragma solidity ^0.8.0;

/* 
 * @title MioDioToken
 * Estudo de criação e implementação de um token na rede Ethereum
 * Implementado todas as funções de operação com o token com base no repo https://github.com/digitalinnovationone
 * dev https://www.linkedin.com/in/jcbdeoliveira/
 * 16/07/2024
 */

interface IERC20{

    //getters
    function totalSupply() external view returns(uint256);
    function balanceOf(address account) external view returns (uint256);
    function allowance(address owner, address spender) external view returns (uint256);

    //functions
    function transfer(address recipient, uint256 amount) external returns (bool);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256);

}

contract DIOToken is IERC20{

    string public constant name = "Mio DIO Token";
    string public constant symbol = "DigIO";
    uint8 public constant decimals = 18;

    mapping (address => uint256) balances;

    mapping(address => mapping(address=>uint256)) allowed;

    uint256 totalSupply_ = 10 ether;

    constructor(){
        balances[msg.sender] = totalSupply_;
    }

    function totalSupply() public override view returns (uint256) {
        return totalSupply_;
    }

    function balanceOf(address tokenOwner) public override view returns (uint256){
        return balances[tokenOwner];
    }

    function transfer(address receiver, uint256 numTokens) public override returns (bool) {
        require(numTokens <= balances[msg.sender]);
        balances[msg.sender] = balances[msg.sender]-numTokens;
        balances[receiver] = balances[receiver]+numTokens;
        emit Transfer(msg.sender, receiver, numTokens);
        return true;
    }

    function approve(address delegate, uint256 numTokens) public override returns (bool) {
        allowed[msg.sender][delegate] = numTokens;
        emit Approval(msg.sender, delegate, numTokens);
        return true;
    }

    function allowance(address owner, address delegate) public override view returns (uint) {
        return allowed[owner][delegate];
    }

    function transferFrom(address owner, address buyer, uint256 numTokens) public override returns (bool) {
        require(numTokens <= balances[owner]);
        require(numTokens <= allowed[owner][msg.sender]);

        balances[owner] = balances[owner]-numTokens;
        allowed[owner][msg.sender] = allowed[owner][msg.sender]-numTokens;
        balances[buyer] = balances[buyer]+numTokens;
        emit Transfer(owner, buyer, numTokens);
        return true;
    }

}
```
## Resultado do deploy

	[]


