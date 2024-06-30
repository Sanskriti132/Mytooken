# DENGEN TOOKEN

A Simple Token Agreement for Evaporating, Burning, and Purchasing

## DESCRIPTION

DengenToken is an Ethereum-based smart contract written in Solidity. By providing features for minting, burning, transferring, and redeeming tokens, it enables users to get an understanding of token management principles within a decentralized setting. 
This contract is perfect for teaching purposes and acts as a basic model for more intricate token systems.

## GETTING STARTED

Important features including minting, burning, transferring, and redeeming tokens are demonstrated via DengenToken. It emits events to record token redemptions and has structures to track redeemed things.

### INSTALLING

In this, I have created a token Degen with symbol DGN with the help of Avalanche Fuji Testnet.

### EXECUTING PROGRAM


    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;

    import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
    import "@openzeppelin/contracts/access/Ownable.sol";

    contract DengenToken is ERC20, Ownable {
        constructor() ERC20("Degen", "DGN") Ownable(msg.sender) {}

    struct RedeemedItem {
        uint256 amount;          
        uint256 tokensRedeemed;  
    }

    event TokensRedeemed(address indexed redeemer, uint256 amount, uint256 tokensRedeemed);

    uint256 public tokenCost = 1; 

    mapping(address => RedeemedItem[]) private redeemedItems;

    
    function mint(address to, uint256 amount) public onlyOwner {
        require(to != address(0), "Cannot mint to zero address");
        require(amount > 0, "Mint amount must be greater than zero");
        _mint(to, amount);
    }

    
    function burn(uint256 amount) public {
        require(amount > 0, "Burn amount must be greater than zero");
        _burn(_msgSender(), amount);
    }

   
    function redeem(uint256 amount) public {
        require(amount > 0, "Redeem amount must be greater than zero");
        uint256 cost = amount * tokenCost;
        require(balanceOf(_msgSender()) >= cost, "Insufficient token balance");

       
        _burn(_msgSender(), cost);

       
        redeemedItems[_msgSender()].push(RedeemedItem({
            amount: amount,
            tokensRedeemed: cost
        }));

        emit TokensRedeemed(_msgSender(), amount, cost);
    }

    
    function getRedeemedItems(address account) public view returns (RedeemedItem[] memory) {
        require(account != address(0), "Query for zero address");
        return redeemedItems[account];
    }

    
    function printRedeemedTokens(address account) public view returns (string memory) {
        require(account != address(0), "Query for zero address");
        RedeemedItem[] memory items = redeemedItems[account];
        require(items.length > 0, "No redeemed tokens found");

        string memory result = "";
        for (uint i = 0; i < items.length; i++) {
            result = string(abi.encodePacked(
                result,
                "Redemption ", uintToString(i + 1), ": ", 
                "Amount: ", uintToString(items[i].amount), 
                " Tokens Redeemed: ", uintToString(items[i].tokensRedeemed), 
                "\n"
            ));
        }
        return result;
    }

    
    function uintToString(uint256 v) internal pure returns (string memory) {
        if (v == 0) {
            return "0";
        }
        uint256 digits;
        uint256 temp = v;
        while (temp != 0) {
            digits++;
            temp /= 10;
        }
        bytes memory buffer = new bytes(digits);
        while (v != 0) {
            digits -= 1;
            buffer[digits] = bytes1(uint8(48 + uint256(v % 10)));
            v /= 10;
        }
        return string(buffer);
    }

   
    function checkBalance(address account) public view returns (uint256) {
        require(account != address(0), "Query for zero address");
        return balanceOf(account);
    }

    
    function transferTokens(address from, address to, uint256 amount) public {
        require(amount > 0, "Transfer amount must be greater than zero");
        require(from != address(0), "Cannot transfer from zero address");
        require(to != address(0), "Cannot transfer to zero address");

        if (from == _msgSender()) {
            _transfer(from, to, amount);
        } else {
            uint256 currentAllowance = allowance(from, _msgSender());
            require(currentAllowance >= amount, "Transfer amount exceeds allowance");
            _approve(from, _msgSender(), currentAllowance - amount);
            _transfer(from, to, amount);
        }
    } 
    }

    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;

    import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
    import "@openzeppelin/contracts/access/Ownable.sol";

    contract DengenToken is ERC20, Ownable {
        constructor() ERC20("Degen", "DGN") Ownable(msg.sender) {}

    struct RedeemedItem {
        uint256 amount;          
        uint256 tokensRedeemed;  
    }

    event TokensRedeemed(address indexed redeemer, uint256 amount, uint256 tokensRedeemed);

    uint256 public tokenCost = 1; 

    mapping(address => RedeemedItem[]) private redeemedItems;

    
    function mint(address to, uint256 amount) public onlyOwner {
        require(to != address(0), "Cannot mint to zero address");
        require(amount > 0, "Mint amount must be greater than zero");
        _mint(to, amount);
    }

    
    function burn(uint256 amount) public {
        require(amount > 0, "Burn amount must be greater than zero");
        _burn(_msgSender(), amount);
    }

   
    function redeem(uint256 amount) public {
        require(amount > 0, "Redeem amount must be greater than zero");
        uint256 cost = amount * tokenCost;
        require(balanceOf(_msgSender()) >= cost, "Insufficient token balance");

       
        _burn(_msgSender(), cost);

       
        redeemedItems[_msgSender()].push(RedeemedItem({
            amount: amount,
            tokensRedeemed: cost
        }));

        emit TokensRedeemed(_msgSender(), amount, cost);
    }

    
    function getRedeemedItems(address account) public view returns (RedeemedItem[] memory) {
        require(account != address(0), "Query for zero address");
        return redeemedItems[account];
    }

    
    function printRedeemedTokens(address account) public view returns (string memory) {
        require(account != address(0), "Query for zero address");
        RedeemedItem[] memory items = redeemedItems[account];
        require(items.length > 0, "No redeemed tokens found");

        string memory result = "";
        for (uint i = 0; i < items.length; i++) {
            result = string(abi.encodePacked(
                result,
                "Redemption ", uintToString(i + 1), ": ", 
                "Amount: ", uintToString(items[i].amount), 
                " Tokens Redeemed: ", uintToString(items[i].tokensRedeemed), 
                "\n"
            ));
        }
        return result;
    }

    
    function uintToString(uint256 v) internal pure returns (string memory) {
        if (v == 0) {
            return "0";
        }
        uint256 digits;
        uint256 temp = v;
        while (temp != 0) {
            digits++;
            temp /= 10;
        }
        bytes memory buffer = new bytes(digits);
        while (v != 0) {
            digits -= 1;
            buffer[digits] = bytes1(uint8(48 + uint256(v % 10)));
            v /= 10;
        }
        return string(buffer);
    }

   
    function checkBalance(address account) public view returns (uint256) {
        require(account != address(0), "Query for zero address");
        return balanceOf(account);
    }

    
    function transferTokens(address from, address to, uint256 amount) public {
        require(amount > 0, "Transfer amount must be greater than zero");
        require(from != address(0), "Cannot transfer from zero address");
        require(to != address(0), "Cannot transfer to zero address");

        if (from == _msgSender()) {
            _transfer(from, to, amount);
        } else {
            uint256 currentAllowance = allowance(from, _msgSender());
            require(currentAllowance >= amount, "Transfer amount exceeds allowance");
            _approve(from, _msgSender(), currentAllowance - amount);
            _transfer(from, to, amount);
        }
    } 
    }

    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;

    import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
    import "@openzeppelin/contracts/access/Ownable.sol";

    contract DengenToken is ERC20, Ownable {
        constructor() ERC20("Degen", "DGN") Ownable(msg.sender) {}

    struct RedeemedItem {
        uint256 amount;          
        uint256 tokensRedeemed;  
    }

    event TokensRedeemed(address indexed redeemer, uint256 amount, uint256 tokensRedeemed);

    uint256 public tokenCost = 1; 

    mapping(address => RedeemedItem[]) private redeemedItems;

    
    function mint(address to, uint256 amount) public onlyOwner {
        require(to != address(0), "Cannot mint to zero address");
        require(amount > 0, "Mint amount must be greater than zero");
        _mint(to, amount);
    }

    
    function burn(uint256 amount) public {
        require(amount > 0, "Burn amount must be greater than zero");
        _burn(_msgSender(), amount);
    }

   
    function redeem(uint256 amount) public {
        require(amount > 0, "Redeem amount must be greater than zero");
        uint256 cost = amount * tokenCost;
        require(balanceOf(_msgSender()) >= cost, "Insufficient token balance");

       
        _burn(_msgSender(), cost);

       
        redeemedItems[_msgSender()].push(RedeemedItem({
            amount: amount,
            tokensRedeemed: cost
        }));

        emit TokensRedeemed(_msgSender(), amount, cost);
    }

    
    function getRedeemedItems(address account) public view returns (RedeemedItem[] memory) {
        require(account != address(0), "Query for zero address");
        return redeemedItems[account];
    }

    
    function printRedeemedTokens(address account) public view returns (string memory) {
        require(account != address(0), "Query for zero address");
        RedeemedItem[] memory items = redeemedItems[account];
        require(items.length > 0, "No redeemed tokens found");

        string memory result = "";
        for (uint i = 0; i < items.length; i++) {
            result = string(abi.encodePacked(
                result,
                "Redemption ", uintToString(i + 1), ": ", 
                "Amount: ", uintToString(items[i].amount), 
                " Tokens Redeemed: ", uintToString(items[i].tokensRedeemed), 
                "\n"
            ));
        }
        return result;
    }

    
    function uintToString(uint256 v) internal pure returns (string memory) {
        if (v == 0) {
            return "0";
        }
        uint256 digits;
        uint256 temp = v;
        while (temp != 0) {
            digits++;
            temp /= 10;
        }
        bytes memory buffer = new bytes(digits);
        while (v != 0) {
            digits -= 1;
            buffer[digits] = bytes1(uint8(48 + uint256(v % 10)));
            v /= 10;
        }
        return string(buffer);
    }

   
    function checkBalance(address account) public view returns (uint256) {
        require(account != address(0), "Query for zero address");
        return balanceOf(account);
    }

    
    function transferTokens(address from, address to, uint256 amount) public {
        require(amount > 0, "Transfer amount must be greater than zero");
        require(from != address(0), "Cannot transfer from zero address");
        require(to != address(0), "Cannot transfer to zero address");

        if (from == _msgSender()) {
            _transfer(from, to, amount);
        } else {
            uint256 currentAllowance = allowance(from, _msgSender());
            require(currentAllowance >= amount, "Transfer amount exceeds allowance");
            _approve(from, _msgSender(), currentAllowance - amount);
            _transfer(from, to, amount);
        }
    } 
    }
## HELP
Compiler Mistakes: Solidity version 0.8.18 or higher is installed on  system.

## AUTHOR
Sanskriti Anand
SANSKRITIANAND@132

## LICENSE
This project is licensed under the METACRAFT License https://github.com/Sanskriti132/Mytooken/new/main?filename=README.md
