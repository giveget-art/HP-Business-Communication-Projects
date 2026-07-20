# Quick Start: Building Git from Source

## Step-by-Step Instructions

### 1. Download Git Source Archive

```bash
# Option A: Using wget
wget https://github.com/git/git/archive/v2.54.0.tar.gz

# Option B: Using curl
curl -O https://github.com/git/git/archive/v2.54.0.tar.gz
```

### 2. Extract Archive

```bash
tar -xzf git-2.54.0.tar.gz
```

### 3. Enter Source Tree

```bash
cd git-2.54.0
```

### 4. View Source Tree Structure

```bash
tree
```

Or for a filtered view:
```bash
tree -L 2
```

### 5. Search Text in Files

```bash
# Search for a specific pattern
grep -R "pattern" .

# Search for function definitions
grep -R "^int main" .

# Search with line numbers
grep -Rn "pattern" .
```

### 6. Edit Files

Using Vim:
```bash
vim filename.c
```

Using Neovim:
```bash
nvim filename.c
```

### 7. Build (Compile)

```bash
# Check for Makefile
ls -la | grep Makefile

# Run make
make
```

### 8. Install

```bash
sudo make install
```

## Verification

Verify installation:
```bash
git --version
which git
```
