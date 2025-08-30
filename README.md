# SmoothieRecipeMktSMUTHYrewards
Smart contracts for the SmoothieMkt on-chain reward system on the XDC Network.
Okay, here's a full codebase and strategy documentation for the SmoothieMkt MVP, formatted as a README.md file. It includes the Phase 1 implementation details and the Phase 2 roadmap.
​SmoothieMkt: On-Chain Rewards System
​Overview 📝
​SmoothieMkt is a creator economy game where users can buy and sell digital smoothie recipes. This repository contains the Solidity smart contracts that power the on-chain reward mechanism for the MVP (Minimum Viable Product). The system is built on the XDC Network to leverage its low fees, high transaction speeds, and EVM compatibility. 
Core Concepts 💡
​SMUTHY Token: An XRC-20 token with a fixed supply of 1 billion, minted to reward early platform participants.
​SmoothieMktPoolSMUTHY: A dedicated smart contract that holds the entire SMUTHY token supply and manages the distribution of rewards.
​Transaction-Based Rewards: Users are rewarded with SMUTHY tokens based on a percentage of each transaction's value.
​Phase 1: MVP Implementation (Fixed Supply)
​Objective
​To create a functional, secure, and transparent reward system for early adopters on the XDC Apothem Testnet.
​Tech Stack 🛠️
​Blockchain: XDC Network (Apothem Testnet)
​Smart Contracts: Solidity
​Development Tools: Remix IDE for XDC, XDCPay Wallet, XDCScan Testnet Explorer
​Browser Compatibility: Chrome
​Smart Contracts
​1. SMUTHY.sol
​This is the main token contract. It defines the SMUTHY token and handles the initial supply minting.
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract SMUTHY is ERC20, Ownable {
    constructor() ERC20("SMUTHY Token", "SMUTHY") Ownable(msg.sender) {
        // Mints 1 billion tokens to the deployer's address
        _mint(msg.sender, 1000000000 * 10 ** decimals()); 
    }
}

2. SmoothieMktPoolSMUTHY.sol
​This contract manages the reward distribution logic. The entire initial supply of SMUTHY will be transferred here after deployment.

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/utils/math/SafeMath.sol";

contract SmoothieMktPoolSMUTHY is Ownable {
    using SafeMath for uint256;

    ERC20 public SMUTHY_TOKEN;

    uint256 public constant BUYER_REWARD_PERCENTAGE = 250; 
    uint256 public constant CREATOR_REWARD_PERCENTAGE = 150;
    uint256 public constant SELLER_REWARD_PERCENTAGE = 50;
    uint256 public constant PERCENTAGE_BASE = 10000;

    constructor() Ownable(msg.sender) {}

    function setSMUTHYTokenAddress(address _smuthyTokenAddress) public onlyOwner {
        require(address(SMUTHY_TOKEN) == address(0), "SMUTHY token address already set");
        SMUTHY_TOKEN = ERC20(_smuthyTokenAddress);
    }

    function distributeRewards(
        address _buyer,
        address _creator,
        address _seller,
        uint256 _transactionValue
    ) public onlyOwner {
        uint256 buyerReward = _transactionValue.mul(BUYER_REWARD_PERCENTAGE).div(PERCENTAGE_BASE);
        uint256 creatorReward = _transactionValue.mul(CREATOR_REWARD_PERCENTAGE).div(PERCENTAGE_BASE);
        uint256 sellerReward = _transactionValue.mul(SELLER_REWARD_PERCENTAGE).div(PERCENTAGE_BASE);

        uint256 totalReward = buyerReward.add(creatorReward).add(sellerReward);
        require(SMUTHY_TOKEN.balanceOf(address(this)) >= totalReward, "Not enough SMUTHY in the pool");

        SMUTHY_TOKEN.transfer(_buyer, buyerReward);
        SMUTHY_TOKEN.transfer(_creator, creatorReward);
        SMUTHY_TOKEN.transfer(_seller, sellerReward);
    }
}

Deployment & Testing Steps
​Set Up: Install the XDCPay Chrome extension and fund your wallet with test XDC from the Apothem Testnet Faucet.
​Deploy SMUTHY.sol: Use Remix IDE for XDC to compile and deploy the SMUTHY contract. This will mint all 1 billion tokens to your wallet.
​Deploy SmoothieMktPoolSMUTHY.sol: Deploy the pool contract, making you the owner.
​Fund the Pool: Transfer all 1 billion SMUTHY tokens from your wallet to the deployed SmoothieMktPoolSMUTHY contract address.
​Connect Contracts: Call the setSMUTHYTokenAddress function on SmoothieMktPoolSMUTHY, passing the address of the SMUTHY token.
​Test Distribution: Simulate a transaction by calling distributeRewards with sample addresses and a transaction value to verify rewards are paid out correctly.
​Verification: Use apothem.xdcscan.io to check the balances and transaction history.
​Phase 2: Token Strategy for Value Appreciation
​Once the MVP is live and gains traction, we will implement these strategies to ensure the long-term value and sustainability of the SMUTHY token.
​Small, Transaction-Based Burn: A small, predetermined percentage of SMUTHY rewards will be burned (permanently removed from circulation) with every transaction. This creates a deflationary token economy, directly linking token value to platform activity.
​Future Buyback and Burn Program: Admin7 will use a portion of the SmoothieMkt platform's revenue to buy SMUTHY from the open market. These tokens will then be burned, providing upward price pressure and demonstrating a commitment to the token's long-term value.
​Staking and Governance Model: Users will be able to lock up their SMUTHY tokens in a staking contract to earn additional rewards, gain platform governance rights, or unlock exclusive features. This incentivizes holding the token and reduces the circulating supply.
​This tiered approach allows us to get the core functionality running quickly in Phase 1 and build a robust, sustainable token ecosystem in Phase 2.
​https://www.youtube.com/watch?v=eVGEea7adDM
​This video provides a general template for creating a well-structured README.md file, which is relevant to our documentation update.

