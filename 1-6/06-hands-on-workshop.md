# Walrus Storage: Hands-On Workshop Guide

**Duration**: 2-3 hours
**Format**: Interactive, lab-style session
**Level**: Beginner to Intermediate

This workshop provides a condensed, hands-on introduction to Walrus storage. Perfect for hackathons, meetups, or intensive training sessions.

---

## Workshop Overview

### Objectives
By the end of this workshop, participants will:
- ✅ Have Walrus CLI installed and configured
- ✅ Successfully upload and download blobs
- ✅ Understand core Walrus concepts
- ✅ Build a simple application using Walrus
- ✅ Know how to continue learning

### Materials Needed
- Computer with terminal access
- Internet connection
- Text editor or IDE
- Optional: Node.js or Python installed

---

## Part 1: Quick Setup (30 minutes)

### Instructor Demo (5 min)

**Show the end goal:**
1. Upload an image to Walrus
2. Share the blob ID
3. Download and display the image
4. Demonstrate decentralization

### Participant Setup (25 min)

**Step 1: Install Walrus CLI (10 min)**

Everyone runs:
```bash
# Testnet installation
curl -sSf https://install.wal.app | sh -s -- -n testnet

# Add to PATH
export PATH="$HOME/.local/bin:$PATH"

# Verify
walrus --version
```

**Checkpoint**: Instructor verifies everyone sees version output.

**Step 2: Set Up Sui Wallet (10 min)**

```bash
# Install Sui CLI (if needed)
# macOS
brew install sui

# Or use cargo
cargo install --locked --git https://github.com/MystenLabs/sui.git --branch mainnet sui

# Create wallet
sui client new-address ed25519

# IMPORTANT: Save recovery phrase!
```

**Step 3: Get Testnet SUI (5 min)**

```bash
# Request testnet tokens
sui client faucet

# Verify balance
sui client gas
```

**Troubleshooting Time**: Instructor helps anyone with issues.

---

## Part 2: First Uploads (30 minutes)

### Exercise 1: Text File Upload (10 min)

**Everyone follows along:**

```bash
# 1. Create a personalized file
echo "Hello Walrus! From [YOUR NAME]" > hello.txt

# 2. Upload to Walrus
walrus store hello.txt --epochs 5

# 3. SAVE YOUR BLOB ID!
# Copy the blob ID from the output
```

**Expected Output:**
```
Successfully stored blob with ID: <BLOB_ID>
Blob object ID: <OBJECT_ID>
Cost: 0.XXX SUI
Expires at epoch: XXX
```

**Activity**: Share your blob ID in chat/on screen.

### Exercise 2: Download Peer's Blob (10 min)

**Instructor shares a blob ID, everyone downloads:**

```bash
# Download instructor's blob
walrus read <INSTRUCTOR_BLOB_ID> --out instructor-message.txt

# View the content
cat instructor-message.txt
```

**Pair Activity**: Exchange blob IDs with a neighbor and download each other's files.

```bash
# Download peer's blob
walrus read <PEER_BLOB_ID> --out peer-message.txt
cat peer-message.txt
```

**Discussion**: What just happened? (Decentralization in action!)

### Exercise 3: Upload an Image (10 min)

**Download a sample image or use your own:**

```bash
# Download a sample image
curl -o sample.jpg "https://picsum.photos/800/600"

# Upload to Walrus
walrus store sample.jpg --epochs 10

# Note the blob ID
```

**Try to retrieve it:**

```bash
walrus read <YOUR_IMAGE_BLOB_ID> --out retrieved.jpg

# Open the image to verify
# macOS: open retrieved.jpg
# Linux: xdg-open retrieved.jpg
# Windows: start retrieved.jpg
```

---

## Part 3: Core Concepts (20 minutes)

### Mini-Lecture (10 min)

**Topics** (slides or whiteboard):

1. **What is Walrus?**
   - Decentralized storage on Sui blockchain
   - Uses erasure coding (not full replication)
   - ~5x storage multiplier vs full copies

2. **Key Concepts**
   - **Blobs**: Files you store
   - **Blob ID**: Unique identifier
   - **Epochs**: Time periods for storage
   - **Deletable vs Permanent**: Can you delete it?

3. **Important: Data is Public!**
   - Anyone with blob ID can download
   - Encrypt sensitive data before upload

4. **Cost Model**
   - Pay per epoch
   - Calculated by size × epochs × rate

### Quick Quiz (5 min)

**Ask participants:**
1. What technology does Walrus use? (Erasure coding)
2. Are blobs private? (No, public!)
3. What blockchain does Walrus use? (Sui)
4. How long can you store data? (Up to 53 epochs)

### Q&A (5 min)

---

## Part 4: Hands-On Features (40 minutes)

### Exercise 4: Deletable Blobs (10 min)

**Concept**: Some blobs can be deleted before expiration.

```bash
# Create temporary data
echo "This is temporary" > temp.txt

# Upload as deletable
walrus store temp.txt --epochs 5 --deletable

# Check it exists
walrus blob-status --blob-id <BLOB_ID>

# Delete it
walrus delete --blob-id <BLOB_ID>

# Verify deletion
walrus blob-status --blob-id <BLOB_ID>
```

**Use case**: Temporary files, drafts, cache

### Exercise 5: Managing Your Blobs (10 min)

**List and organize:**

```bash
# See all your blobs
walrus list-blobs

# Include expired ones
walrus list-blobs --include-expired

# Check specific blob status
walrus blob-status --blob-id <BLOB_ID>

# Get blob ID from file (without uploading)
walrus blob-id hello.txt
```

**Challenge**: How many blobs have you created? Which ones are expired?

### Exercise 6: Quilts - Multiple Files (20 min)

**Concept**: Store multiple related files together.

**Activity: Create a Mini Website**

```bash
# 1. Create project directory
mkdir my-walrus-site
cd my-walrus-site

# 2. Create HTML file
cat > index.html << 'EOF'
<!DOCTYPE html>
<html>
<head>
    <title>My Walrus Site</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>Welcome to My Walrus Site!</h1>
    <p>Hosted on decentralized storage!</p>
    <script src="script.js"></script>
</body>
</html>
EOF

# 3. Create CSS file
cat > style.css << 'EOF'
body {
    font-family: Arial, sans-serif;
    max-width: 800px;
    margin: 50px auto;
    padding: 20px;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
}
h1 { font-size: 2.5em; }
EOF

# 4. Create JavaScript file
cat > script.js << 'EOF'
console.log('Hello from Walrus storage!');
document.addEventListener('DOMContentLoaded', () => {
    console.log('Site loaded successfully from decentralized storage!');
});
EOF

# 5. Upload as quilt
cd ..
walrus store-quilt --epochs 20 --paths my-walrus-site

# 6. SAVE YOUR QUILT ID!
```

**Download and verify:**

```bash
# Download the quilt
walrus read-quilt --quilt-id <QUILT_ID> --out downloaded-site

# Check the files
ls downloaded-site/
```

**Challenge**: Open `downloaded-site/index.html` in a browser!

---

## Part 5: Build Something! (40 minutes)

### Project Options (Choose One)

**Option A: Simple File Uploader (Easy)**

Create a bash script that:
1. Takes a directory path as input
2. Uploads all files
3. Saves blob IDs to a manifest file
4. Can restore directory from manifest

**Starter Code:**

```bash
#!/bin/bash
# upload-directory.sh

DIRECTORY=$1
MANIFEST="manifest.txt"

# Upload each file
for file in "$DIRECTORY"/*; do
    echo "Uploading $file..."
    blob_id=$(walrus store "$file" --epochs 10 | grep "Blob ID" | awk '{print $NF}')
    echo "$(basename $file):$blob_id" >> $MANIFEST
done

echo "Manifest saved to $MANIFEST"
```

**Option B: Web Image Upload (Intermediate)**

Create an HTML page to upload images to Walrus.

**Starter Code:**

```html
<!DOCTYPE html>
<html>
<head>
    <title>Walrus Image Uploader</title>
    <style>
        body { font-family: Arial; max-width: 600px; margin: 50px auto; }
        #preview { max-width: 400px; margin: 20px 0; }
    </style>
</head>
<body>
    <h1>Upload Image to Walrus</h1>
    <input type="file" id="fileInput" accept="image/*">
    <button onclick="upload()">Upload</button>
    <div id="status"></div>
    <img id="preview" style="display:none;">

    <script>
        const PUBLISHER = 'https://publisher-testnet.walrus.xyz';
        const AGGREGATOR = 'https://aggregator-testnet.walrus.xyz';

        async function upload() {
            const file = document.getElementById('fileInput').files[0];
            if (!file) return alert('Select a file!');

            const status = document.getElementById('status');
            status.textContent = 'Uploading...';

            try {
                const arrayBuffer = await file.arrayBuffer();
                const response = await fetch(`${PUBLISHER}/v1/store?epochs=10`, {
                    method: 'PUT',
                    body: arrayBuffer
                });

                const result = await response.json();
                status.innerHTML = `
                    <p>✅ Upload successful!</p>
                    <p>Blob ID: ${result.blobId}</p>
                    <p><a href="${AGGREGATOR}/v1/${result.blobId}" target="_blank">View Image</a></p>
                `;

                // Show preview
                const preview = document.getElementById('preview');
                preview.src = `${AGGREGATOR}/v1/${result.blobId}`;
                preview.style.display = 'block';

            } catch (error) {
                status.textContent = `❌ Error: ${error.message}`;
            }
        }
    </script>
</body>
</html>
```

**Option C: TypeScript CLI Tool (Advanced)**

Create a TypeScript tool to backup and restore directories.

**Requirements:**
1. Install dependencies: `npm install @mysten/walrus-sdk`
2. Upload all files in a directory
3. Generate manifest with metadata
4. Restore directory from manifest
5. Handle errors gracefully

### Work Time (30 min)

Participants work on their chosen project. Instructor circulates to help.

### Show & Tell (10 min)

Volunteers demonstrate their projects.

---

## Part 6: Next Steps & Resources (20 minutes)

### What We Covered

✅ Installed and configured Walrus
✅ Uploaded and downloaded blobs
✅ Used deletable blobs
✅ Created quilts
✅ Built a project

### What We Didn't Cover (But You Should Explore!)

1. **Blob Attributes**: Add metadata to organize blobs
2. **Shared Blobs**: Multiple people can fund storage
3. **Encryption**: Protect sensitive data
4. **Sui Integration**: Use Walrus in smart contracts
5. **Walrus Sites**: Host entire websites
6. **Production Deployments**: Mainnet usage

### Learning Resources

**Documentation:**
- Official Docs: https://docs.wal.app
- Sui Docs: https://docs.sui.io

**Community:**
- Discord: https://discord.gg/sui (#walrus channel)
- Twitter: @Walrus_xyz
- GitHub: https://github.com/MystenLabs/walrus

**Examples:**
- Official Examples: https://github.com/MystenLabs/walrus-docs
- Community Guide: https://github.com/Angleito/Walrus-Code-Guide

**From This Workshop:**
- Use the materials in this folder
- `01-walrus-overview.md` - Comprehensive concepts
- `02-getting-started-guide.md` - Detailed setup
- `03-code-examples.md` - More code examples
- `05-cli-cheat-sheet.md` - Quick reference

### Challenges to Try at Home

1. **Encrypt a file** before uploading
2. **Build a photo gallery** that stores images on Walrus
3. **Create a blog** with posts stored as quilts
4. **Implement version control** for documents
5. **Build an NFT project** using Walrus for metadata

### Feedback (5 min)

**Quick Survey:**
1. What was most valuable?
2. What was confusing?
3. What do you want to learn next?
4. Would you use Walrus in a project?

---

## Workshop Checklist

### Before Workshop

**Instructor Prep:**
- [ ] Test all commands on your machine
- [ ] Prepare sample files (text, images)
- [ ] Have backup wallets with testnet SUI
- [ ] Test network connectivity
- [ ] Prepare slides/materials
- [ ] Set up screen sharing/projector
- [ ] Create shared document for blob IDs

**Participant Prep (Send Ahead):**
- [ ] Have computer with terminal access
- [ ] Install brew/cargo (for Sui CLI)
- [ ] Download sample files (optional)
- [ ] Join Discord server (optional)

### During Workshop

**Opening (0-5 min):**
- [ ] Welcome participants
- [ ] Explain workshop format
- [ ] Share schedule
- [ ] Set expectations

**Each Exercise:**
- [ ] Demo first
- [ ] Let participants try
- [ ] Circulate to help
- [ ] Verify checkpoints
- [ ] Address common issues

**Breaks:**
- [ ] 5-10 min break after Part 3
- [ ] Encourage networking

**Closing:**
- [ ] Recap key learnings
- [ ] Share resources
- [ ] Collect feedback
- [ ] Thank participants

### After Workshop

- [ ] Share materials (GitHub/email)
- [ ] Send resource links
- [ ] Share recordings (if applicable)
- [ ] Follow up on Discord
- [ ] Note improvements for next time

---

## Troubleshooting Guide for Instructors

### Common Issues

**Issue**: "walrus: command not found"
**Quick Fix**:
```bash
export PATH="$HOME/.local/bin:$PATH"
source ~/.bashrc
```

**Issue**: "Insufficient gas"
**Quick Fix**:
```bash
sui client faucet
# Wait 30 seconds
sui client gas
```

**Issue**: Installation fails on M1/M2 Mac
**Quick Fix**: Use `macos-arm64` binary explicitly

**Issue**: Faucet not working
**Quick Fix**: Use Discord faucet or instructor's backup wallet

**Issue**: Slow network/timeouts
**Quick Fix**:
```bash
walrus health --committee  # Check nodes
# Try upload relay: --upload-relay https://relay.walrus.xyz
```

### Emergency Backup Plan

If live demo fails:
1. Have pre-recorded video backup
2. Use prepared blob IDs
3. Show screenshots
4. Continue with theory while debugging

### Time Management

**Running ahead?**
- Add bonus exercises
- Do live coding demo
- Extended Q&A
- Help with advanced features

**Running behind?**
- Skip mini-lecture details
- Reduce project time
- Skip show & tell
- Share materials for home practice

---

## Sample Timeline

| Time | Activity | Duration |
|------|----------|----------|
| 0:00 | Welcome & Intro | 10 min |
| 0:10 | Part 1: Setup | 30 min |
| 0:40 | Part 2: First Uploads | 30 min |
| 1:10 | **Break** | 10 min |
| 1:20 | Part 3: Core Concepts | 20 min |
| 1:40 | Part 4: Features | 40 min |
| 2:20 | **Break** | 10 min |
| 2:30 | Part 5: Build Project | 40 min |
| 3:10 | Part 6: Next Steps | 20 min |
| 3:30 | **End** | - |

**Total: 3 hours 30 minutes (with breaks)**

---

## Success Metrics

Workshop is successful if participants:
- ✅ Successfully upload at least one blob
- ✅ Download and verify a peer's blob
- ✅ Understand Walrus basics
- ✅ Complete at least one feature exercise
- ✅ Know how to continue learning
- ✅ Feel confident to use Walrus in projects

---

**Good luck with your workshop! 🎉**

Questions? Join us on Discord: https://discord.gg/sui
