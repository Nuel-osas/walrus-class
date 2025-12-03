# Walrus CLI Quick Reference Cheat Sheet

## Installation

```bash
# Mainnet
curl -sSf https://install.wal.app | sh

# Testnet
curl -sSf https://install.wal.app | sh -s -- -n testnet

# Verify
walrus --version
```

## Basic Commands

### Store (Upload)

```bash
# Basic upload
walrus store <FILE> --epochs <NUM>

# Store for maximum duration
walrus store file.txt --epochs max

# Store as deletable
walrus store file.txt --epochs 10 --deletable

# Store with specific end date
walrus store file.txt --earliest-expiry-time "2025-12-31T23:59:59Z"

# Store with specific end epoch
walrus store file.txt --end-epoch 1000

# Create shared blob
walrus store file.txt --epochs 20 --share --amount 5000000000
```

### Read (Download)

```bash
# Read to stdout
walrus read <BLOB_ID>

# Save to file
walrus read <BLOB_ID> --out output.txt

# With strict consistency check
walrus read <BLOB_ID> --strict-consistency-check

# Skip consistency check (faster)
walrus read <BLOB_ID> --skip-consistency-check
```

### Blob Information

```bash
# Check blob status
walrus blob-status --blob-id <BLOB_ID>
walrus blob-status --file <FILE>

# Get blob ID from file (without uploading)
walrus blob-id <FILE>

# List your blobs
walrus list-blobs
walrus list-blobs --include-expired
```

### Delete & Extend

```bash
# Delete deletable blob
walrus delete --blob-id <BLOB_ID>
walrus delete --file <FILE>
walrus delete --object-id <SUI_ID> --yes

# Extend blob lifetime
walrus extend --blob-obj-id <BLOB_OBJ_ID> --epochs 10
walrus extend --blob-obj-id <BLOB_OBJ_ID> --shared --epochs 10
```

### Shared Blob Operations

```bash
# Convert to shared blob
walrus share --blob-obj-id <BLOB_OBJ_ID>

# Fund shared blob
walrus fund-shared-blob --blob-obj-id <BLOB_OBJ_ID> --amount <SUI_AMOUNT>
```

## Quilts (Multiple Files)

### Store Quilt

```bash
# Store directory as quilt
walrus store-quilt --epochs 10 --paths <DIRECTORY>

# Store with specific blob configuration
walrus store-quilt --epochs 10 \
  --blobs '{"path":"file1.txt","identifier":"doc1","tags":{"type":"text"}}'
```

### Read Quilt

```bash
# Read all patches
walrus read-quilt --quilt-id <QUILT_ID> --out <OUTPUT_DIR>

# Read specific files
walrus read-quilt --quilt-id <QUILT_ID> --identifiers file1 file2 --out <DIR>

# Read by tag
walrus read-quilt --quilt-id <QUILT_ID> --tag category docs --out <DIR>

# Read specific patches
walrus read-quilt --quilt-patch-ids <PATCH_ID> --out <DIR>
```

### List Quilt Contents

```bash
walrus list-patches-in-quilt <QUILT_ID>
```

## Blob Attributes (Metadata)

```bash
# Set attributes
walrus set-blob-attribute <BLOB_OBJ_ID> \
  --attr "key1" "value1" \
  --attr "key2" "value2"

# Get attributes
walrus get-blob-attribute <BLOB_OBJ_ID>

# Remove specific attributes
walrus remove-blob-attribute-fields <BLOB_OBJ_ID> --keys "key1,key2"

# Remove all attributes
walrus remove-blob-attribute <BLOB_OBJ_ID>
```

## System Information

```bash
# Show network info
walrus info
walrus info all

# Check node health
walrus health --committee
```

## Utilities

```bash
# Convert blob ID format
walrus convert-blob-id <BLOB_ID_DECIMAL>

# Burn blob objects
walrus burn-blobs --object-ids <ID1,ID2>
walrus burn-blobs --all
walrus burn-blobs --all-expired
```

## Configuration Options

```bash
# Use custom config
walrus --config <PATH> <COMMAND>

# Use custom wallet
walrus --wallet <WALLET_PATH> <COMMAND>

# Set gas budget (in MIST)
walrus --gas-budget <AMOUNT> <COMMAND>
```

## Environment Variables

```bash
# Enable debug logging
RUST_LOG=walrus=debug walrus info

# Enable trace logging
RUST_LOG=walrus=trace walrus info
```

## Common Workflows

### Workflow 1: Upload and Share

```bash
# 1. Upload file
walrus store document.pdf --epochs 20

# 2. Note blob ID from output
# 3. Share blob ID with others
# 4. Others can read:
walrus read <BLOB_ID> --out document.pdf
```

### Workflow 2: Temporary Storage

```bash
# 1. Upload as deletable
walrus store temp-file.txt --epochs 5 --deletable

# 2. Use the file
walrus read <BLOB_ID>

# 3. Delete when done
walrus delete --blob-id <BLOB_ID>
```

### Workflow 3: Website Upload

```bash
# 1. Create website directory
mkdir my-site
echo "<html>...</html>" > my-site/index.html

# 2. Upload as quilt
walrus store-quilt --epochs 50 --paths my-site

# 3. Access files
walrus read-quilt --quilt-id <QUILT_ID> --out downloaded-site
```

### Workflow 4: Add Metadata

```bash
# 1. Upload file
walrus store research.pdf --epochs 15

# 2. Add metadata
walrus set-blob-attribute <BLOB_OBJ_ID> \
  --attr "title" "Research Paper" \
  --attr "author" "John Doe"

# 3. Retrieve metadata
walrus get-blob-attribute <BLOB_OBJ_ID>
```

## Sui Wallet Commands

```bash
# Check active address
sui client active-address

# Check balance
sui client gas

# Get testnet tokens
sui client faucet

# Switch network
sui client switch --env testnet
sui client switch --env mainnet

# List networks
sui client envs
```

## Error Troubleshooting

### Insufficient Gas

```bash
# Check balance
sui client gas

# Get testnet SUI
sui client faucet
```

### Blob Not Found

```bash
# Check status
walrus blob-status --blob-id <BLOB_ID>

# Check if expired
walrus list-blobs --include-expired
```

### Connection Issues

```bash
# Check node health
walrus health --committee

# Check network info
walrus info
```

## HTTP API Quick Reference

### Upload via HTTP

```bash
curl -X PUT \
  "https://publisher.walrus.xyz/v1/store?epochs=10" \
  --data-binary @file.txt
```

### Download via HTTP

```bash
curl "https://aggregator.walrus.xyz/v1/<BLOB_ID>" -o output.txt
```

### Check Status via HTTP

```bash
curl "https://aggregator.walrus.xyz/v1/<BLOB_ID>/info"
```

## Cost Calculation

```bash
# Get pricing info
walrus info

# Storage cost formula:
# Cost = file_size × storage_coefficient × epochs × price_per_unit

# Example for 10 MB, 10 epochs:
# Cost ≈ 10 MB × 5 (erasure coding) × 10 epochs × price
```

## Tips & Best Practices

1. **Always save blob IDs** - You'll need them to retrieve data
2. **Use --deletable for temporary data** - Save costs
3. **Use quilts for related files** - More efficient
4. **Encrypt sensitive data** - All blobs are public
5. **Monitor expiration** - Set reminders to extend important blobs
6. **Use shared blobs for community data** - Others can help fund
7. **Add metadata** - Easier to organize and search
8. **Check blob status** - Ensure availability before depending on it
9. **Use testnet first** - Practice before mainnet
10. **Keep wallet secure** - Back up recovery phrases

## Quick Examples

```bash
# Upload image
walrus store photo.jpg --epochs 100

# Upload and get just blob ID
walrus store file.txt --epochs 10 | grep "Blob ID"

# Download and view text file
walrus read <BLOB_ID> | less

# Check if blob exists
walrus blob-status --blob-id <BLOB_ID> && echo "Exists" || echo "Not found"

# Upload directory
walrus store-quilt --epochs 20 --paths ./my-project

# Extend expiring blob
walrus list-blobs | grep "expiring soon"
walrus extend --blob-obj-id <BLOB_OBJ_ID> --epochs 10
```

## URLs

- **Mainnet Publisher**: https://publisher.walrus.xyz
- **Mainnet Aggregator**: https://aggregator.walrus.xyz
- **Testnet Publisher**: https://publisher-testnet.walrus.xyz
- **Testnet Aggregator**: https://aggregator-testnet.walrus.xyz
- **Documentation**: https://docs.wal.app
- **Website**: https://www.walrus.xyz

---

**Print this for quick reference during development!**
