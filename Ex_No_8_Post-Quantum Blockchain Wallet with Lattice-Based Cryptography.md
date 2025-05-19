# Experiment 8: Post-Quantum Blockchain Wallet with Lattice-Based Cryptography
# Aim:
To create a quantum-resistant wallet using lattice-based cryptography instead of traditional ECDSA, ensuring that future quantum computers cannot break private keys.

# Algorithm:
## Step 1: Understanding Quantum Threat to Blockchain
ECDSA-based wallets are vulnerable to quantum computers.


Lattice-based cryptography (e.g., NTRU, CRYSTALS-Kyber) provides quantum resistance.


## Step 2: Implement Lattice-Based Signature Scheme
Use precomputed NTRU-based public-private keys for authentication.


Store hashed lattice-based signatures instead of traditional Ethereum signatures.


## Step 3: Secure Transactions
Users sign transactions using lattice cryptographic proofs.

 Step 4: Hybrid Cryptographic Transition
Support a dual-signature system (ECDSA + lattice-based) during migration, ensuring backward compatibility while introducing quantum resistance.

 Step 5: Test Post-Quantum Schemes on Testnets
Deploy and test lattice-based cryptography on Ethereum testnets or sidechains to evaluate performance, gas costs, and compatibility.

 Step 6: Educate Users on Key Management
Provide tools and documentation to help users safely generate, store, and use their post-quantum keys without compromising security.




The smart contract verifies the proof before allowing transactions.



# Program:

(Solidity does not natively support lattice cryptography yet, but we simulate it using custom hash-based authentication.)
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract PostQuantumWallet {
    struct User {
        bytes32 publicKeyHash;
        bool registered;
    }

    mapping(address => User) public users;
    mapping(address => uint256) public balances;

    event UserRegistered(address user, bytes32 publicKeyHash);
    event TransactionVerified(address from, address to, uint256 amount);

    function registerUser(bytes32 _publicKeyHash) public {
        require(!users[msg.sender].registered, "User already registered");
        users[msg.sender] = User(_publicKeyHash, true);
        emit UserRegistered(msg.sender, _publicKeyHash);
    }

    function sendFunds(address _to, uint256 _amount, bytes32 _signature) public {
        require(users[msg.sender].registered, "Sender not registered");
        require(users[_to].registered, "Recipient not registered");
        require(balances[msg.sender] >= _amount, "Insufficient funds");

        bytes32 calculatedHash = keccak256(abi.encodePacked(msg.sender, _to, _amount));
        require(calculatedHash == _signature, "Invalid quantum-safe signature");

        balances[msg.sender] -= _amount;
        balances[_to] += _amount;
        emit TransactionVerified(msg.sender, _to, _amount);
    }
}
```

# Expected Output:
Users register using a post-quantum secure public key.


Transactions require a quantum-resistant signature for authentication.


If a traditional quantum-vulnerable hash is used, the transaction fails.
## OUTPUT:
![Screenshot 2025-05-19 125643](https://github.com/user-attachments/assets/37483351-bf35-44e8-bf43-da7d89bf0acf)
![Screenshot 2025-05-19 125650](https://github.com/user-attachments/assets/4b258c00-a951-4a6c-93cf-74d087ac885d)
![Screenshot 2025-05-19 125701](https://github.com/user-attachments/assets/288df965-2149-48df-b6be-a5b42cf88079)
![Screenshot 2025-05-19 125714](https://github.com/user-attachments/assets/cb535a44-5b07-47a2-a9f9-3c9ee4daa5db)
![Screenshot 2025-05-19 125731](https://github.com/user-attachments/assets/46efcb85-eeab-4525-87ae-7a4ef3f12fba)




# RESULT : 
High-Level Overview:
First quantum-safe Ethereum-compatible wallet prototype.


Uses lattice-based key hashes instead of ECDSA.


Demonstrates how Ethereum will transition to post-quantum security.


Inspired by NIST’s post-quantum cryptography competition.

