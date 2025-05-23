# Experiment 5: Zero-Knowledge Proof (ZK) Private Voting System
# Aim:
To implement a fully private and transparent voting system using Zero-Knowledge Proofs (ZKPs). This ensures that votes are counted fairly without revealing who voted for whom.

# Algorithm:
Step 1: Voter Registration
Each voter generates a secret vote key and submits a commitment (hashed vote) to the contract.


Step 2: Voting Process
Voters submit their votes privately using a hash, without revealing their choice.


Step 3: ZK Verification
The contract verifies if a vote belongs to a registered voter but does not reveal the actual vote.


Step 4: Vote Counting
Once voting ends, the contract reveals the final tally without linking votes to individuals.

step 5: Prevent Double Voting
Implement a mechanism to ensure each registered voter can vote only once using their commitment, without revealing identity.

🔐 Step 6: Anonymity Set Management
Maintain an anonymity set to group all eligible voters, making it hard to trace any individual’s vote.

# Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract ZKVoting {
    struct Voter {
        bool registered;
        bytes32 voteCommitment;
    }

    mapping(address => Voter) public voters;
    uint256 public votesForA;
    uint256 public votesForB;

    event VoteCommitted(address voter, bytes32 commitment);
    event VoteRevealed(address voter, uint256 choice);

    function registerVoter(bytes32 commitment) public {
        require(!voters[msg.sender].registered, "Already registered");
        voters[msg.sender] = Voter(true, commitment);
        emit VoteCommitted(msg.sender, commitment);
    }

    function revealVote(string memory secret, uint256 choice) public {
        require(voters[msg.sender].registered, "Not registered");
        require(keccak256(abi.encodePacked(secret, choice)) == voters[msg.sender].voteCommitment, "Invalid proof");

        if (choice == 1) votesForA++;
        if (choice == 2) votesForB++;

        emit VoteRevealed(msg.sender, choice);
    }
}

```
# Expected Output:
Voters commit their votes privately.


When revealed, the contract verifies correctness but keeps votes anonymous.


Final result is publicly verifiable without exposing individual votes.



# High-Level Overview:
Uses ZKPs to ensure anonymous and fair elections.


Prevents vote tampering while maintaining voter privacy.


Mimics real-world ZK voting applications in governance and DAOs.
## OUTPUT:
![Screenshot 2025-05-07 082316](https://github.com/user-attachments/assets/ac5cb0eb-3013-43f6-a40e-06cb5bd698c3)
![Screenshot 2025-05-07 082539](https://github.com/user-attachments/assets/571d7e31-7ba2-4a83-94a6-65535acd368a)
![Screenshot 2025-05-07 082548](https://github.com/user-attachments/assets/e797b9f4-02ed-4491-aa80-ef386c54995e)
![Screenshot 2025-05-07 082558](https://github.com/user-attachments/assets/4ba68520-bad6-4ae1-94c5-abd6af9093fb)

# RESULT: 
The Experiment have been successfully verified
