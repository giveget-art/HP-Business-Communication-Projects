# Tools Reference

## Compression & Archive Tools

### gzip
- Purpose: Compress/decompress files
- Common use: Part of tar commands
- Example: `gzip file.txt` creates `file.txt.gz`

### tar
- Purpose: Archive files and directories
- Common options:
  - `-x` : Extract
  - `-z` : Gzip compression
  - `-f` : File name
  - `-v` : Verbose

Example: `tar -xzf git-2.54.0.tar.gz`

## Directory & File Navigation

### tree
- Purpose: Display directory structure
- Example: `tree` shows full hierarchy
- Options:
  - `-L 2` : Limit depth to 2 levels
  - `-a` : Show all files including hidden
  - `-I 'pattern'` : Ignore files matching pattern

## Text Search

### grep
- Purpose: Search for text patterns in files
- Common options:
  - `-R` : Recursive search
  - `-n` : Show line numbers
  - `-i` : Case-insensitive
  - `-l` : Show only filenames

Example: `grep -Rn "function_name" .`

## Text Editors

### Vim
- Modal editor (Normal, Insert, Command modes)
- Commands:
  - `:wq` - Write and quit
  - `:q!` - Quit without saving
  - `/pattern` - Search
  - `dd` - Delete line

### Neovim (nvim)
- Modern fork of Vim with improved features
- Similar commands to Vim
- Better plugin support and defaults

## Build Tools

### make
- Purpose: Automate build process
- Reads: `Makefile` or `makefile`
- Common targets:
  - `make` - Default build
  - `make install` - Install built files
  - `make clean` - Remove build artifacts
  - `make distclean` - Deep clean

Verify Makefile exists: `ls -la | grep -i makefile`

## Installation

### sudo
- Elevates privileges for system-level installation
- Example: `sudo make install` - Install to system directories
