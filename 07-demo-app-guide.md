# Walrus Demo App - Teaching Guide

This guide explains the `walrus-demo-app` - a complete Next.js application demonstrating Walrus storage on Mainnet.

## What This Demo Shows

✅ **Upload** - Upload files to Walrus via HTTP API
✅ **Extend** - Extend blob lifetime using Walrus SDK
✅ **Delete** - Delete blobs using Walrus SDK
✅ **Wallet Integration** - Connect Sui wallet for signing transactions
✅ **Real Mainnet Usage** - Works with actual Sui and Walrus Mainnet

## For Instructors

### When to Use This Demo

**Best for:**
- Advanced students who completed CLI basics
- Hour 2-3 of the 4-hour course
- Students building web applications
- Demonstrating production integration

**Learning Objectives:**
- Understand HTTP API vs SDK approaches
- Learn wallet integration with @mysten/dapp-kit
- See transaction signing in practice
- Build complete full-stack app

### Demo Walkthrough (15-20 min)

#### 1. Setup (5 min)

```bash
cd walrus-demo-app
npm install
npm run dev
```

Open http://localhost:3000

#### 2. Code Walkthrough (10 min)

**Show students:**

**Upload Flow** (`app/api/upload/route.ts`):
```typescript
// HTTP API approach - no wallet needed for upload
const url = `${publisherUrl}/v1/blobs?epochs=${epochs}`;
url += `&send_object_to=${walletAddress}`;  // ← KEY: YOU own the blob

const response = await fetch(url, {
  method: 'PUT',
  body: fileData
});
```

**Extend Flow** (`lib/walrus.ts` → `app/page.tsx`):
```typescript
// 1. Server: Build unsigned transaction
const tx = await client.extendBlobTransaction({
  blobObjectId,
  epochs: 10
});

// 2. Client: Sign with wallet
const result = await signTransaction({
  transaction: tx.bytes
});
```

**Delete Flow** (similar to extend):
```typescript
const tx = await client.deleteBlobTransaction({
  blobObjectId,
  owner: walletAddress
});
```

#### 3. Live Demo (5 min)

1. **Upload**: Show file upload with deletable flag
2. **Extend**: Click extend, show wallet popup
3. **Delete**: Delete the blob, show it's gone
4. **Check**: Visit Sui Explorer to see transactions

### Key Teaching Points

**1. HTTP API vs SDK**
- Upload uses **HTTP API** (simple, no wallet)
- Extend/Delete use **SDK** (requires wallet signatures)

**2. Blob Ownership**
- `send_object_to` parameter is CRITICAL
- Without it, YOU don't own the blob object
- Can't extend or delete blobs you don't own

**3. Transaction Signing**
- Extend/delete are Sui transactions
- User signs with their wallet
- Costs real SUI on mainnet

**4. Deletable Flag**
- Set during upload, can't change
- Only deletable blobs can be deleted
- All blobs expire naturally

### Common Student Questions

**Q: Why do I need a wallet for extend but not upload?**
A: Upload is an HTTP request to Walrus publisher. Extend/delete are Sui blockchain transactions that modify on-chain objects.

**Q: Can I delete any blob?**
A: No, only if: (1) You own the blob object, (2) Blob is deletable, (3) Blob hasn't expired

**Q: Why use `send_object_to`?**
A: It transfers blob ownership to your wallet. Without it, the server wallet owns the blob.

**Q: What happens if blob expires?**
A: Data is removed from storage nodes. Can't recover. Must upload again.

### Troubleshooting

**"npm install fails"**
- Check Node.js version (18+)
- Try `npm install --legacy-peer-deps`

**"Wallet won't connect"**
- Check wallet extension installed
- Ensure wallet is on Mainnet
- Try different wallet (Sui Wallet, Ethos)

**"Upload works but extend fails"**
- Verify student owns the blob object
- Check they have SUI for gas
- Look at browser console errors

---

## For Students

### Prerequisites

Before using this demo:
- ✅ Completed CLI exercises (modules 1-3)
- ✅ Have Sui wallet installed
- ✅ Have 0.1+ SUI on Mainnet (for testing)
- ✅ Basic React/Next.js knowledge

### How to Use

**See: `walrus-demo-app/QUICK-START.md`**

Key steps:
1. `npm install`
2. `npm run dev`
3. Connect wallet
4. Upload file
5. Try extend/delete

### What You'll Learn

**HTTP API Uploads:**
```typescript
// Simple PUT request
fetch('https://publisher.walrus.space/v1/blobs?epochs=5', {
  method: 'PUT',
  body: fileData
})
```

**SDK Transactions:**
```typescript
// Build transaction
const tx = await walrusClient.extendBlobTransaction({...});

// Sign with wallet
await signTransaction({ transaction: tx });
```

**Wallet Integration:**
```typescript
// Connect wallet
import { ConnectButton, useCurrentAccount } from '@mysten/dapp-kit';

// Use in component
const account = useCurrentAccount();
```

### Code Study Guide

**Start here:**
1. `app/page.tsx` - UI and user interactions
2. `app/api/upload/route.ts` - HTTP upload
3. `lib/walrus.ts` - Walrus SDK wrapper
4. `app/providers.tsx` - Wallet setup

**Try modifying:**
- Change default epochs (line 258 in page.tsx)
- Add file size limit
- Show upload progress
- Add retry logic
- Support multiple files

### Exercises

**Beginner:**
1. Upload a text file
2. Extend it by 5 epochs
3. Delete it
4. Check transaction on Sui Explorer

**Intermediate:**
5. Add upload progress indicator
6. Show current epoch from blockchain
7. Calculate storage cost before upload
8. List all user's blobs

**Advanced:**
9. Implement batch upload (multiple files)
10. Add file encryption before upload
11. Create shareable links with access control
12. Build gallery view of uploaded images

---

## Architecture Overview

```
┌─────────────────────────────────────────┐
│         User (Browser)                  │
│  - Connect Sui Wallet                   │
│  - Select Files                         │
│  - Sign Transactions                    │
└────────────┬────────────────────────────┘
             │
             ▼
┌─────────────────────────────────────────┐
│      Next.js App (Frontend)             │
│  - app/page.tsx (UI)                    │
│  - app/providers.tsx (Wallet)           │
│  - @mysten/dapp-kit                     │
└────────────┬────────────────────────────┘
             │
             ▼
┌─────────────────────────────────────────┐
│     API Routes (Backend)                │
│  - /api/upload (HTTP upload)            │
│  - /api/extend (build tx)               │
│  - /api/delete (build tx)               │
└────┬───────────────────────────┬────────┘
     │                           │
     ▼                           ▼
┌──────────────┐          ┌─────────────┐
│ Walrus HTTP  │          │ Walrus SDK  │
│ Publisher    │          │ (@mysten)   │
└──────┬───────┘          └──────┬──────┘
       │                         │
       ▼                         ▼
┌──────────────────────────────────────┐
│    Walrus Storage Network            │
│  - Storage Nodes                     │
│  - Erasure Coding                    │
│  - Blob Distribution                 │
└──────────────────────────────────────┘
       ▲                         │
       │                         │
       └─────────┬───────────────┘
                 │
                 ▼
┌──────────────────────────────────────┐
│       Sui Blockchain                 │
│  - Blob Objects                      │
│  - Ownership Records                 │
│  - Expiry Tracking                   │
└──────────────────────────────────────┘
```

---

## Integration Points

### 1. Upload Flow

```
User selects file
    ↓
app/page.tsx FormData
    ↓
POST /api/upload
    ↓
HTTP PUT to Walrus Publisher
    ↓
Walrus stores blob + creates Sui object
    ↓
Return blob ID & object ID
    ↓
Display in UI
```

### 2. Extend Flow

```
User clicks extend
    ↓
POST /api/extend (blob object ID)
    ↓
Walrus SDK builds unsigned transaction
    ↓
Return serialized transaction
    ↓
User signs with wallet
    ↓
Transaction executed on Sui
    ↓
Blob expiry updated
```

### 3. Delete Flow

```
User clicks delete
    ↓
POST /api/delete (blob object ID + owner)
    ↓
Walrus SDK builds unsigned transaction
    ↓
Return serialized transaction
    ↓
User signs with wallet
    ↓
Transaction executed on Sui
    ↓
Blob object destroyed
    ↓
Storage nodes remove data
```

---

## Customization Ideas

### Easy Modifications

1. **Change Network**
```typescript
// In lib/walrus.ts, line 11
walrus({ network: "testnet" })  // Switch to testnet
```

2. **Adjust Default Epochs**
```typescript
// In app/page.tsx, line 258
defaultValue={10}  // Change from 5 to 10
```

3. **Add File Size Limit**
```typescript
// In app/page.tsx, handleUpload
if (file.size > 10 * 1024 * 1024) {
  setError("File too large (max 10 MB)");
  return;
}
```

### Advanced Modifications

4. **Upload Progress**
```typescript
// Use XMLHttpRequest for progress tracking
const xhr = new XMLHttpRequest();
xhr.upload.addEventListener('progress', (e) => {
  const percent = (e.loaded / e.total) * 100;
  setProgress(percent);
});
```

5. **Batch Upload**
```typescript
// Upload multiple files as quilt
const blobs = files.map(f => ({
  contents: await f.arrayBuffer(),
  identifier: f.name
}));

await walrusClient.writeQuilt({ blobs, ... });
```

6. **Cost Calculator**
```typescript
// Calculate cost before upload
const cost = await walrusClient.storageCost(
  file.size,
  epochs
);
console.log(`Cost: ${cost.totalCost / 1e9} SUI`);
```

---

## Deployment

### Vercel (Recommended)

```bash
npm install -g vercel
vercel
```

### Environment Variables

No secrets needed! All operations use:
- User's wallet for signing
- Public Walrus/Sui endpoints

### Build for Production

```bash
npm run build
npm run start
```

---

## Resources

**Demo App Files:**
- `walrus-demo-app/README.md` - Full documentation
- `walrus-demo-app/QUICK-START.md` - Quick setup
- `walrus-demo-app/app/page.tsx` - Main UI code
- `walrus-demo-app/lib/walrus.ts` - SDK integration

**External Resources:**
- Walrus Docs: https://docs.wal.app
- Sui dApp Kit: https://sdk.mystenlabs.com/dapp-kit
- Walrus SDK: https://www.npmjs.com/package/@mysten/walrus

---

**This demo is production-ready and can be deployed immediately!** 🚀
