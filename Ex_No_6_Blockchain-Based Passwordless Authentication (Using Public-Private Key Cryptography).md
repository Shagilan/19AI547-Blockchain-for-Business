# Experiment 6: Blockchain-Based Passwordless Authentication (Using Public-Private Key Cryptography)
# Aim:
To implement a secure passwordless authentication system using public-private key cryptography on Ethereum. This prevents phishing and password leaks.

# Algorithm:
Step 1: User Registration
A user registers with their Ethereum public key (instead of a password).


Step 2: Login Process
When logging in, the user signs a random challenge message using their private key.


The smart contract verifies the signature using the user’s public key.



# Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract PasswordlessAuth {
    mapping(address => bool) public registeredUsers;

    event UserRegistered(address user);
    event UserAuthenticated(address user);

    function registerUser() public {
        require(!registeredUsers[msg.sender], "Already registered");
        registeredUsers[msg.sender] = true;
        emit UserRegistered(msg.sender);
    }

    function authenticate(bytes32 hash, uint8 v, bytes32 r, bytes32 s) public view returns (bool) {
        require(registeredUsers[msg.sender], "User not registered");
        address signer = ecrecover(hash, v, r, s);
        return signer == msg.sender;
    }
}
```

# Expected Output:
1.Users can register without a password.


2.Users sign a challenge with their private key for authentication.


3.The smart contract verifies signatures to confirm identity.



# High-Level Overview:
1.Eliminates password hacks & phishing attacks.


2.Uses Ethereum's built-in cryptographic functions.


3.Inspired by Web3 login solutions like MetaMask authentication.

# OUTPUT:
![alt text](<Screenshot 2025-04-28 144106.png>)
![alt text](<Screenshot 2025-04-28 144432.png>)
![alt text](<Screenshot 2025-04-28 145143.png>)
![alt text](<Screenshot 2025-04-28 144747.png>)
![alt text](<Screenshot 2025-04-28 144946.png>)
# RESULT: 
Thus, a Blockchain-Based Passwordless Authentication has been successfully built and implemented on Remix - Ethereum IDE.
