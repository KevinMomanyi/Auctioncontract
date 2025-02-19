NFT Auction Smart Contract
A Starknet smart contract that enables NFT auctions with bidding functionality. This contract allows users to create auctions for NFTs, place bids, and automatically determines the winner based on the highest bid.

Features
Create auctions for NFTs with customizable duration and starting price
Place bids with automatic highest bid tracking
Automatic auction expiration based on duration
Event emission for auction creation, bidding, and completion
View functions for auction status and current highest bid
Owner-controlled auction finalization
Prerequisites
Rust (latest stable version)
Cairo (latest version)
Scarb (Starknet's package manager)
Starkli (CLI tool for Starknet)
Starknet-devnet (for local development)
Installation
Install Rust:
# Windows
Visit https://rustup.rs/ and download rustup-init.exe

# Unix-based systems
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
Install Scarb:
# Windows (PowerShell)
curl --proto '=https' --tlsv1.2 -sSf https://docs.swmansion.com/scarb/install.sh | sh

# Unix-based systems
curl --proto '=https' --tlsv1.2 -sSf https://docs.swmansion.com/scarb/install.sh | sh
Install Starkli:
# Using cargo
cargo install starkli

# Setup directories
mkdir ~/.starkli-wallets
mkdir ~/.starkli-accounts
Install Starknet-devnet:
# Using cargo
cargo install starknet-devnet

# Or using Docker
docker pull shardlabs/starknet-devnet
Project Setup
Clone the repository:
git clone <repository-url>
cd nft-auction
Build the project:
scarb build
Contract Structure
src/
├── lib.cairo          # Main contract file
└── tests/            # Test files
    └── test_auction.cairo

Scarb.toml           # Project configuration
README.md           # This file
Usage
Setting Up Local Development Environment
Start the local Starknet devnet:
# Using native installation
starknet-devnet

# Or using Docker
docker run -p 9545:9545 shardlabs/starknet-devnet
Set up your account and environment:
# Set environment variables
export STARKNET_RPC=http://localhost:9545
export STARKNET_ACCOUNT=~/.starkli-wallets/account.json
export STARKNET_KEYSTORE=~/.starkli-wallets/deployer.json

# Create keystore and account
starkli signer keystore new ~/.starkli-wallets/deployer.json
starkli account oz init ~/.starkli-wallets/account.json
Deploying the Contract
starkli deploy ./target/dev/nft_auction_NFTAuction.contract_class.json \
    $ACCOUNT_ADDRESS \
    0x1234... \       # NFT contract address
    u256:1 \         # token_id
    u256:1000000 \   # start_price (in wei)
    u64:3600        # duration (in seconds)
Interacting with the Contract
Place a bid:
starkli invoke <CONTRACT_ADDRESS> place_bid --value 1000000
Check highest bid:
starkli call <CONTRACT_ADDRESS> get_highest_bid
End auction (owner only):
starkli invoke <CONTRACT_ADDRESS> end_auction
Contract Interface
Events
AuctionCreated: Emitted when a new auction is created
BidPlaced: Emitted when a new bid is placed
AuctionEnded: Emitted when the auction is finalized
Functions
constructor: Initializes the auction with NFT details and parameters
place_bid: Allows users to place bids
end_auction: Allows owner to end the auction after duration
get_highest_bid: View current highest bid
get_highest_bidder: View current highest bidder
get_auction_end_time: View auction end time
is_active: Check if auction is still active
Testing
Run the test suite:

scarb test
Security Considerations
Ensure proper access control for admin functions
Verify bid amounts are greater than previous bids
Check auction timeframes and status before actions
Validate NFT ownership and approvals
Consider front-running protection for bids
