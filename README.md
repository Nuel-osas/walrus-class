# Walrus Storage Class Materials

Complete teaching materials for Walrus decentralized storage on the Sui blockchain.

## ⚡ **START HERE: 4-Hour Class Plan**

**Teaching today with only 4 hours?**
👉 **Use: `00-4-HOUR-LESSON-PLAN.md`**

This is your complete, ready-to-teach, 4-hour intensive course with:
- ✅ Exact timing for each section
- ✅ Hands-on exercises built in
- ✅ Two 10-minute breaks included
- ✅ Complete code examples
- ✅ Student projects
- ✅ No fluff, all essentials

---

## 📚 What's Included

This repository contains everything you need to teach or learn about Walrus storage:

### 1. **Overview & Concepts** (`01-walrus-overview.md`)
- What is Walrus?
- How it works (erasure coding, architecture)
- Key concepts (blobs, epochs, quilts)
- Use cases and benefits
- Comparison with other storage solutions

### 2. **Getting Started Guide** (`02-getting-started-guide.md`)
- Installation instructions (all platforms)
- Sui wallet setup
- Getting testnet/mainnet SUI tokens
- Your first blob upload
- Troubleshooting common issues
- Practice exercises

### 3. **Code Examples** (`03-code-examples.md`)
- CLI command examples
- TypeScript/JavaScript integration
- Python integration
- Web application examples
- Advanced features (encryption, progress tracking)
- Hands-on exercises

### 4. **Lesson Plan** (`04-lesson-plan.md`)
- Complete 4-6 hour course structure
- Session-by-session breakdown
- Teaching activities and exercises
- Quizzes and assessments
- Final project ideas
- Instructor tips and checklist

### 5. **CLI Cheat Sheet** (`05-cli-cheat-sheet.md`)
- Quick reference for all commands
- Common workflows
- Troubleshooting guide
- Best practices
- HTTP API reference

## 🎯 Who Is This For?

### Instructors
- Blockchain educators
- University professors
- Bootcamp instructors
- Workshop facilitators
- Corporate trainers

### Students
- Web3 developers
- Blockchain enthusiasts
- Full-stack developers
- Data engineers
- Anyone interested in decentralized storage

## 🚀 Quick Start

### For Students

1. **Start with the Overview**
   ```bash
   Read: 01-walrus-overview.md
   ```
   Understand what Walrus is and why it matters.

2. **Follow the Getting Started Guide**
   ```bash
   Read: 02-getting-started-guide.md
   ```
   Install Walrus CLI and upload your first blob.

3. **Try Code Examples**
   ```bash
   Read: 03-code-examples.md
   ```
   Work through examples in your preferred language.

4. **Keep the Cheat Sheet Handy**
   ```bash
   Reference: 05-cli-cheat-sheet.md
   ```
   Quick reference while coding.

### For Instructors

1. **Review All Materials**
   Read through all documents to familiarize yourself.

2. **Follow the Lesson Plan**
   ```bash
   Guide: 04-lesson-plan.md
   ```
   Structured 6-session course with activities.

3. **Test Everything**
   Run all commands and code examples before class.

4. **Prepare Environment**
   - Have testnet SUI ready
   - Test network connectivity
   - Prepare example files
   - Set up demo projects

## 📋 Prerequisites

### Knowledge
- Basic command-line usage
- Understanding of blockchain concepts (helpful but not required)
- Programming experience (for coding sections)

### Software
- macOS, Linux, or Windows computer
- Internet connection
- Terminal/command-line access

### Optional
- Node.js (for TypeScript examples)
- Python 3.x (for Python examples)
- Code editor (VS Code, Sublime, etc.)

## 🛠️ Installation

Follow the detailed installation instructions in `02-getting-started-guide.md`.

**Quick Install (Testnet):**
```bash
# Install Walrus CLI
curl -sSf https://install.wal.app | sh -s -- -n testnet

# Add to PATH
export PATH="$HOME/.local/bin:$PATH"

# Verify
walrus --version
```

## 📖 Course Outline

### Session 1: Introduction (60 min)
- What is Walrus?
- Key concepts
- The Sui connection

### Session 2: Hands-On Setup (60 min)
- Environment setup
- First blob upload

### Session 3: CLI Deep Dive (60 min)
- Storage options
- Quilts
- Metadata

### Session 4: Programming Integration (90 min)
- TypeScript/JavaScript
- Python
- Code projects

### Session 5: Web Applications (60 min)
- HTTP API
- Image gallery project

### Session 6: Advanced Topics (60 min)
- Security & encryption
- Cost optimization
- Best practices

### Final Project (60 min)
- Build a complete application

## 🎓 Learning Outcomes

After completing this course, you will be able to:

✅ Explain how Walrus works and its benefits
✅ Install and configure Walrus CLI
✅ Upload and download blobs using various methods
✅ Integrate Walrus into applications (TypeScript/Python/Web)
✅ Implement advanced features (encryption, quilts, metadata)
✅ Optimize costs and follow best practices
✅ Build production-ready decentralized storage applications

## 🌐 Useful Resources

### Official Resources
- **Documentation**: https://docs.wal.app
- **Website**: https://www.walrus.xyz
- **GitHub**: https://github.com/MystenLabs/walrus
- **Sui Docs**: https://docs.sui.io

### Community
- **Discord**: https://discord.gg/sui
- **Twitter**: @Walrus_xyz
- **Forum**: https://forums.sui.io

### Examples & Tutorials
- **Official Examples**: https://github.com/MystenLabs/walrus-docs
- **Community Guide**: https://github.com/Angleito/Walrus-Code-Guide
- **Sui by Examples**: https://suibyexamples.com

## 🧪 Practice Exercises

### Beginner
1. Install Walrus CLI and verify installation
2. Upload a text file and download it
3. Check blob status and list your blobs
4. Create and delete a deletable blob

### Intermediate
5. Create a quilt with multiple files
6. Add metadata to blobs using attributes
7. Build a TypeScript script to upload files
8. Create a simple web upload form

### Advanced
9. Implement encryption before upload
10. Build a file-sharing application
11. Create an automated backup system
12. Integrate Walrus with a Sui smart contract

## 💡 Project Ideas

1. **Decentralized File Sharing** - Share files via blob IDs
2. **NFT Metadata Storage** - Store NFT data on Walrus
3. **Document Management** - Organize docs with metadata
4. **Decentralized Blog** - Host blog posts on Walrus
5. **Photo Gallery** - Store and display images
6. **Backup Tool** - Automated file backup system
7. **Data Marketplace** - Buy/sell access to data
8. **Version Control** - Track file versions over time

## 🐛 Troubleshooting

### Common Issues

**Problem**: "walrus: command not found"
**Solution**: Add `$HOME/.local/bin` to your PATH

**Problem**: "Insufficient gas"
**Solution**: Get testnet SUI with `sui client faucet`

**Problem**: "Blob not found"
**Solution**: Check blob status with `walrus blob-status --blob-id <ID>`

**Problem**: "Connection refused"
**Solution**: Check network health with `walrus health --committee`

See `02-getting-started-guide.md` for detailed troubleshooting.

## 🤝 Contributing

Found an error or have a suggestion? Please:
1. Check existing issues
2. Open a new issue with details
3. Submit a pull request with fixes

## 📜 License

These educational materials are provided for teaching and learning purposes.

Walrus is developed by Mysten Labs.

## 🙏 Acknowledgments

- **Mysten Labs** - For creating Walrus and Sui
- **Walrus Community** - For documentation and examples
- **Contributors** - Everyone who helps improve these materials

## 📞 Support

- **Technical Issues**: https://discord.gg/sui
- **Documentation**: https://docs.wal.app
- **Course Questions**: See instructor or community forum

---

## 🚀 Ready to Get Started?

1. **Students**: Begin with `01-walrus-overview.md`
2. **Instructors**: Review `04-lesson-plan.md`
3. **Quick Reference**: Bookmark `05-cli-cheat-sheet.md`

**Happy learning! Welcome to the future of decentralized storage!** 🎉

---

**Last Updated**: December 2025
**Walrus Version**: Mainnet (launched March 27, 2025)
**Sui Network**: Mainnet & Testnet
