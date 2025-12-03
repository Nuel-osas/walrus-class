# Walrus Storage: Code Examples & Tutorials

This guide provides hands-on code examples for working with Walrus across different platforms and languages.

## Table of Contents
1. [CLI Examples](#cli-examples)
2. [TypeScript/JavaScript Examples](#typescriptjavascript-examples)
3. [Python Examples](#python-examples)
4. [Web Application Examples](#web-application-examples)
5. [Advanced Features](#advanced-features)

---

## CLI Examples

### Example 1: Basic File Upload and Download

```bash
# Create a sample file
echo "This is my first Walrus blob!" > first-blob.txt

# Upload to Walrus (store for 10 epochs)
walrus store first-blob.txt --epochs 10

# Output will show:
# Blob ID: <BLOB_ID>
# Object ID: <OBJECT_ID>
# Cost: X.XX SUI

# Download the blob (replace <BLOB_ID> with actual ID)
walrus read <BLOB_ID> --out downloaded-blob.txt

# Verify contents match
diff first-blob.txt downloaded-blob.txt
```

### Example 2: Storing Images

```bash
# Store an image for maximum duration
walrus store photo.jpg --epochs max

# Check blob status
walrus blob-status --blob-id <BLOB_ID>

# Download image
walrus read <BLOB_ID> --out retrieved-photo.jpg
```

### Example 3: Deletable Blobs

```bash
# Store a temporary file as deletable
walrus store temp-data.json --epochs 5 --deletable

# Later, delete it before expiration
walrus delete --blob-id <BLOB_ID>

# Or delete using file
walrus delete --file temp-data.json
```

### Example 4: Shared Blobs for Community Data

```bash
# Create a shared blob that others can fund
walrus store community-dataset.csv --epochs 20 --share --amount 5000000000

# Anyone can extend this blob's lifetime
walrus fund-shared-blob --blob-obj-id <BLOB_OBJ_ID> --amount 2000000000
```

### Example 5: Working with Quilts (Multiple Files)

```bash
# Create a directory with multiple files
mkdir my-website
echo "<html><body>Home</body></html>" > my-website/index.html
echo "body { color: blue; }" > my-website/style.css
echo "console.log('Hello');" > my-website/app.js

# Store all files as a quilt
walrus store-quilt --epochs 15 --paths my-website

# Download specific files from quilt
walrus read-quilt --quilt-id <QUILT_ID> --identifiers index.html style.css --out ./downloaded
```

### Example 6: Blob Attributes (Metadata)

```bash
# Upload a file
walrus store document.pdf --epochs 10

# Add metadata attributes
walrus set-blob-attribute <BLOB_OBJ_ID> \
  --attr "title" "Research Paper" \
  --attr "author" "John Doe" \
  --attr "category" "science"

# Retrieve attributes
walrus get-blob-attribute <BLOB_OBJ_ID>

# Remove specific attributes
walrus remove-blob-attribute-fields <BLOB_OBJ_ID> --keys "category"
```

### Example 7: Extending Blob Lifetime

```bash
# List your blobs to find one nearing expiration
walrus list-blobs

# Extend a blob's lifetime
walrus extend --blob-obj-id <BLOB_OBJECT_ID> --epochs 10

# For shared blobs
walrus extend --blob-obj-id <BLOB_OBJECT_ID> --shared --epochs 10
```

---

## TypeScript/JavaScript Examples

### Example 1: Basic Setup

```typescript
// Install dependencies
// npm install @mysten/walrus-sdk @mysten/sui.js

import { WalrusClient } from '@mysten/walrus-sdk';
import { Ed25519Keypair } from '@mysten/sui.js/keypairs/ed25519';
import { SuiClient } from '@mysten/sui.js/client';

// Initialize Walrus client
const walrusClient = new WalrusClient({
  aggregator: 'https://aggregator.walrus.xyz', // Mainnet
  publisher: 'https://publisher.walrus.xyz',
});

// Initialize Sui client
const suiClient = new SuiClient({
  url: 'https://fullnode.mainnet.sui.io:443'
});

// Load your keypair (from environment or file)
const keypair = Ed25519Keypair.fromSecretKey(
  Uint8Array.from(Buffer.from(process.env.SUI_PRIVATE_KEY!, 'hex'))
);
```

### Example 2: Upload a Blob

```typescript
import fs from 'fs';

async function uploadBlob(filePath: string, epochs: number) {
  try {
    // Read file
    const fileContent = fs.readFileSync(filePath);

    // Upload to Walrus
    const result = await walrusClient.store({
      blob: fileContent,
      epochs: epochs,
      signer: keypair,
    });

    console.log('Upload successful!');
    console.log('Blob ID:', result.blobId);
    console.log('Object ID:', result.objectId);
    console.log('Expiry Epoch:', result.expiryEpoch);

    return result.blobId;
  } catch (error) {
    console.error('Upload failed:', error);
    throw error;
  }
}

// Usage
uploadBlob('./my-file.txt', 10);
```

### Example 3: Download a Blob

```typescript
async function downloadBlob(blobId: string, outputPath: string) {
  try {
    // Read blob from Walrus
    const blobData = await walrusClient.read(blobId);

    // Save to file
    fs.writeFileSync(outputPath, blobData);

    console.log(`Blob downloaded to: ${outputPath}`);
    console.log(`Size: ${blobData.length} bytes`);

    return blobData;
  } catch (error) {
    console.error('Download failed:', error);
    throw error;
  }
}

// Usage
downloadBlob('<BLOB_ID>', './downloaded-file.txt');
```

### Example 4: Upload with Deletable Option

```typescript
async function uploadDeletableBlob(filePath: string, epochs: number) {
  const fileContent = fs.readFileSync(filePath);

  const result = await walrusClient.store({
    blob: fileContent,
    epochs: epochs,
    deletable: true,
    signer: keypair,
  });

  console.log('Deletable blob created:', result.blobId);
  return result;
}

async function deleteBlob(blobId: string) {
  try {
    await walrusClient.delete({
      blobId: blobId,
      signer: keypair,
    });

    console.log('Blob deleted successfully');
  } catch (error) {
    console.error('Deletion failed:', error);
  }
}
```

### Example 5: Check Blob Status

```typescript
async function checkBlobStatus(blobId: string) {
  try {
    const status = await walrusClient.blobStatus(blobId);

    console.log('Blob Status:');
    console.log('- Stored:', status.isStored);
    console.log('- Available Shards:', status.availableShards);
    console.log('- Expiry Epoch:', status.expiryEpoch);

    return status;
  } catch (error) {
    console.error('Status check failed:', error);
    throw error;
  }
}
```

### Example 6: Web Application - Image Upload

```typescript
// React component example
import React, { useState } from 'react';

function ImageUploader() {
  const [blobId, setBlobId] = useState<string | null>(null);
  const [uploading, setUploading] = useState(false);

  const handleFileUpload = async (event: React.ChangeEvent<HTMLInputElement>) => {
    const file = event.target.files?.[0];
    if (!file) return;

    setUploading(true);

    try {
      // Read file as ArrayBuffer
      const arrayBuffer = await file.arrayBuffer();
      const blob = new Uint8Array(arrayBuffer);

      // Upload to Walrus
      const result = await walrusClient.store({
        blob: blob,
        epochs: 100,
        signer: keypair,
      });

      setBlobId(result.blobId);
      console.log('Image uploaded! Blob ID:', result.blobId);
    } catch (error) {
      console.error('Upload failed:', error);
    } finally {
      setUploading(false);
    }
  };

  return (
    <div>
      <input
        type="file"
        onChange={handleFileUpload}
        accept="image/*"
        disabled={uploading}
      />

      {uploading && <p>Uploading...</p>}

      {blobId && (
        <div>
          <p>Blob ID: {blobId}</p>
          <img
            src={`https://aggregator.walrus.xyz/v1/${blobId}`}
            alt="Uploaded"
          />
        </div>
      )}
    </div>
  );
}

export default ImageUploader;
```

---

## Python Examples

### Example 1: Basic Setup

```python
# Install: pip install pysui walrus-python

import os
from pathlib import Path
from walrus import WalrusClient
from pysui import SuiClient, SuiConfig
from pysui.sui.sui_crypto import SuiKeyPair

# Initialize clients
walrus_client = WalrusClient(
    aggregator_url="https://aggregator.walrus.xyz",
    publisher_url="https://publisher.walrus.xyz"
)

sui_config = SuiConfig.default()
sui_client = SuiClient(sui_config)

# Load keypair
keypair = SuiKeyPair.from_secret_key(
    os.environ.get("SUI_PRIVATE_KEY")
)
```

### Example 2: Upload File

```python
def upload_file(file_path: str, epochs: int = 10):
    """Upload a file to Walrus storage"""

    # Read file
    with open(file_path, 'rb') as f:
        file_data = f.read()

    # Upload to Walrus
    result = walrus_client.store(
        blob=file_data,
        epochs=epochs,
        signer=keypair
    )

    print(f"Upload successful!")
    print(f"Blob ID: {result.blob_id}")
    print(f"Object ID: {result.object_id}")
    print(f"Cost: {result.cost} SUI")

    return result.blob_id

# Usage
blob_id = upload_file("data/dataset.csv", epochs=20)
```

### Example 3: Download File

```python
def download_file(blob_id: str, output_path: str):
    """Download a blob from Walrus"""

    try:
        # Read blob
        blob_data = walrus_client.read(blob_id)

        # Save to file
        with open(output_path, 'wb') as f:
            f.write(blob_data)

        print(f"Downloaded to: {output_path}")
        print(f"Size: {len(blob_data)} bytes")

        return blob_data

    except Exception as e:
        print(f"Download failed: {e}")
        raise

# Usage
download_file("<BLOB_ID>", "downloaded-dataset.csv")
```

### Example 4: Batch Upload Multiple Files

```python
from typing import List, Dict
import asyncio

async def batch_upload_files(file_paths: List[str], epochs: int = 10) -> Dict[str, str]:
    """Upload multiple files and return mapping of filename to blob_id"""

    results = {}

    for file_path in file_paths:
        filename = Path(file_path).name

        with open(file_path, 'rb') as f:
            file_data = f.read()

        result = walrus_client.store(
            blob=file_data,
            epochs=epochs,
            signer=keypair
        )

        results[filename] = result.blob_id
        print(f"✓ Uploaded {filename}: {result.blob_id}")

    return results

# Usage
files = ["image1.jpg", "image2.jpg", "image3.jpg"]
blob_ids = asyncio.run(batch_upload_files(files))
```

### Example 5: Check Blob Status

```python
def check_blob_status(blob_id: str):
    """Check if a blob is available"""

    status = walrus_client.blob_status(blob_id)

    print(f"Blob Status for {blob_id}:")
    print(f"  Stored: {status.is_stored}")
    print(f"  Expiry Epoch: {status.expiry_epoch}")
    print(f"  Available Shards: {status.available_shards}/{status.total_shards}")

    if status.is_stored:
        print("  ✓ Blob is available for download")
    else:
        print("  ✗ Blob not found or expired")

    return status
```

---

## Web Application Examples

### Example 1: HTML/JavaScript Upload Form

```html
<!DOCTYPE html>
<html>
<head>
  <title>Walrus Upload Demo</title>
</head>
<body>
  <h1>Upload to Walrus</h1>

  <input type="file" id="fileInput" />
  <button onclick="uploadToWalrus()">Upload</button>

  <div id="result"></div>

  <script>
    const PUBLISHER_URL = 'https://publisher.walrus.xyz';
    const AGGREGATOR_URL = 'https://aggregator.walrus.xyz';

    async function uploadToWalrus() {
      const fileInput = document.getElementById('fileInput');
      const file = fileInput.files[0];

      if (!file) {
        alert('Please select a file');
        return;
      }

      const resultDiv = document.getElementById('result');
      resultDiv.innerHTML = 'Uploading...';

      try {
        // Read file as ArrayBuffer
        const arrayBuffer = await file.arrayBuffer();

        // Upload via HTTP API
        const response = await fetch(`${PUBLISHER_URL}/v1/store?epochs=10`, {
          method: 'PUT',
          body: arrayBuffer,
          headers: {
            'Content-Type': 'application/octet-stream'
          }
        });

        const result = await response.json();

        resultDiv.innerHTML = `
          <h3>Upload Successful!</h3>
          <p><strong>Blob ID:</strong> ${result.blobId}</p>
          <p><strong>URL:</strong> <a href="${AGGREGATOR_URL}/v1/${result.blobId}" target="_blank">
            ${AGGREGATOR_URL}/v1/${result.blobId}
          </a></p>
        `;

      } catch (error) {
        resultDiv.innerHTML = `<p style="color: red;">Upload failed: ${error.message}</p>`;
      }
    }
  </script>
</body>
</html>
```

### Example 2: Image Gallery with Walrus

```html
<!DOCTYPE html>
<html>
<head>
  <title>Walrus Image Gallery</title>
  <style>
    .gallery { display: flex; flex-wrap: wrap; gap: 10px; }
    .gallery img { width: 200px; height: 200px; object-fit: cover; }
  </style>
</head>
<body>
  <h1>Walrus Image Gallery</h1>

  <input type="file" id="imageInput" accept="image/*" multiple />
  <button onclick="uploadImages()">Upload Images</button>

  <div id="gallery" class="gallery"></div>

  <script>
    const PUBLISHER_URL = 'https://publisher.walrus.xyz';
    const AGGREGATOR_URL = 'https://aggregator.walrus.xyz';
    const blobIds = [];

    async function uploadImages() {
      const files = document.getElementById('imageInput').files;

      for (const file of files) {
        const arrayBuffer = await file.arrayBuffer();

        const response = await fetch(`${PUBLISHER_URL}/v1/store?epochs=100`, {
          method: 'PUT',
          body: arrayBuffer
        });

        const result = await response.json();
        blobIds.push(result.blobId);

        // Add to gallery
        addImageToGallery(result.blobId);
      }
    }

    function addImageToGallery(blobId) {
      const gallery = document.getElementById('gallery');
      const img = document.createElement('img');
      img.src = `${AGGREGATOR_URL}/v1/${blobId}`;
      img.alt = `Blob ${blobId}`;
      gallery.appendChild(img);
    }

    // Load existing images (if stored in localStorage)
    function loadGallery() {
      const stored = localStorage.getItem('walrusGallery');
      if (stored) {
        JSON.parse(stored).forEach(addImageToGallery);
      }
    }

    // Save gallery on upload
    function saveGallery() {
      localStorage.setItem('walrusGallery', JSON.stringify(blobIds));
    }

    loadGallery();
  </script>
</body>
</html>
```

---

## Advanced Features

### Example 1: Encryption Before Upload

```typescript
import crypto from 'crypto';

function encryptData(data: Buffer, password: string): Buffer {
  const algorithm = 'aes-256-cbc';
  const key = crypto.scryptSync(password, 'salt', 32);
  const iv = crypto.randomBytes(16);

  const cipher = crypto.createCipheriv(algorithm, key, iv);
  const encrypted = Buffer.concat([cipher.update(data), cipher.final()]);

  // Prepend IV to encrypted data
  return Buffer.concat([iv, encrypted]);
}

function decryptData(encryptedData: Buffer, password: string): Buffer {
  const algorithm = 'aes-256-cbc';
  const key = crypto.scryptSync(password, 'salt', 32);

  const iv = encryptedData.slice(0, 16);
  const encrypted = encryptedData.slice(16);

  const decipher = crypto.createDecipheriv(algorithm, key, iv);
  return Buffer.concat([decipher.update(encrypted), decipher.final()]);
}

// Usage
const sensitiveData = fs.readFileSync('sensitive.txt');
const encrypted = encryptData(sensitiveData, 'my-secret-password');

// Upload encrypted data
const result = await walrusClient.store({
  blob: encrypted,
  epochs: 10,
  signer: keypair
});

// Download and decrypt
const downloaded = await walrusClient.read(result.blobId);
const decrypted = decryptData(downloaded, 'my-secret-password');
```

### Example 2: Progress Tracking for Large Files

```typescript
async function uploadWithProgress(
  filePath: string,
  epochs: number,
  onProgress: (percent: number) => void
) {
  const stats = fs.statSync(filePath);
  const totalSize = stats.size;
  let uploadedSize = 0;

  // Read file in chunks
  const chunkSize = 1024 * 1024; // 1MB chunks
  const chunks: Buffer[] = [];

  const readStream = fs.createReadStream(filePath, { highWaterMark: chunkSize });

  for await (const chunk of readStream) {
    chunks.push(chunk);
    uploadedSize += chunk.length;
    onProgress((uploadedSize / totalSize) * 100);
  }

  const fileContent = Buffer.concat(chunks);

  // Upload to Walrus
  const result = await walrusClient.store({
    blob: fileContent,
    epochs: epochs,
    signer: keypair
  });

  onProgress(100);
  return result;
}

// Usage
uploadWithProgress('large-video.mp4', 50, (percent) => {
  console.log(`Upload progress: ${percent.toFixed(2)}%`);
});
```

---

## Practice Exercises

### Exercise 1: CLI Basics
1. Store 3 different file types (text, image, JSON)
2. List all your blobs
3. Download one and verify integrity
4. Delete one deletable blob

### Exercise 2: TypeScript Integration
1. Create a Node.js app that uploads a directory of files
2. Store blob IDs in a JSON manifest file
3. Create a download function that reconstructs the directory

### Exercise 3: Web Application
1. Build a simple file-sharing web app
2. Upload files to Walrus
3. Generate shareable links
4. Display upload/download progress

### Exercise 4: Advanced Features
1. Encrypt a file before uploading
2. Add metadata using blob attributes
3. Create a shared blob that others can fund
4. Implement automatic blob renewal before expiration

---

**Next**: See the lesson plan for structured teaching approach!
