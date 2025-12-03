# Walrus Storage - Student Handout

**Print this page and keep it at your desk during class!**

---

## 🚀 Quick Setup

```bash
# Install Walrus CLI
curl -sSf https://install.wal.app | sh -s -- -n testnet
export PATH="$HOME/.local/bin:$PATH"

# Install Sui & Create Wallet
brew install sui  # or use cargo
sui client new-address ed25519

# Get Testnet SUI
sui client faucet
```

---

## 📋 Essential Commands

### Upload
```bash
walrus store <FILE> --epochs <NUM>
walrus store file.txt --epochs 10 --deletable
```

### Download
```bash
walrus read <BLOB_ID>
walrus read <BLOB_ID> --out output.txt
```

### Manage
```bash
walrus list-blobs
walrus blob-status --blob-id <BLOB_ID>
walrus delete --blob-id <BLOB_ID>
```

### Quilts (Multiple Files)
```bash
walrus store-quilt --epochs 20 --paths <DIRECTORY>
walrus read-quilt --quilt-id <QUILT_ID> --out <DIR>
```

### Metadata
```bash
walrus set-blob-attribute <BLOB_OBJ_ID> --attr "key" "value"
walrus get-blob-attribute <BLOB_OBJ_ID>
```

---

## 🔑 Key Concepts

| Term | Meaning |
|------|---------|
| **Blob** | Your file stored on Walrus |
| **Blob ID** | Unique identifier (like a URL) |
| **Epoch** | Time period for storage |
| **Deletable** | Can be deleted before expiration |
| **Quilt** | Multiple files stored together |
| **SUI** | Token for payments |

---

## ⚠️ Important Notes

- ❗ **All blobs are PUBLIC** - anyone with blob ID can download
- 🔐 **Encrypt sensitive data** before uploading
- 💾 **Save your blob IDs** - you need them to retrieve data
- ⏰ **Blobs expire** - set enough epochs or extend later
- 🆓 **Testnet is free** - practice without cost

---

## 🌐 HTTP API (for Web Apps)

### Upload
```javascript
const response = await fetch(
  'https://publisher-testnet.walrus.xyz/v1/store?epochs=10',
  {
    method: 'PUT',
    body: fileArrayBuffer
  }
);
const result = await response.json();
// result.blobId
```

### Download
```
https://aggregator-testnet.walrus.xyz/v1/<BLOB_ID>
```

---

## 🆘 Troubleshooting

**"Command not found"**
```bash
export PATH="$HOME/.local/bin:$PATH"
```

**"Insufficient gas"**
```bash
sui client faucet
```

**"Blob not found"**
```bash
walrus blob-status --blob-id <BLOB_ID>
```

---

## 📚 Resources

- **Docs**: https://docs.wal.app
- **Discord**: https://discord.gg/sui
- **GitHub**: https://github.com/MystenLabs/walrus

---

## ✏️ Notes Section

**My Blob IDs:**

1. ________________________________________

2. ________________________________________

3. ________________________________________

**My Wallet Address:**

________________________________________

**Important URLs:**

Publisher: https://publisher-testnet.walrus.xyz

Aggregator: https://aggregator-testnet.walrus.xyz

**Today's Project:**

________________________________________

________________________________________

________________________________________

---

**🎯 Today's Goals:**
- ☐ Install Walrus CLI
- ☐ Upload a text file
- ☐ Download a peer's blob
- ☐ Upload an image
- ☐ Create a quilt
- ☐ Complete the project
- ☐ Understand key concepts

---

*Walrus Storage on Sui Blockchain*
*Decentralized • Affordable • Reliable*
