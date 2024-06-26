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

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

import "@openzeppelin/contracts/access/Ownable.sol";

contract DegenTokenArlos is ERC20, Ownable {

    mapping(uint256 => uint256) public GameValue;
    
    mapping(uint256 => address) public RedeemedItems;
    
    mapping(uint256 => address) public GameItemOwners;

    event GameExclusiveRedeemed(address indexed gamer, address indexed recipient, uint256 indexed gameId, uint256 value);
    event GameItemTransferred(uint256 indexed gameId, address indexed recipient);

    constructor(address initialOwner) Ownable(initialOwner) ERC20("DEGEN", "DGN") {
        _mint(initialOwner, 30000);

        GameValue[1] = 1000;  // Game Item 1: STARDEW VALLEY - Value: 1000 tokens
        GameValue[2] = 2000;  // Game Item 2: Surviving Mars - Value: 2000 tokens
        GameValue[3] = 3000;  // Game Item 3: Total War SHOGUN 2 - Value: 3000 tokens
        GameValue[4] = 4000;  // Game Item 4: Total War Warhammer 2 - Value: 4000 tokens
        GameValue[5] = 5000;  // Game Item 5: No Man Sky - Value: 5000 tokens
        GameValue[6] = 6000;  // Game Item 6: Valorant - Value: 6000 tokens
        GameValue[7] = 7000;  // Game Item 7: Assassin's Creed Origins - Value: 7000 tokens
        GameValue[8] = 8000;  // Game Item 8: Assassin's Creed Odyssey - Value: 8000 tokens
        GameValue[9] = 9000;  // Game Item 9: Star Wars Empire at war - Value: 9000 tokens
        GameValue[10] = 10000; // Game Item 10: GTA VI - Value: 10000 tokens GTA VI
    }

    function mint(address to, uint256 amount) public onlyOwner {
        _mint(to, amount);
    }

    function burn(uint256 amount) public {
        _burn(msg.sender, amount);
    }

    function setItemValue(uint256 gameId, uint256 value) external onlyOwner {
        GameValue[gameId] = value;
    }

    function RedeemExclusiveGame(uint256 gameId, address recipient) external {
        require(GameValue[gameId] > 0, "Game Value is not set");
        require(balanceOf(msg.sender) >= GameValue[gameId], "Insufficient balance");

        _transfer(msg.sender, recipient, GameValue[gameId]);
        RedeemedItems[gameId] = recipient;
        emit GameExclusiveRedeemed(msg.sender, recipient, gameId, GameValue[gameId]);

        GameItemOwners[gameId] = recipient;
        emit GameItemTransferred(gameId, recipient);
    }
    function transfer(address recipient, uint256 amount) public virtual override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }
}

## Authors
National Teachers College

Razelle Brian L. Arlos

8214515@ntc.edu.ph
