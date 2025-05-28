// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Project {
    
    // Structure to store user identity
    struct UserIdentity {
        string fullName;
        string documentHash;
        bool verified;
        address verifiedBy;
        uint256 createdAt;
    }
    
    // Contract state variables
    address public contractOwner;
    mapping(address => UserIdentity) public userIdentities;
    mapping(address => bool) public authorizedVerifiers;
    uint256 public totalRegistrations;
    
    // Events
    event IdentityRegistered(address indexed user, string name, uint256 timestamp);
    event IdentityVerified(address indexed user, address indexed verifier, uint256 timestamp);
    event VerifierAuthorized(address indexed verifier);
    
    // Constructor
    constructor() {
        contractOwner = msg.sender;
        authorizedVerifiers[msg.sender] = true;
    }
    
    // Modifier for owner only functions
    modifier onlyOwner() {
        require(msg.sender == contractOwner, "Access denied: Owner only");
        _;
    }
    
    // Modifier for authorized verifiers
    modifier onlyAuthorizedVerifier() {
        require(authorizedVerifiers[msg.sender], "Access denied: Not authorized verifier");
        _;
    }
    
    // Core Function 1: Register Identity
    function registerIdentity(string memory _name, string memory _documentHash) public {
        require(bytes(_name).length > 0, "Name cannot be empty");
        require(bytes(_documentHash).length > 0, "Document hash cannot be empty");
        require(bytes(userIdentities[msg.sender].fullName).length == 0, "Identity already registered");
        
        userIdentities[msg.sender] = UserIdentity({
            fullName: _name,
            documentHash: _documentHash,
            verified: false,
            verifiedBy: address(0),
            createdAt: block.timestamp
        });
        
        totalRegistrations++;
        emit IdentityRegistered(msg.sender, _name, block.timestamp);
    }
    
    // Core Function 2: Verify Identity
    function verifyIdentity(address _userAddress) public onlyAuthorizedVerifier {
        require(bytes(userIdentities[_userAddress].fullName).length > 0, "User not registered");
        require(!userIdentities[_userAddress].verified, "Identity already verified");
        require(_userAddress != msg.sender, "Cannot verify own identity");
        
        userIdentities[_userAddress].verified = true;
        userIdentities[_userAddress].verifiedBy = msg.sender;
        
        emit IdentityVerified(_userAddress, msg.sender, block.timestamp);
    }
    
    // Core Function 3: Authorize Verifier
    function authorizeVerifier(address _verifierAddress) public onlyOwner {
        require(_verifierAddress != address(0), "Invalid verifier address");
        require(!authorizedVerifiers[_verifierAddress], "Verifier already authorized");
        
        authorizedVerifiers[_verifierAddress] = true;
        emit VerifierAuthorized(_verifierAddress);
    }
    
    // Function to check if identity is verified
    function isIdentityVerified(address _userAddress) public view returns (bool) {
        return userIdentities[_userAddress].verified;
    }
    
    // Function to get identity details
    function getIdentityDetails(address _userAddress) public view returns (
        string memory name,
        string memory documentHash,
        bool verified,
        address verifiedBy,
        uint256 createdAt
    ) {
        UserIdentity memory identity = userIdentities[_userAddress];
        return (
            identity.fullName,
            identity.documentHash,
            identity.verified,
            identity.verifiedBy,
            identity.createdAt
        );
    }
    
    // Function to check if address is authorized verifier
    function isAuthorizedVerifier(address _verifierAddress) public view returns (bool) {
        return authorizedVerifiers[_verifierAddress];
    }
}
