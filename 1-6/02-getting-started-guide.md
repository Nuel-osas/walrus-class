# Getting Started with Walrus Storage

This guide will walk you through setting up Walrus and performing your first storage operations.

## Prerequisites

Before starting, you'll need:
- A computer running macOS, Linux (Ubuntu), or Windows
- Internet connection
- Basic command-line familiarity
- Some SUI tokens (for Mainnet) or testnet SUI (for Testnet)

## Step 1: Install the Walrus CLI

### Method 1: Using the Install Script (Recommended)

**For Mainnet:**
```bash
curl -sSf https://install.wal.app | sh
```

**For Testnet:**
```bash
curl -sSf https://install.wal.app | sh -s -- -n testnet
```

The installer will:
- Download the appropriate binary for your system
- Place it in `$HOME/.local/bin`
- Make it executable

**Add to PATH:**
```bash
# Add to your shell profile (~/.bashrc, ~/.zshrc, etc.)
export PATH="$HOME/.local/bin:$PATH"

# Reload your shell configuration
source ~/.bashrc  # or source ~/.zshrc
```

### Method 2: Manual Binary Download

**Step 1:** Set your system type:
```bash
# Choose one based on your system:
export SYSTEM="ubuntu-x86_64"        # Linux (most compatible)
export SYSTEM="ubuntu-x86_64-generic" # Linux (generic)
export SYSTEM="macos-x86_64"          # macOS Intel
export SYSTEM="macos-arm64"           # macOS Apple Silicon (M1/M2/M3)
export SYSTEM="windows-x86_64.exe"    # Windows
```

**Step 2:** Download the binary:
```bash
# For Mainnet
curl https://storage.googleapis.com/mysten-walrus-binaries/walrus-mainnet-latest-$SYSTEM -o walrus

# For Testnet
curl https://storage.googleapis.com/mysten-walrus-binaries/walrus-testnet-latest-$SYSTEM -o walrus
```

**Step 3:** Make it executable and move to PATH:
```bash
chmod +x walrus
sudo mv walrus /usr/local/bin/
# Or move to ~/.local/bin/ if you don't have sudo access
```

### Method 3: Using suiup (Alternative)

```bash
# Install or update suiup first
curl --proto '=https' --tlsv1.2 -sSf https://suiup.mystenlabs.com/install | sh

# Install walrus via suiup
suiup install walrus
```

### Verify Installation

```bash
walrus --version
```

You should see output showing the Walrus version.

## Step 2: Set Up a Sui Wallet

Walrus uses Sui for coordination and payments, so you need a Sui wallet.

### Install Sui CLI (if not already installed)

```bash
cargo install --locked --git https://github.com/MystenLabs/sui.git --branch mainnet sui
```

Or download binaries from: https://docs.sui.io/guides/developer/getting-started/sui-install

### Generate a New Wallet

```bash
# Generate a new Sui wallet
sui client new-address ed25519

# This will create a wallet and show your address
# Save the recovery phrase securely!
```

### Check Your Wallet Address

```bash
sui client active-address
```

### Switch Between Networks

```bash
# List available networks
sui client envs

# Switch to testnet
sui client switch --env testnet

# Switch to mainnet
sui client switch --env mainnet
```

## Step 3: Get SUI Tokens

### For Testnet (FREE)

**Option 1: Using Sui CLI**
```bash
sui client faucet
```

**Option 2: Discord Faucet**
1. Join Sui Discord: https://discord.gg/sui
2. Go to #testnet-faucet channel
3. Type: `!faucet <your-sui-address>`

**Option 3: Web Faucet**
Visit: https://faucet.testnet.sui.io/

### For Mainnet (PURCHASE REQUIRED)

You'll need to purchase SUI tokens from an exchange:
- Binance, Coinbase, Kraken, or other exchanges
- Transfer to your Sui wallet address

### Verify Balance

```bash
sui client gas
```

## Step 4: Configure Walrus

### Check Current Configuration

```bash
walrus info
```

This shows:
- Network parameters
- Number of storage nodes
- Shard configuration
- Pricing information

### Custom Configuration (Optional)

You can create a custom config file:

```bash
# View current config location
walrus --help | grep config

# Use custom config
walrus --config /path/to/custom-config.yaml info
```

### Set Custom Wallet (Optional)

```bash
walrus --wallet /path/to/wallet.yaml store file.txt
```

## Step 5: Your First Blob Upload

### Store a Simple Text File

**Create a test file:**
```bash
echo "Hello, Walrus!" > hello.txt
```

**Upload to Walrus:**
```bash
# Store for 5 epochs
walrus store hello.txt --epochs 5
```

**Expected Output:**
```
Successfully stored blob with ID: <BLOB_ID>
Blob object ID: <OBJECT_ID>
Cost: X.XX SUI
Expires at epoch: XXX
```

**Save the Blob ID!** You'll need it to retrieve your data.

### Retrieve Your Blob

```bash
# Read blob content to stdout
walrus read <BLOB_ID>

# Save blob to file
walrus read <BLOB_ID> --out retrieved-hello.txt

# Verify the content
cat retrieved-hello.txt
```

## Step 6: Explore Basic Commands

### Check Blob Status

```bash
walrus blob-status --blob-id <BLOB_ID>
```

Shows:
- Whether blob is stored
- Which nodes have it
- Expiration epoch

### List Your Blobs

```bash
# List all your active blobs
walrus list-blobs

# Include expired blobs
walrus list-blobs --include-expired
```

### Get Blob ID from File

```bash
walrus blob-id hello.txt
```

This derives the blob ID without actually uploading.

### Check System Health

```bash
walrus health --committee
```

Shows which storage nodes are online and healthy.

## Step 7: Storage Duration Options

### Store for Maximum Duration

```bash
walrus store file.txt --epochs max
```

### Store Until Specific Date

```bash
walrus store file.txt --earliest-expiry-time "2025-12-31T23:59:59Z"
```

### Store Until Specific Epoch

```bash
walrus store file.txt --end-epoch 1000
```

## Step 8: Advanced Features

### Make Blob Deletable

```bash
walrus store sensitive.txt --epochs 10 --deletable
```

Later, you can delete it:
```bash
walrus delete --blob-id <BLOB_ID>
```

### Create Shared Blob

```bash
# Anyone can fund/extend this blob
walrus store public-data.txt --epochs 10 --share --amount 1000000000
```

### Use Upload Relay (for better connectivity)

```bash
walrus store file.txt --epochs 10 --upload-relay https://relay.walrus.xyz
```

## Troubleshooting

### "Insufficient gas" Error

**Solution:** Get more SUI tokens
```bash
# Check balance
sui client gas

# Get testnet tokens
sui client faucet
```

### "Blob not found" Error

**Possible causes:**
- Blob ID is incorrect
- Blob has expired
- Not enough storage nodes online

**Solution:** Check blob status
```bash
walrus blob-status --blob-id <BLOB_ID>
```

### "Connection refused" Error

**Solution:** Check network connectivity and node health
```bash
walrus health --committee
```

### Binary Not Found After Installation

**Solution:** Add to PATH
```bash
export PATH="$HOME/.local/bin:$PATH"
source ~/.bashrc
```

## Quick Reference Card

```bash
# Installation
curl -sSf https://install.wal.app | sh

# Verify installation
walrus --version

# Get testnet SUI
sui client faucet

# Upload file
walrus store file.txt --epochs 5

# Download file
walrus read <BLOB_ID> --out output.txt

# Check status
walrus blob-status --blob-id <BLOB_ID>

# List your blobs
walrus list-blobs

# Get system info
walrus info

# Check health
walrus health --committee
```

## Next Steps

Now that you have Walrus set up, explore:

1. **Code Integration**: Use TypeScript/Python SDKs
2. **Quilts**: Store multiple related files efficiently
3. **Blob Attributes**: Add metadata to your blobs
4. **Walrus Sites**: Host entire websites on Walrus

See the code examples guide for hands-on tutorials!

## Useful Resources

- **Official Documentation**: https://docs.wal.app
- **CLI Reference**: https://docs.wal.app/usage/client-cli.html
- **Sui Documentation**: https://docs.sui.io
- **Community Discord**: https://discord.gg/sui
- **GitHub Examples**: https://github.com/MystenLabs/walrus-docs

## Practice Exercises

1. Store a text file for 10 epochs
2. Retrieve the blob and verify content
3. Create a deletable blob and then delete it
4. List all your active blobs
5. Store an image file and retrieve it
6. Check the health of storage nodes
7. Calculate blob ID without uploading

---

**Congratulations!** You're now ready to use Walrus for decentralized storage. 🎉
