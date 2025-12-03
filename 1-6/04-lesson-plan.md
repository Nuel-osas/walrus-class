# Walrus Storage: Complete Lesson Plan

This comprehensive lesson plan provides a structured approach to teaching Walrus storage for the Sui blockchain.

## Course Overview

**Duration**: 4-6 hours (can be split into multiple sessions)
**Level**: Intermediate (basic blockchain knowledge helpful)
**Prerequisites**:
- Basic command-line familiarity
- Understanding of blockchain concepts
- Optional: Programming experience (JavaScript/TypeScript/Python)

**Learning Objectives:**
By the end of this course, students will be able to:
1. Explain what Walrus is and how it works
2. Install and configure Walrus CLI
3. Upload and download blobs using various methods
4. Integrate Walrus into applications
5. Implement advanced features (encryption, quilts, metadata)
6. Understand cost optimization and best practices

---

## Session 1: Introduction to Walrus (60 minutes)

### Part 1: What is Walrus? (20 minutes)

**Topics:**
- The problem: Decentralized storage challenges
- How Walrus solves it: Erasure coding vs replication
- Walrus architecture and components
- Use cases and real-world applications

**Teaching Method:**
- Presentation with diagrams
- Compare with traditional storage (AWS S3, IPFS, Arweave)
- Show live Walrus website examples

**Discussion Questions:**
1. Why is decentralized storage important for Web3?
2. How does erasure coding reduce costs?
3. What types of applications benefit from Walrus?

**Materials:**
- Refer to: `01-walrus-overview.md`
- Show architecture diagram
- Demo: Visit https://www.walrus.xyz

### Part 2: Key Concepts (20 minutes)

**Topics:**
- Blobs and Blob IDs
- Storage epochs
- Deletable vs permanent blobs
- Shared blobs
- Quilts for multiple files
- Public nature of data (encryption importance)

**Teaching Method:**
- Whiteboard/slides for concepts
- Real examples of blob IDs
- Cost calculations

**Activity:**
- Calculate storage costs for different file sizes
- Discuss scenarios: When to use deletable vs permanent?

### Part 3: The Sui Connection (20 minutes)

**Topics:**
- How Walrus uses Sui blockchain
- Storage space as Sui resources
- SUI tokens for payment
- Smart contract integration potential

**Teaching Method:**
- Show Sui explorer
- Demonstrate blob objects on-chain
- Explain transaction flow

**Demo:**
- Visit Sui explorer: https://suiexplorer.com
- Show a Walrus storage transaction

---

## Session 2: Hands-On Setup (60 minutes)

### Part 1: Environment Setup (30 minutes)

**Activities:**
1. Install Walrus CLI
2. Install Sui CLI
3. Create Sui wallet
4. Get testnet SUI tokens
5. Verify installation

**Step-by-Step:**
Follow `02-getting-started-guide.md` Section by Section

**Common Issues & Solutions:**
- PATH not set → Add to shell profile
- Wallet creation fails → Check Sui CLI version
- Faucet timeout → Try Discord faucet
- Binary permissions → Use `chmod +x`

**Checkpoint:**
Everyone should successfully run:
```bash
walrus --version
sui client active-address
sui client gas
```

### Part 2: First Blob Upload (30 minutes)

**Guided Exercise:**

1. **Create test file:**
   ```bash
   echo "Hello from [Your Name]!" > hello.txt
   ```

2. **Upload to Walrus:**
   ```bash
   walrus store hello.txt --epochs 5
   ```

3. **Save blob ID** (everyone shares theirs)

4. **Download peer's blob:**
   ```bash
   walrus read <PEER_BLOB_ID> --out peer-hello.txt
   cat peer-hello.txt
   ```

5. **Check status:**
   ```bash
   walrus blob-status --blob-id <YOUR_BLOB_ID>
   walrus list-blobs
   ```

**Learning Outcomes:**
- Understand upload/download flow
- Experience decentralization (reading peer data)
- Interpret blob status output

---

## Session 3: CLI Deep Dive (60 minutes)

### Part 1: Storage Options (20 minutes)

**Topics:**
- Epoch-based storage
- Deletable blobs
- Shared blobs
- Upload relays

**Hands-On Exercises:**

**Exercise 1: Deletable Blob**
```bash
echo "Temporary data" > temp.txt
walrus store temp.txt --epochs 3 --deletable
# Note the blob ID
walrus delete --blob-id <BLOB_ID>
walrus blob-status --blob-id <BLOB_ID>  # Should show deleted
```

**Exercise 2: Shared Blob**
```bash
echo "Community data" > community.txt
walrus store community.txt --epochs 10 --share --amount 1000000000
# Share object ID with peers for funding
```

**Exercise 3: Duration Options**
```bash
walrus store important.txt --epochs max
walrus store scheduled.txt --earliest-expiry-time "2025-12-31T23:59:59Z"
```

### Part 2: Quilts - Multiple Files (20 minutes)

**Concept:**
Quilts allow storing multiple related files together efficiently.

**Exercise: Create a Simple Website**
```bash
mkdir simple-site
cat > simple-site/index.html << 'EOF'
<!DOCTYPE html>
<html>
<head><title>My Site</title><link rel="stylesheet" href="style.css"></head>
<body><h1>Hello Walrus!</h1></body>
</html>
EOF

cat > simple-site/style.css << 'EOF'
body { background: #f0f0f0; font-family: Arial; }
h1 { color: #0066cc; }
EOF

# Store as quilt
walrus store-quilt --epochs 20 --paths simple-site

# Download quilt
walrus read-quilt --quilt-id <QUILT_ID> --out ./downloaded-site
```

### Part 3: Blob Metadata (20 minutes)

**Exercise: Add Attributes**
```bash
# Upload a file
walrus store research-paper.pdf --epochs 15

# Add metadata
walrus set-blob-attribute <BLOB_OBJ_ID> \
  --attr "title" "Blockchain Storage Research" \
  --attr "author" "Student Name" \
  --attr "date" "2025-12-03" \
  --attr "category" "research"

# Retrieve metadata
walrus get-blob-attribute <BLOB_OBJ_ID>

# Search exercise: Have students categorize blobs and retrieve by category
```

---

## Session 4: Programming Integration (90 minutes)

### Part 1: TypeScript/JavaScript (45 minutes)

**Setup:**
```bash
mkdir walrus-app
cd walrus-app
npm init -y
npm install @mysten/walrus-sdk @mysten/sui.js
```

**Exercise 1: Basic Upload Script**
Create `upload.ts` from examples in `03-code-examples.md`

**Exercise 2: Download Script**
Create `download.ts`

**Exercise 3: Batch Operations**
Upload multiple files from a directory

**Project Challenge:**
Build a simple CLI tool that:
- Takes a directory path
- Uploads all files
- Saves a manifest with blob IDs
- Can restore directory from manifest

### Part 2: Python Integration (45 minutes)

**Setup:**
```bash
python -m venv venv
source venv/bin/activate  # or venv\Scripts\activate on Windows
pip install pysui walrus-python
```

**Exercise 1: Upload Script**
Use examples from `03-code-examples.md`

**Exercise 2: Data Pipeline**
Build a script that:
1. Processes CSV data
2. Uploads to Walrus
3. Stores blob ID in metadata database
4. Can retrieve and process later

**Project Challenge:**
Create a backup script:
- Watches a directory for changes
- Automatically uploads new/modified files
- Maintains local manifest
- Can restore from manifest

---

## Session 5: Web Applications (60 minutes)

### Part 1: HTTP API (20 minutes)

**Concept:**
Walrus provides HTTP API for web integration without needing SDKs.

**Exercise: Simple Upload Form**
Use HTML example from `03-code-examples.md`

**Demo:**
1. Open HTML file in browser
2. Upload an image
3. See blob ID and URL
4. Share URL with peers

### Part 2: Image Gallery Project (40 minutes)

**Group Project:**
Build a decentralized image gallery:

**Requirements:**
- Upload multiple images
- Display gallery
- Persist blob IDs in localStorage
- Add captions/metadata
- Share gallery with blob ID list

**Bonus Features:**
- Image preview before upload
- Upload progress bar
- Download images
- Delete images (if deletable)

**Code Starter:**
Use `03-code-examples.md` Example 2

---

## Session 6: Advanced Topics (60 minutes)

### Part 1: Security & Encryption (20 minutes)

**Important Concept:**
All Walrus blobs are public!

**Exercise: Encrypt Before Upload**
```typescript
// Use encryption example from code examples
// Encrypt sensitive.txt
// Upload encrypted version
// Download and decrypt
// Verify contents match
```

**Discussion:**
- When to encrypt?
- Key management strategies
- Walrus with Seal (future topic)

### Part 2: Cost Optimization (20 minutes)

**Topics:**
- Choosing appropriate epochs
- Shared blobs for community data
- Quilts vs individual blobs
- Extending vs re-uploading

**Exercise: Cost Calculation**
```bash
walrus info  # Get pricing

# Calculate costs for:
# - 100 MB for 10 epochs
# - 1 GB for max epochs
# - 10 files × 10 MB vs 1 quilt
```

**Activity:**
Design storage strategy for different scenarios:
1. NFT marketplace
2. Social media app
3. Document management
4. Video streaming platform

### Part 3: Best Practices (20 minutes)

**Checklist:**
- ✅ Encrypt sensitive data
- ✅ Use shared blobs for public data
- ✅ Set appropriate expiration
- ✅ Monitor blob status regularly
- ✅ Keep blob IDs secure
- ✅ Use quilts for related files
- ✅ Add metadata for organization
- ✅ Implement retry logic in applications
- ✅ Handle errors gracefully
- ✅ Document blob purposes

**Discussion:**
- How to handle blob expiration in production?
- Backup strategies
- Migration from other storage

---

## Final Project (60 minutes)

**Options (choose one):**

### Option 1: Decentralized File Sharing App
Build a web app where users can:
- Upload files to Walrus
- Generate shareable links
- Download via blob ID
- Optional: Add password protection

### Option 2: NFT Metadata Storage
Create a system that:
- Stores NFT images and metadata on Walrus
- Integrates with Sui NFT contract
- Displays NFT gallery
- Proves data availability

### Option 3: Document Management System
Build a CLI tool that:
- Organizes documents by category
- Uses blob attributes for metadata
- Supports search by attributes
- Implements version control
- Can backup/restore entire system

### Option 4: Decentralized Blog
Create a blog platform where:
- Posts stored as quilts (HTML + assets)
- Index stored on Walrus
- Commenters can fund post storage
- Works entirely on Walrus + Sui

**Evaluation Criteria:**
- ✅ Correctly uploads/downloads blobs
- ✅ Handles errors appropriately
- ✅ Uses appropriate Walrus features
- ✅ Follows best practices
- ✅ Documentation and code quality
- ✅ Creativity and user experience

---

## Assessment & Quizzes

### Quiz 1: Concepts (After Session 1)

1. What technology does Walrus use to reduce storage costs?
2. True/False: Blobs on Walrus are private by default
3. What blockchain does Walrus use for coordination?
4. Explain the difference between deletable and permanent blobs
5. What is a quilt?

### Quiz 2: Practical Skills (After Session 3)

1. Write the command to store a file for 15 epochs as deletable
2. How do you check the status of a blob?
3. What command lists all your active blobs?
4. How do you add metadata to a blob?
5. Write the command to download a blob to a specific file

### Quiz 3: Integration (After Session 5)

1. What are the three ways to interact with Walrus?
2. How do you upload a file using the HTTP API?
3. Why should you encrypt sensitive data before uploading?
4. How can you display an image stored on Walrus in HTML?
5. What's the advantage of using quilts over individual blobs?

### Final Assessment

**Practical Exam:**
1. Set up Walrus CLI from scratch (15 min)
2. Upload 3 different file types (10 min)
3. Create a quilt with 3 files (10 min)
4. Build a simple upload/download script (30 min)
5. Explain your design choices (15 min)

---

## Additional Resources

### Documentation
- Official Docs: https://docs.wal.app
- Sui Docs: https://docs.sui.io
- GitHub: https://github.com/MystenLabs/walrus

### Community
- Discord: https://discord.gg/sui
- Twitter: @Walrus_xyz
- Forum: https://forums.sui.io

### Code Examples
- Official Examples: https://github.com/MystenLabs/walrus-docs
- Community Guide: https://github.com/Angleito/Walrus-Code-Guide
- Tutorials: https://suibyexamples.com

### Tools
- Sui Explorer: https://suiexplorer.com
- Walrus Sites: https://docs.walrus.site
- CLI Cheat Sheet: See resources folder

---

## Teaching Tips

### For Instructors

**Preparation:**
1. Test all commands beforehand
2. Have backup Sui wallets with testnet SUI
3. Prepare example files of various types
4. Set up demo projects in advance
5. Have troubleshooting guide ready

**Pacing:**
- Allow extra time for setup (installations can be slow)
- Build in breaks between sessions
- Have backup exercises for fast learners
- Prepare simplified versions for struggling students

**Engagement:**
- Use peer-to-peer blob sharing exercises
- Create competitions (fastest upload, largest file, etc.)
- Show real-world Walrus applications
- Invite students to share their projects

**Common Student Challenges:**
1. **PATH issues** → Provide clear shell-specific instructions
2. **Wallet confusion** → Draw diagrams showing key concepts
3. **Cost anxiety** → Emphasize testnet is free
4. **Blockchain intimidation** → Start simple, add complexity gradually

**Assessment Strategy:**
- Mix theory and practical evaluation
- Focus on understanding over memorization
- Allow collaboration on projects
- Encourage creative applications

---

## Session Checklist

### Before Each Session
- [ ] Test internet connectivity
- [ ] Verify Walrus network is operational (`walrus health`)
- [ ] Prepare example files
- [ ] Load slides/materials
- [ ] Test code examples
- [ ] Have troubleshooting resources ready

### During Session
- [ ] Take attendance
- [ ] Quick recap of previous session
- [ ] Clear learning objectives stated
- [ ] Hands-on time for all students
- [ ] Address questions
- [ ] Note common issues for next session

### After Session
- [ ] Share session materials
- [ ] Post homework/practice exercises
- [ ] Update troubleshooting guide
- [ ] Collect feedback
- [ ] Prepare for next session

---

## Student Handout Checklist

Provide students with:
- [ ] `01-walrus-overview.md`
- [ ] `02-getting-started-guide.md`
- [ ] `03-code-examples.md`
- [ ] CLI command reference
- [ ] Links to all resources
- [ ] Practice exercise list
- [ ] Final project requirements
- [ ] Assessment rubric

---

**Good luck with your Walrus class! 🎓**

For questions or additional support, refer to:
- Documentation: https://docs.wal.app
- Community Discord: https://discord.gg/sui
