# Walrus Storage: 4-Hour Intensive Course

**Duration**: Exactly 4 hours
**Format**: Fast-paced, hands-on focused
**Breaks**: 2 × 10-minute breaks included

---

## 🎯 Course Goals

By the end of 4 hours, students will:
1. ✅ Understand what Walrus is and why it matters
2. ✅ Have working Walrus CLI setup
3. ✅ Upload/download blobs confidently
4. ✅ Know key features (deletable, quilts, metadata)
5. ✅ Build a working application
6. ✅ Know how to continue learning

---

## 📅 Schedule Overview

| Time | Activity | Duration |
|------|----------|----------|
| 0:00-0:40 | **Hour 1**: Intro + Setup | 40 min |
| 0:40-1:20 | **Hour 1**: First Uploads | 40 min |
| 1:20-1:30 | **BREAK** | 10 min |
| 1:30-2:10 | **Hour 2**: Core Features | 40 min |
| 2:10-2:50 | **Hour 2**: Code Integration | 40 min |
| 2:50-3:00 | **BREAK** | 10 min |
| 3:00-3:50 | **Hour 3**: Build Project | 50 min |
| 3:50-4:00 | **Hour 4**: Wrap-up | 10 min |

---

## Hour 1: Foundation (80 minutes)

### Part 1: Introduction (0:00-0:20) - 20 minutes

**What is Walrus? (10 min)**

Quick overview:
- Decentralized storage for Sui blockchain
- Uses erasure coding (not full replication)
- ~5x storage cost vs 3x for replication = more efficient
- Data is PUBLIC - encrypt sensitive files
- Pay per epoch (time period)

**Key Concepts (10 min)**

Must-know terms:
- **Blob**: Your file
- **Blob ID**: Unique identifier (like a URL)
- **Epoch**: Time period for storage
- **Deletable**: Can delete before expiration
- **Quilt**: Multiple files together
- **Sui**: Blockchain for payments/coordination

**Show live demo:**
```bash
# Upload
walrus store demo.jpg --epochs 10

# Download
walrus read <BLOB_ID> --out retrieved.jpg
```

### Part 2: Setup (0:20-0:40) - 20 minutes

**Everyone follows along:**

**Step 1: Install Walrus (5 min)**
```bash
# Testnet version
curl -sSf https://install.wal.app | sh -s -- -n testnet

# Add to PATH
export PATH="$HOME/.local/bin:$PATH"

# Verify
walrus --version
```

**Step 2: Install Sui CLI (5 min)**
```bash
# macOS
brew install sui

# Or cargo
cargo install --locked --git https://github.com/MystenLabs/sui.git --branch mainnet sui

# Create wallet
sui client new-address ed25519
# SAVE YOUR RECOVERY PHRASE!
```

**Step 3: Get Testnet SUI (5 min)**
```bash
# Request tokens
sui client faucet

# Verify
sui client gas
```

**Step 4: Troubleshooting (5 min)**
Instructor helps anyone stuck.

**✅ Checkpoint**: Everyone should successfully run `walrus --version`

### Part 3: First Uploads (0:40-1:20) - 40 minutes

**Exercise 1: Upload Text (10 min)**

```bash
# Create file
echo "Hello Walrus! - [Your Name]" > hello.txt

# Upload
walrus store hello.txt --epochs 5

# SAVE YOUR BLOB ID!
```

**Activity**: Share blob IDs in chat.

**Exercise 2: Download Peer's Blob (10 min)**

```bash
# Download someone else's blob
walrus read <PEER_BLOB_ID> --out peer-hello.txt

# View it
cat peer-hello.txt
```

**This demonstrates decentralization!**

**Exercise 3: Upload Image (10 min)**

```bash
# Get sample image
curl -o sample.jpg "https://picsum.photos/800/600"

# Upload to Walrus
walrus store sample.jpg --epochs 10

# Download and verify
walrus read <BLOB_ID> --out retrieved.jpg

# Open to check
open retrieved.jpg  # macOS
```

**Exercise 4: Blob Management (10 min)**

```bash
# List your blobs
walrus list-blobs

# Check status
walrus blob-status --blob-id <BLOB_ID>

# Get blob ID from file (without uploading)
walrus blob-id sample.jpg
```

---

## **🛑 BREAK #1 (1:20-1:30) - 10 minutes**

---

## Hour 2: Core Features & Integration (80 minutes)

### Part 4: Essential Features (1:30-2:10) - 40 minutes

**Feature 1: Deletable Blobs (10 min)**

```bash
# Upload as deletable
echo "Temporary data" > temp.txt
walrus store temp.txt --epochs 5 --deletable

# Later, delete it
walrus delete --blob-id <BLOB_ID>

# Verify deletion
walrus blob-status --blob-id <BLOB_ID>
```

**Use case**: Temporary files, caching

**Feature 2: Quilts - Multiple Files (15 min)**

```bash
# Create a mini website
mkdir my-site

cat > my-site/index.html << 'EOF'
<!DOCTYPE html>
<html>
<head>
  <title>My Walrus Site</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <h1>Hello from Walrus!</h1>
  <p>This site is stored on decentralized storage!</p>
</body>
</html>
EOF

cat > my-site/style.css << 'EOF'
body {
  font-family: Arial;
  max-width: 800px;
  margin: 50px auto;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 20px;
}
EOF

# Upload as quilt
walrus store-quilt --epochs 20 --paths my-site

# SAVE QUILT ID!

# Download it
walrus read-quilt --quilt-id <QUILT_ID> --out downloaded-site

# Open index.html in browser!
```

**Feature 3: Blob Attributes (10 min)**

```bash
# Upload a file
walrus store document.txt --epochs 10

# Add metadata
walrus set-blob-attribute <BLOB_OBJ_ID> \
  --attr "title" "Important Document" \
  --attr "author" "Your Name" \
  --attr "category" "education"

# Retrieve attributes
walrus get-blob-attribute <BLOB_OBJ_ID>
```

**Use case**: Organization, search, metadata

**Quick Recap (5 min)**

What we've learned:
- ✅ Basic upload/download
- ✅ Deletable blobs
- ✅ Quilts for multiple files
- ✅ Metadata with attributes

### Part 5: Code Integration (2:10-2:50) - 40 minutes

**Choose ONE based on your audience:**

#### Option A: Web/JavaScript Track

**Simple HTML Uploader (40 min)**

Create `uploader.html`:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Walrus Uploader</title>
  <style>
    body {
      font-family: Arial;
      max-width: 600px;
      margin: 50px auto;
      padding: 20px;
    }
    .upload-area {
      border: 2px dashed #ccc;
      padding: 40px;
      text-align: center;
      margin: 20px 0;
    }
    button {
      background: #667eea;
      color: white;
      border: none;
      padding: 10px 20px;
      font-size: 16px;
      cursor: pointer;
      border-radius: 5px;
    }
    button:hover { background: #5568d3; }
    #result {
      margin-top: 20px;
      padding: 15px;
      background: #f0f0f0;
      border-radius: 5px;
    }
    img { max-width: 100%; margin-top: 10px; }
  </style>
</head>
<body>
  <h1>📦 Walrus File Uploader</h1>

  <div class="upload-area">
    <input type="file" id="fileInput" />
    <br><br>
    <button onclick="uploadFile()">Upload to Walrus</button>
  </div>

  <div id="result"></div>

  <script>
    const PUBLISHER = 'https://publisher-testnet.walrus.xyz';
    const AGGREGATOR = 'https://aggregator-testnet.walrus.xyz';

    async function uploadFile() {
      const fileInput = document.getElementById('fileInput');
      const file = fileInput.files[0];

      if (!file) {
        alert('Please select a file first!');
        return;
      }

      const resultDiv = document.getElementById('result');
      resultDiv.innerHTML = '<p>⏳ Uploading...</p>';

      try {
        // Read file as ArrayBuffer
        const arrayBuffer = await file.arrayBuffer();

        // Upload to Walrus via HTTP API
        const response = await fetch(`${PUBLISHER}/v1/store?epochs=10`, {
          method: 'PUT',
          body: arrayBuffer,
          headers: {
            'Content-Type': 'application/octet-stream'
          }
        });

        if (!response.ok) {
          throw new Error(`Upload failed: ${response.statusText}`);
        }

        const result = await response.json();

        // Display result
        resultDiv.innerHTML = `
          <h3>✅ Upload Successful!</h3>
          <p><strong>Blob ID:</strong> <code>${result.blobId}</code></p>
          <p><strong>Cost:</strong> ${result.cost || 'N/A'} SUI</p>
          <p><strong>Expires:</strong> Epoch ${result.endEpoch || 'N/A'}</p>
          <p><a href="${AGGREGATOR}/v1/${result.blobId}" target="_blank">View File</a></p>
        `;

        // If image, show preview
        if (file.type.startsWith('image/')) {
          resultDiv.innerHTML += `
            <h4>Preview:</h4>
            <img src="${AGGREGATOR}/v1/${result.blobId}" alt="Uploaded image">
          `;
        }

      } catch (error) {
        resultDiv.innerHTML = `<p style="color: red;">❌ Error: ${error.message}</p>`;
      }
    }
  </script>
</body>
</html>
```

**Activity:**
1. Create the file
2. Open in browser
3. Upload an image
4. Share the blob ID with classmates
5. Visit each other's images

#### Option B: Python Track

**Python Upload Script (40 min)**

Create `walrus_uploader.py`:

```python
#!/usr/bin/env python3
"""
Simple Walrus uploader script
"""

import os
import sys
import subprocess
import json

def upload_file(file_path, epochs=10, deletable=False):
    """Upload a file to Walrus storage"""

    if not os.path.exists(file_path):
        print(f"❌ File not found: {file_path}")
        return None

    # Build command
    cmd = ['walrus', 'store', file_path, '--epochs', str(epochs)]

    if deletable:
        cmd.append('--deletable')

    print(f"📤 Uploading {file_path}...")

    try:
        # Run command
        result = subprocess.run(cmd, capture_output=True, text=True)

        if result.returncode != 0:
            print(f"❌ Upload failed: {result.stderr}")
            return None

        # Parse output for blob ID
        output = result.stdout
        for line in output.split('\n'):
            if 'Blob ID' in line:
                blob_id = line.split(':')[-1].strip()
                print(f"✅ Upload successful!")
                print(f"📦 Blob ID: {blob_id}")
                return blob_id

        return None

    except Exception as e:
        print(f"❌ Error: {e}")
        return None

def download_file(blob_id, output_path):
    """Download a blob from Walrus"""

    print(f"📥 Downloading blob {blob_id}...")

    cmd = ['walrus', 'read', blob_id, '--out', output_path]

    try:
        result = subprocess.run(cmd, capture_output=True, text=True)

        if result.returncode != 0:
            print(f"❌ Download failed: {result.stderr}")
            return False

        print(f"✅ Downloaded to: {output_path}")
        return True

    except Exception as e:
        print(f"❌ Error: {e}")
        return False

def list_blobs():
    """List all blobs"""

    print("📋 Your blobs:")

    cmd = ['walrus', 'list-blobs']

    try:
        subprocess.run(cmd)
    except Exception as e:
        print(f"❌ Error: {e}")

def main():
    if len(sys.argv) < 2:
        print("Usage:")
        print("  Upload:   python walrus_uploader.py upload <file> [epochs]")
        print("  Download: python walrus_uploader.py download <blob_id> <output>")
        print("  List:     python walrus_uploader.py list")
        sys.exit(1)

    command = sys.argv[1]

    if command == 'upload':
        if len(sys.argv) < 3:
            print("❌ Please provide a file to upload")
            sys.exit(1)

        file_path = sys.argv[2]
        epochs = int(sys.argv[3]) if len(sys.argv) > 3 else 10

        upload_file(file_path, epochs)

    elif command == 'download':
        if len(sys.argv) < 4:
            print("❌ Please provide blob ID and output path")
            sys.exit(1)

        blob_id = sys.argv[2]
        output_path = sys.argv[3]

        download_file(blob_id, output_path)

    elif command == 'list':
        list_blobs()

    else:
        print(f"❌ Unknown command: {command}")
        sys.exit(1)

if __name__ == '__main__':
    main()
```

**Activity:**
1. Create the script
2. Make it executable: `chmod +x walrus_uploader.py`
3. Upload files: `python walrus_uploader.py upload image.jpg 15`
4. Download: `python walrus_uploader.py download <BLOB_ID> output.jpg`
5. List blobs: `python walrus_uploader.py list`

---

## **🛑 BREAK #2 (2:50-3:00) - 10 minutes**

---

## Hour 3: Build a Project (50 minutes)

### Part 6: Hands-On Project (3:00-3:50) - 50 minutes

**Choose ONE project:**

#### Project A: Image Gallery (Beginner-Friendly)

**Goal**: Build a decentralized image gallery

**HTML File (`gallery.html`):**

```html
<!DOCTYPE html>
<html>
<head>
  <title>Walrus Gallery</title>
  <style>
    body {
      font-family: Arial;
      max-width: 1200px;
      margin: 20px auto;
      padding: 20px;
    }
    .controls {
      background: #f5f5f5;
      padding: 20px;
      border-radius: 8px;
      margin-bottom: 20px;
    }
    .gallery {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
      gap: 20px;
    }
    .gallery-item {
      border: 1px solid #ddd;
      border-radius: 8px;
      overflow: hidden;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    .gallery-item img {
      width: 100%;
      height: 200px;
      object-fit: cover;
    }
    .gallery-item-info {
      padding: 10px;
      background: white;
    }
    .blob-id {
      font-size: 10px;
      color: #666;
      word-break: break-all;
    }
    button {
      background: #667eea;
      color: white;
      border: none;
      padding: 10px 20px;
      font-size: 16px;
      cursor: pointer;
      border-radius: 5px;
      margin-right: 10px;
    }
    button:hover { background: #5568d3; }
  </style>
</head>
<body>
  <h1>📸 Walrus Image Gallery</h1>

  <div class="controls">
    <input type="file" id="imageInput" accept="image/*" multiple>
    <button onclick="uploadImages()">Upload Images</button>
    <button onclick="loadGallery()">Refresh Gallery</button>
    <button onclick="clearGallery()">Clear All</button>
  </div>

  <div id="status"></div>
  <div class="gallery" id="gallery"></div>

  <script>
    const PUBLISHER = 'https://publisher-testnet.walrus.xyz';
    const AGGREGATOR = 'https://aggregator-testnet.walrus.xyz';
    const STORAGE_KEY = 'walrus_gallery';

    // Load gallery on page load
    window.onload = () => {
      loadGallery();
    };

    async function uploadImages() {
      const files = document.getElementById('imageInput').files;
      const status = document.getElementById('status');

      if (files.length === 0) {
        alert('Please select images first!');
        return;
      }

      status.innerHTML = `<p>⏳ Uploading ${files.length} image(s)...</p>`;

      const gallery = getGalleryData();

      for (let i = 0; i < files.length; i++) {
        const file = files[i];

        try {
          const arrayBuffer = await file.arrayBuffer();

          const response = await fetch(`${PUBLISHER}/v1/store?epochs=100`, {
            method: 'PUT',
            body: arrayBuffer
          });

          const result = await response.json();

          // Save to gallery
          gallery.push({
            blobId: result.blobId,
            filename: file.name,
            uploadDate: new Date().toISOString()
          });

          status.innerHTML = `<p>✅ Uploaded ${i + 1}/${files.length}</p>`;

        } catch (error) {
          console.error('Upload failed:', error);
          status.innerHTML = `<p style="color:red;">❌ Error uploading ${file.name}</p>`;
        }
      }

      // Save gallery
      saveGalleryData(gallery);

      status.innerHTML = `<p style="color:green;">✅ All uploads complete!</p>`;

      // Refresh display
      loadGallery();
    }

    function loadGallery() {
      const gallery = getGalleryData();
      const galleryDiv = document.getElementById('gallery');

      if (gallery.length === 0) {
        galleryDiv.innerHTML = '<p>No images yet. Upload some!</p>';
        return;
      }

      galleryDiv.innerHTML = gallery.map(item => `
        <div class="gallery-item">
          <img src="${AGGREGATOR}/v1/${item.blobId}"
               alt="${item.filename}"
               onerror="this.src='https://via.placeholder.com/250x200?text=Failed+to+load'">
          <div class="gallery-item-info">
            <strong>${item.filename}</strong><br>
            <span class="blob-id">Blob ID: ${item.blobId}</span><br>
            <small>${new Date(item.uploadDate).toLocaleDateString()}</small>
          </div>
        </div>
      `).join('');
    }

    function getGalleryData() {
      const data = localStorage.getItem(STORAGE_KEY);
      return data ? JSON.parse(data) : [];
    }

    function saveGalleryData(data) {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(data));
    }

    function clearGallery() {
      if (confirm('Clear all images from gallery? (They will still exist on Walrus)')) {
        localStorage.removeItem(STORAGE_KEY);
        loadGallery();
      }
    }
  </script>
</body>
</html>
```

**Instructions:**
1. Create `gallery.html`
2. Open in browser
3. Upload multiple images
4. See them displayed
5. Share blob IDs with classmates
6. Try loading each other's images

#### Project B: File Backup Tool (Intermediate)

**Goal**: Create a directory backup script

Create `backup.sh`:

```bash
#!/bin/bash

# Walrus Backup Tool
# Usage: ./backup.sh backup <directory>
#        ./backup.sh restore <manifest-file>

MANIFEST="walrus-backup-manifest.json"

backup_directory() {
    local dir=$1

    if [ ! -d "$dir" ]; then
        echo "❌ Directory not found: $dir"
        exit 1
    fi

    echo "📦 Backing up directory: $dir"
    echo "{"
    echo "  \"backup_date\": \"$(date -u +%Y-%m-%dT%H:%M:%SZ)\","
    echo "  \"directory\": \"$dir\","
    echo "  \"files\": ["

    first=true
    for file in "$dir"/*; do
        if [ -f "$file" ]; then
            echo "  📤 Uploading $(basename $file)..."

            # Upload to Walrus
            output=$(walrus store "$file" --epochs 50 2>&1)
            blob_id=$(echo "$output" | grep "Blob ID" | awk '{print $NF}')

            if [ ! -z "$blob_id" ]; then
                if [ "$first" = false ]; then
                    echo ","
                fi
                echo "    {"
                echo "      \"filename\": \"$(basename $file)\","
                echo "      \"path\": \"$file\","
                echo "      \"blob_id\": \"$blob_id\""
                echo -n "    }"
                first=false
            fi
        fi
    done

    echo ""
    echo "  ]"
    echo "}"

    echo ""
    echo "✅ Backup complete! Manifest saved to $MANIFEST"
}

restore_backup() {
    local manifest=$1
    local restore_dir="restored_$(date +%Y%m%d_%H%M%S)"

    if [ ! -f "$manifest" ]; then
        echo "❌ Manifest not found: $manifest"
        exit 1
    fi

    echo "📥 Restoring from manifest: $manifest"
    mkdir -p "$restore_dir"

    # Parse JSON and restore files
    # (Simplified - in production use jq)
    grep "blob_id" "$manifest" | while read line; do
        blob_id=$(echo $line | sed 's/.*: "\(.*\)".*/\1/')
        echo "Downloading blob: $blob_id"
        walrus read "$blob_id" --out "$restore_dir/file_$blob_id"
    done

    echo "✅ Restore complete! Files in: $restore_dir"
}

# Main
case "$1" in
    backup)
        backup_directory "$2" > "$MANIFEST"
        ;;
    restore)
        restore_backup "$2"
        ;;
    *)
        echo "Usage:"
        echo "  Backup:  ./backup.sh backup <directory>"
        echo "  Restore: ./backup.sh restore <manifest-file>"
        exit 1
        ;;
esac
```

**Instructions:**
1. Create the script
2. Make executable: `chmod +x backup.sh`
3. Create test directory with files
4. Backup: `./backup.sh backup ./test-dir`
5. Restore: `./backup.sh restore walrus-backup-manifest.json`

**Work Time**: 40 minutes for students to build and test

**Show & Tell**: Last 10 minutes - volunteers demo their projects

---

## Hour 4: Wrap-up (10 minutes)

### Part 7: Summary & Next Steps (3:50-4:00)

**Quick Recap (3 min)**

What we covered:
- ✅ Walrus fundamentals
- ✅ CLI setup and usage
- ✅ Upload/download blobs
- ✅ Deletable blobs, quilts, metadata
- ✅ Code integration (web or Python)
- ✅ Built a working project!

**What's Next? (3 min)**

**Explore More:**
1. **Encryption**: Protect sensitive data before upload
2. **Shared Blobs**: Community-funded storage
3. **Walrus Sites**: Host entire websites
4. **Sui Integration**: Use in smart contracts
5. **Production**: Move to Mainnet

**Resources (2 min)**

- 📚 **Docs**: https://docs.wal.app
- 💬 **Discord**: https://discord.gg/sui
- 🐙 **GitHub**: https://github.com/MystenLabs/walrus
- 📖 **These materials**: Keep them for reference!

**Practice Challenges:**
1. Encrypt a file before uploading
2. Build an NFT project with Walrus metadata
3. Create a decentralized blog
4. Implement automatic blob renewal

**Final Q&A (2 min)**

---

## 📋 Instructor Checklist

### Before Class
- [ ] Test Walrus network is operational
- [ ] Have backup wallet with testnet SUI
- [ ] Prepare sample files (text, images)
- [ ] Test all code examples
- [ ] Set up screen sharing
- [ ] Print/share cheat sheet

### During Class
- [ ] Start on time
- [ ] Stick to schedule (use timer!)
- [ ] Do live demos
- [ ] Help struggling students during breaks
- [ ] Take 10-min breaks exactly
- [ ] Encourage peer helping

### After Class
- [ ] Share all materials
- [ ] Send resource links
- [ ] Answer questions on Discord
- [ ] Note improvements

---

## ⏱️ Time Management Tips

**Running Behind?**
- Skip detailed theory (share slides)
- Reduce project time to 30 min
- Skip show & tell
- Give pre-written code

**Running Ahead?**
- Add encryption demo
- Show Walrus Sites
- Extended Q&A
- Help with advanced features

**Must-Have Milestones:**
- ✅ Hour 1: Everyone has CLI working
- ✅ Hour 2: Everyone uploaded/downloaded
- ✅ Hour 3: Everyone has working project
- ✅ Hour 4: Resources shared

---

## 🎯 Success Criteria

Students successfully:
- ✅ Upload at least 3 blobs
- ✅ Download a peer's blob
- ✅ Create a quilt OR use attributes
- ✅ Complete one project
- ✅ Know how to continue learning

---

**You've got this! This 4-hour plan is tight but achievable. Focus on hands-on practice over theory. Good luck! 🚀**
