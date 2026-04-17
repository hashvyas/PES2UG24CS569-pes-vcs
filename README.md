# PES-VCS — A Version Control System from Scratch

**Author:** V Vyas  
**SRN:** PES2UG24CS569

---

## Overview

PES-VCS is a fully functional version control system built from scratch in C, implementing core Git concepts including content-addressable storage, staging area, commits, and history tracking. This project demonstrates deep understanding of filesystem operations, data structures, and version control internals.

**Platform:** Ubuntu 22.04 / WSL Debian

---

## What This Project Does

PES-VCS implements a lightweight version control system with the following features:

- **Content-Addressable Storage**: Files are stored by their SHA-256 hash, enabling automatic deduplication
- **Staging Area (Index)**: Track which files will be included in the next commit
- **Commit History**: Create snapshots with metadata (author, timestamp, message) and parent pointers
- **Tree Objects**: Efficiently represent directory structures with support for nested paths
- **Atomic Operations**: All writes use temp-file-then-rename pattern for crash safety
- **Object Integrity**: SHA-256 verification ensures data hasn't been corrupted

---

## Quick Start

### Prerequisites

```bash
sudo apt update && sudo apt install -y gcc build-essential libssl-dev
```

### Building

```bash
make          # Build the pes binary
make all      # Build pes + test binaries
make clean    # Remove all build artifacts
```

### Basic Usage

```bash
# Initialize a repository
./pes init

# Configure your identity
export PES_AUTHOR="Your Name <your.email@example.com>"

# Stage files
./pes add file1.txt file2.txt

# Check status
./pes status

# Create a commit
./pes commit -m "Initial commit"

# View history
./pes log
```

---

## Architecture

### Directory Structure

```
.pes/
├── objects/          # Content-addressable object store
│   ├── 2f/          # Sharded by first 2 hex chars
│   │   └── 8a3b...  # Blob, tree, or commit object
│   └── a1/
│       └── 9c4e...
├── refs/
│   └── heads/
│       └── main     # Branch pointer (commit hash)
├── index            # Staging area (text format)
└── HEAD             # Current branch reference
```

### Object Types

1. **Blob**: Raw file contents
2. **Tree**: Directory listing (maps names to blob/tree hashes)
3. **Commit**: Snapshot with metadata (tree, parent, author, message)

---

## Implementation Highlights

- **Content-Addressable Storage**: Using cryptographic hashes as identifiers
- **Atomic Operations**: Ensuring consistency with temp-file-then-rename
- **Data Deduplication**: Storing identical content only once
- **Merkle Trees**: Using hash trees for efficient change detection
- **Crash Safety**: fsync() and atomic operations for durability
- **Binary Serialization**: Efficient on-disk formats

---

## Project Structure

| File | Purpose | Status |
|------|---------|--------|
| `object.c` | Content-addressable object store |  Complete |
| `tree.c` | Tree serialization and construction |  Complete |
| `index.c` | Staging area implementation |  Complete |
| `commit.c` | Commit creation and history |  Complete |
| `pes.c` | CLI entry point |  Provided |
| `pes.h` | Core data structures |  Provided |

---

## Further Reading

- **Git Internals** (Pro Git book): https://git-scm.com/book/en/v2/Git-Internals-Plumbing-and-Porcelain
- **Git from the inside out**: https://codewords.recurse.com/issues/two/git-from-the-inside-out
- **The Git Parable**: https://tom.preston-werner.com/2009/05/19/the-git-parable.html
