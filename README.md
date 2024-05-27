# Project Title

Project: Degen Token (ERC-20)

## Description
Create the ERC-20 token

Set the name to “Degen”

Set the symbol to “DGN”

1. Minting new tokens: The smart contract should be able to create new tokens and distribute them to gamers as rewards. Only the owner can mint tokens.
2. Transferring tokens: Gamers should be able to transfer their tokens to others.
3. Redeeming tokens: Gamers should be able to redeem their tokens for items in the in-game store.
4. Checking token balance: Gamers should be able to check their token balance at any time.
5. Burning tokens: Anyone should be able to burn tokens, that they own, that are no longer needed.

### Executing program

// SPDX-License-Identifier: MIT

pragma solidity >=0.6.12 <0.9.0;

// Project: Degen Token (ERC-20)

// Challenge:

// 1. Minting new tokens: The smart contract should be able to create new tokens and distribute them to gamers as rewards. Only the owner can mint tokens.

// 2. Transferring tokens: Gamers should be able to transfer their tokens to others.

// 3. Redeeming tokens: Gamers should be able to redeem their tokens for items in the in-game store.

// 4. Checking token balance: Gamers should be able to check their token balance at any time.

// 5. Burning tokens: Anyone should be able to burn tokens, that they own, that are no longer needed.

// By importing ERC20.sol, your contract inherits all the functionality and features defined in the ERC20 standard.

// This includes functions like transfer, transferFrom, approve, balanceOf, totalSupply, and more, which are essential for interacting with ERC20 tokens.

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

// By importing Ownable.sol into your contract, you gain access to this functionality,

// allowing you to easily implement ownership-based access control in your smart contracts.

// This is particularly useful for scenarios where certain operations, such as administrative tasks or sensitive operations, should only be performed by the contract owner.

import "@openzeppelin/contracts/access/Ownable.sol";

// The contract DegenTokenArlos inherits from both ERC20 and Ownable, which means it will have access to the functionalities of both contracts.

contract DegenTokenArlos is ERC20, Ownable {

    // In your contract, GameValue is used to store the values corresponding to different game IDs. Each game ID is associated with a certain value, 
    // Representing the cost or value of the game in terms of your token (DegenTokenArlos).
    mapping(uint256 => uint256) public GameValue;

    // With this event declaration, you can emit GameExclusiveRedeemed events in your contract whenever a gamer redeems an exclusive game item, 
    // providing transparency and allowing external parties to track these events on the Ethereum blockchain.
    event GameExclusiveRedeemed(address indexed gamer, uint256 indexed gameId, uint256 value);

    // Constructor: The constructor function initializes the contract. It takes an initialOwner address as an argument, 
    // which will be passed to the constructor of the Ownable contract. The Ownable constructor sets the initial owner of the contract. 
    // Additionally, the constructor of the ERC20 contract is called with the token name "DEGEN" and symbol "DGN".
    constructor(address initialOwner) Ownable(initialOwner) ERC20("DEGEN", "DGN") {

        // Token Minting: Inside the constructor, _mint function from the ERC20 contract is called to mint an initial supply of 30000 tokens to the initialOwner.
        _mint(initialOwner, 30000);

        GameValue[1] = 1000; // Item 1: STARDEW VALLEY - Value: 1000 tokens
        GameValue[2] = 2000; // Item 2: Surviving Mars - Value: 2000 tokens
        GameValue[3] = 3000; // Item 3: Total War SHOGUN 2 - Value: 3000 tokens
        GameValue[4] = 4000; // Item 4: Total War Warhammer 2 - Value: 4000 tokens
        GameValue[5] = 5000; // Item 5: No Man Sky - Value: 5000 tokens
        GameValue[6] = 6000; // Item 6: Valorant - Value: 6000 tokens
        GameValue[7] = 7000; // Item 7: Assassin's Creed Origins - Value: 7000 tokens
        GameValue[8] = 8000; // Item 8: Assassin's Creed Odyssey - Value: 8000 tokens
        GameValue[9] = 9000; // Item 9: Star Wars Empire at war - Value: 9000 tokens
        GameValue[10] = 10000; // Item 10: GTA VI - Value: 10000 tokens GTA VI
    }

    // This function allows the contract owner to mint new tokens and assign them to any address. 
    // This is a common feature in ERC20 tokens, as it allows for the controlled expansion of the token supply when needed.
    function mint(address to, uint256 amount) public onlyOwner {
        _mint(to, amount);
    }

    // This function allows any user to burn their own tokens, reducing the total supply of the token. 
    // Burning tokens can be useful for various purposes, such as reducing supply to increase token value or removing tokens from circulation.
    function burn(uint256 amount) public {
        _burn(msg.sender, amount);
    }

    // This function provides flexibility for the contract owner to adjust the value of game items without the need for redeploying the contract, 
    // enhancing the contract's adaptability to changing circumstances or market conditions.
    function setItemValue(uint256 gameId, uint256 value) external onlyOwner {
        GameValue[gameId] = value;
    }

    // This function provides a straightforward way for gamers to redeem exclusive game items using the token (DegenTokenArlos) and 
    // ensures that the necessary conditions are met before processing the redemption.
    function RedeemExclusiveGame(uint256 gameId) external {

        // These require statements ensure that the redemption of the game item is valid and that the caller meets 
        // the necessary conditions before proceeding with the redemption process.
        // Ensure Game Value is set and gamers has enough balance
        require(GameValue[gameId] > 0, "Game Value is not set");
        require(balanceOf(msg.sender) >= GameValue[gameId], "Insufficient balance");

        // These actions ensure that the redemption process is properly logged on the blockchain and that the ownership of the tokens is transferred accordingly.
        // Transfer tokens from gamers to owner (in-game store)
        _transfer(msg.sender, owner(), GameValue[gameId]);
        emit GameExclusiveRedeemed(msg.sender, gameId, GameValue[gameId]);
    }
} 

## Authors
National Teachers College

Razelle Brian L. Arlos

8214515@ntc.edu.ph
