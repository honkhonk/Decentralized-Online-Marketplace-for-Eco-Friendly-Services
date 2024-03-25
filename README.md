# Decentralized-Online-Marketplace-for-Eco-Friendly-Services
構築WEB3ベースのプラットフォームにより、個人がエコフレンドリーなサービス（グリーンクリーニング、持続可能な造園など）を提供し、発見することができます。
from web3 import Web3, HTTPProvider

# Sample Solidity Smart Contract (Deploy this separately on Remix and use its address and ABI here)
'''
pragma solidity ^0.8.0;

contract EcoFriendlyMarketplace {
    struct Service {
        string name;
        string description;
        address payable provider;
        uint price;
    }
    
    Service[] public services;
    
    function addService(string memory name, string memory description, uint price) public {
        services.push(Service(name, description, payable(msg.sender), price));
    }
    
    function getService(uint index) public view returns (string memory, string memory, address, uint) {
        Service memory s = services[index];
        return (s.name, s.description, s.provider, s.price);
    }
    
    function purchaseService(uint index) public payable {
        require(msg.value >= services[index].price, "Insufficient funds sent.");
        services[index].provider.transfer(msg.value);
    }
}
'''

# Connection to Rinkeby Testnet
w3 = Web3(HTTPProvider("https://rinkeby.infura.io/v3/YOUR_INFURA_PROJECT_ID"))

# Ensure connection is successful
if w3.isConnected():
    print("Connected to Rinkeby Testnet")
else:
    print("Failed to connect to Rinkeby Testnet")

# Contract Address and ABI (Replace these with your contract's details)
contract_address = w3.toChecksumAddress("YOUR_CONTRACT_ADDRESS")
contract_abi = 'YOUR_CONTRACT_ABI'

# Creating the contract instance
contract = w3.eth.contract(address=contract_address, abi=contract_abi)

# Function to add a service
def add_service(name, description, price):
    txn = contract.functions.addService(name, description, price).buildTransaction({
        'from': w3.eth.accounts[0],
        'nonce': w3.eth.getTransactionCount(w3.eth.accounts[0]),
        # Additional transaction parameters like gasPrice and gas can be added here
    })
    # Here you would typically sign and send the transaction
    print(f"Adding service: {name}, {description}, Price: {price} WEI")

# Function to view a service
def view_service(index):
    service = contract.functions.getService(index).call()
    print(f"Service {index}: Name: {service[0]}, Description: {service[1]}, Provider: {service[2]}, Price: {service[3]} WEI")

# Example Usage (Replace these with actual transaction signing and sending)
add_service("Green Cleaning", "Eco-friendly residential cleaning service", 1000000000000000000)  # 1 ETH for simplicity
view_service(0)

# Remember, this is a simplified example. Real-world applications require managing private keys securely,
# handling transaction receipts, event listening, error handling, and more.
