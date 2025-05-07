# Experiment 6: Blockchain-Based Passwordless Authentication (Using Public-Private Key Cryptography)
# Aim:
To implement a secure passwordless authentication system using public-private key cryptography on Ethereum. This prevents phishing and password leaks.

# Algorithm:
Step 1: User Registration
A user registers with their Ethereum public key (instead of a password).


Step 2: Login Process
When logging in, the user signs a random challenge message using their private key.

Step 3: Signature Verification
The backend or smart contract verifies the signed challenge using the user’s public key to confirm their identity.

 Step 4: Nonce Management
Generate a new random nonce (challenge) each time a user attempts to log in to prevent replay attacks.

 Step 5: Session Token Generation
After successful verification, issue a temporary session token (JWT) for authenticated access without needing to sign every request.


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
Users can register without a password.


Users sign a challenge with their private key for authentication.


The smart contract verifies signatures to confirm identity.



# High-Level Overview:
Eliminates password hacks & phishing attacks.


Uses Ethereum's built-in cryptographic functions.


Inspired by Web3 login solutions like MetaMask authentication.
## OUTPUT:
![Screenshot 2025-05-07 082316](https://github.com/user-attachments/assets/8be484b3-785f-4606-9fca-d9b3830a4dad)
![Screenshot 2025-05-07 082539](https://github.com/user-attachments/assets/506fabb1-9c94-4448-a8f6-4e732ed8a216)
![Screenshot 2025-05-07 082548](https://github.com/user-attachments/assets/d5b831b8-8f4a-4d05-ad3e-ed97cd756124)
![Screenshot 2025-05-07 082558](https://github.com/user-attachments/assets/3cbe4582-3e3b-45a2-8f23-ddd2d120b67e)

# RESULT: 
Thus, a Blockchain-Based Passwordless Authentication has been successfully built and implemented on Remix - Ethereum IDE.
