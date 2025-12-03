# Walrus Class: Quick Start Guide

## 🎓 For Instructors

### Teaching TODAY (4 hours)?

**Use this:**
```
00-4-HOUR-LESSON-PLAN.md
```

**Your prep checklist:**
1. ✅ Read the 4-hour lesson plan (15 min)
2. ✅ Test Walrus CLI on your machine (5 min)
3. ✅ Have testnet SUI in backup wallet (5 min)
4. ✅ Download sample files (2 min)
5. ✅ Print/bookmark `05-cli-cheat-sheet.md`

**Total prep time: ~30 minutes**

---

### Teaching a Full Course (6+ hours)?

**Use this:**
```
04-lesson-plan.md
```

6 detailed sessions with quizzes, projects, and assessments.

---

### Teaching a Workshop (2-3 hours)?

**Use this:**
```
06-hands-on-workshop.md
```

Fast-paced, interactive format perfect for hackathons.

---

## 📖 For Students

### Learning Path

**Step 1: Understanding (20 min)**
```
Read: 01-walrus-overview.md
```
Learn what Walrus is and how it works.

**Step 2: Setup (30 min)**
```
Follow: 02-getting-started-guide.md
```
Install everything and upload your first blob.

**Step 3: Practice (1-2 hours)**
```
Try: 03-code-examples.md
```
Work through examples in your favorite language.

**Step 4: Reference**
```
Keep handy: 05-cli-cheat-sheet.md
```
Quick reference for all commands.

---

## 🚀 Absolute Fastest Start

**Just want to try Walrus RIGHT NOW?**

### 5-Minute Quick Start

```bash
# 1. Install Walrus CLI (testnet)
curl -sSf https://install.wal.app | sh -s -- -n testnet

# 2. Add to PATH
export PATH="$HOME/.local/bin:$PATH"

# 3. Verify
walrus --version

# 4. Install Sui CLI
brew install sui
# OR: cargo install --locked --git https://github.com/MystenLabs/sui.git --branch mainnet sui

# 5. Create wallet
sui client new-address ed25519
# SAVE YOUR RECOVERY PHRASE!

# 6. Get testnet SUI
sui client faucet

# 7. Upload your first file
echo "Hello Walrus!" > hello.txt
walrus store hello.txt --epochs 5

# 8. SAVE THE BLOB ID FROM OUTPUT!

# 9. Download it back
walrus read <YOUR_BLOB_ID> --out retrieved.txt

# 10. Verify
cat retrieved.txt
```

**Congratulations! You just used decentralized storage! 🎉**

---

## 📂 File Guide

| File | Purpose | When to Use |
|------|---------|-------------|
| `00-4-HOUR-LESSON-PLAN.md` | **4-hour intensive course** | **Teaching today!** |
| `01-walrus-overview.md` | Concepts & theory | Learning fundamentals |
| `02-getting-started-guide.md` | Installation & setup | First time setup |
| `03-code-examples.md` | Code tutorials | Building applications |
| `04-lesson-plan.md` | Full 6-session course | Multi-day teaching |
| `05-cli-cheat-sheet.md` | Quick reference | During development |
| `06-hands-on-workshop.md` | 2-3 hour workshop | Hackathons, meetups |
| `README.md` | Overview of all materials | Navigation |
| `QUICK-START.md` | This file! | Getting oriented |

---

## 🎯 What Should I Use?

### I'm teaching a class TODAY (4 hours)
→ `00-4-HOUR-LESSON-PLAN.md`

### I'm a student learning Walrus
→ Start: `01-walrus-overview.md`
→ Then: `02-getting-started-guide.md`

### I need a command reference
→ `05-cli-cheat-sheet.md`

### I want code examples
→ `03-code-examples.md`

### I'm running a workshop
→ `06-hands-on-workshop.md`

### I'm teaching a full course
→ `04-lesson-plan.md`

---

## 🆘 Emergency Help

### Walrus Not Working?

```bash
# Check installation
walrus --version

# Fix PATH
export PATH="$HOME/.local/bin:$PATH"
source ~/.bashrc

# Check network
walrus health --committee
```

### No SUI Tokens?

```bash
# Get testnet SUI
sui client faucet

# Wait 30 seconds, then verify
sui client gas
```

### Still Stuck?

1. Check `02-getting-started-guide.md` → Troubleshooting section
2. Ask on Discord: https://discord.gg/sui
3. Check docs: https://docs.wal.app

---

## 📞 Resources

- **Official Docs**: https://docs.wal.app
- **Discord Support**: https://discord.gg/sui
- **GitHub**: https://github.com/MystenLabs/walrus
- **Website**: https://www.walrus.xyz

---

## ✅ Pre-Class Checklist (Instructors)

**30 minutes before class:**

- [ ] Walrus network is operational: `walrus health`
- [ ] Your CLI is working: `walrus --version`
- [ ] You have testnet SUI: `sui client gas`
- [ ] Sample files ready (text, image, etc.)
- [ ] Screen sharing tested
- [ ] Timer/clock visible
- [ ] Lesson plan open: `00-4-HOUR-LESSON-PLAN.md`
- [ ] Cheat sheet accessible: `05-cli-cheat-sheet.md`

---

## ✅ Pre-Study Checklist (Students)

**Before class:**

- [ ] Computer with terminal access
- [ ] Internet connection
- [ ] Text editor installed
- [ ] Read: `01-walrus-overview.md` (optional but helpful)
- [ ] Join Discord: https://discord.gg/sui (optional)

---

**Ready? Let's go! 🚀**

Choose your path above and start learning about decentralized storage with Walrus!
