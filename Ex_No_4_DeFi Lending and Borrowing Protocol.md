# Experiment 4: DeFi Lending and Borrowing Protocol
# Aim:
To build a decentralized lending protocol where users can deposit assets to earn interest and borrow assets by providing collateral. This experiment introduces concepts like overcollateralization, liquidity pools, and interest accrual in DeFi.

# Algorithm:
Step 1: Setup Lending and Borrowing Mechanism
Users deposit ETH into the contract as liquidity.


Depositors receive interest based on their deposits.


Borrowers can borrow ETH but must provide collateral (e.g., 150% of the borrowed amount).


Interest on borrowed funds is calculated dynamically based on utilization rate.


Step 2: Implement Overcollateralization
If a borrower’s collateral value drops below a certain liquidation threshold, their collateral is liquidated to repay the debt.


Step 3: Allow Liquidation
If collateral < liquidation threshold, liquidators can repay the borrower's debt and claim their collateral at a discount.

Step 4: Dynamic Interest Rate Model
Use an algorithm (e.g., kinked interest rate curve) that adjusts borrow and deposit rates dynamically based on the utilization ratio of the pool. This incentivizes balanced liquidity and discourages over-borrowing or excess idle funds.

Step 5: Collateral Price Feed Integration
Integrate with a decentralized oracle (like Chainlink) to fetch real-time collateral asset prices, ensuring accurate and tamper-proof valuation for overcollateralization and liquidation logic.

Step 6: Health Factor Monitoring
Introduce a health factor metric for borrowers, calculated from collateral and debt values. If the health factor drops below 1, the position becomes eligible for liquidation.

Step 7: Emergency Pause Function
Add an emergency pause mechanism (e.g., only accessible by a multisig DAO) to temporarily halt lending, borrowing, or liquidations in case of bugs, oracle failure, or an economic attack.



Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract DeFiLending {
    address public owner;
    uint256 public interestRate = 5; // 5% interest per cycle
    uint256 public liquidationThreshold = 150; // 150% collateralization
    mapping(address => uint256) public deposits;
    mapping(address => uint256) public borrowed;
    mapping(address => uint256) public collateral;

    event Deposited(address indexed user, uint256 amount);
    event Borrowed(address indexed user, uint256 amount, uint256 collateral);
    event Liquidated(address indexed user, uint256 debtRepaid, uint256 collateralSeized);

    constructor() {
        owner = msg.sender;
    }

    function deposit() public payable {
        require(msg.value > 0, "Deposit must be greater than zero");
        deposits[msg.sender] += msg.value;
        emit Deposited(msg.sender, msg.value);
    }

    function borrow(uint256 amount) public payable {
        require(msg.value >= (amount * liquidationThreshold) / 100, "Not enough collateral");
        borrowed[msg.sender] += amount;
        collateral[msg.sender] += msg.value;
        payable(msg.sender).transfer(amount);
        emit Borrowed(msg.sender, amount, msg.value);
    }

    function liquidate(address borrower) public {
        require(collateral[borrower] < (borrowed[borrower] * liquidationThreshold) / 100, "Not eligible for liquidation");
        uint256 debt = borrowed[borrower];
        uint256 seizedCollateral = collateral[borrower];

        borrowed[borrower] = 0;
        collateral[borrower] = 0;
        payable(msg.sender).transfer(seizedCollateral);
        emit Liquidated(borrower, debt, seizedCollateral);
    }
}

```
# Expected Output:
Users can deposit ETH and earn interest.


Users can borrow ETH by providing collateral.


If collateral < 150% of borrowed amount, liquidators can seize the collateral.



# High-Level Overview:
Teaches key DeFi concepts: lending, borrowing, collateral, liquidation.


Introduces risk management: overcollateralization and liquidation.


Directly related to DeFi protocols like Aave and Compound.

# RESULT : 
![Screenshot 2025-05-07 082247](https://github.com/user-attachments/assets/a52b9693-7587-4205-a7ce-a32cea4651a0)
![Screenshot 2025-05-07 082256](https://github.com/user-attachments/assets/953bf77e-45e5-4b30-b2f8-fa6d43185d83)
![Screenshot 2025-05-07 082305](https://github.com/user-attachments/assets/85b635eb-4282-4563-81cb-d638e1cbeaac)
![Screenshot 2025-05-07 082316](https://github.com/user-attachments/assets/0eed542b-5099-464f-b5b2-1c88c02139e6)

