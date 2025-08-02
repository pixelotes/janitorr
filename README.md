# 🧹 Janitorr - Smart Media Duplicate Cleaner

[![Python 3.6+](https://img.shields.io/badge/python-3.6+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Janitorr is an intelligent duplicate media file cleaner designed for home media servers. It automatically detects and removes duplicate movies and TV episodes while keeping the highest quality versions, helping you maintain a clean and organized media library.

## ✨ Features

### 🎬 **Movie Support**
- **Multi-detection strategy**: Folder-based, title-based, and fuzzy matching
- **Smart parsing**: Extracts titles and years from folder names or filenames
- **Extras protection**: Automatically ignores bonus content and extras folders
- **Size filtering**: Configurable minimum file size to skip samples and trailers

### 📺 **TV Show Support**
- **Episode parsing**: Handles SxxExx format including multi-episode files
- **Series grouping**: Groups by normalized series names
- **Flexible naming**: Works with various TV show naming conventions

### 🤖 **Smart Features**
- **Auto-detection**: Automatically determines if your library contains movies or TV shows
- **Quality scoring**: Advanced algorithm considering resolution, source, codec, and audio
- **Interactive mode**: Review each deletion before it happens
- **Backup creation**: JSON logs of all deletions for recovery
- **Dry-run mode**: See what would be deleted without actually deleting files

## 🚀 Quick Start

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/janitorr.git
cd janitorr

# Make executable (optional)
chmod +x janitorr.py
```

**Requirements**: Python 3.6+ (uses only standard library)

### Basic Usage

```bash
# Dry run to see what would be deleted
python janitorr.py -d "/path/to/media" --dry-run

# Interactive mode for careful deletion
python janitorr.py -d "/path/to/media" --interactive

# Automatic deletion (be careful!)
python janitorr.py -d "/path/to/media"
```

## 📖 Usage Examples

### Movies

```bash
# Auto-detect and clean movie library
python janitorr.py -d "/movies" --mode movie --dry-run

# Enable fuzzy matching for similar titles
python janitorr.py -d "/movies" --mode movie --fuzzy-matching --interactive

# Keep smaller files instead of higher quality
python janitorr.py -d "/movies" --mode movie --reverse --prefer-smaller
```

### TV Shows

```bash
# Clean TV show library
python janitorr.py -d "/tv-shows" --mode tv --dry-run

# Include only specific shows
python janitorr.py -d "/tv-shows" --mode tv --include "Breaking Bad" "Better Call Saul"

# Exclude certain patterns
python janitorr.py -d "/tv-shows" --mode tv --exclude "sample" "trailer"
```

### Advanced Options

```bash
# Custom minimum file size and backup location
python janitorr.py -d "/movies" --min-size-mb 500 --backup custom_backup.json

# Verbose logging with specific patterns
python janitorr.py -d "/media" --verbose --include "1080p" --exclude "cam"

# Keep extras folders and use fuzzy matching
python janitorr.py -d "/movies" --keep-extras --fuzzy-matching --interactive
```

## 🎯 Command Line Options

### Required
- `-d, --directory`: Media library directory to scan

### Modes
- `--mode {tv,movie,auto}`: Content type (default: auto-detect)
- `--dry-run`: Preview deletions without actually deleting
- `--interactive`: Confirm each deletion manually

### Quality Preferences
- `--reverse`: Keep lower quality files instead of higher quality
- `--prefer-smaller`: Consider file size in quality scoring
- `--min-size-mb`: Minimum file size in MB (default: 100)

### Filtering
- `--include PATTERN [PATTERN ...]`: Include only matching files
- `--exclude PATTERN [PATTERN ...]`: Exclude matching files
- `--fuzzy-matching`: Enable fuzzy title matching for movies
- `--keep-extras`: Don't ignore extras/bonus folders

### Backup & Logging
- `--backup FILE`: Custom backup file location
- `-v, --verbose`: Enable detailed logging

## 🏗️ How It Works

### Movie Detection Strategy

1. **Folder-based detection**: Multiple video files in the same folder
2. **Title-based detection**: Same movie (title + year) across different folders  
3. **Fuzzy matching**: Similar titles with 85%+ similarity (optional)

### TV Show Detection

1. **Episode parsing**: Extracts series name and SxxExx episode identifiers
2. **Duplicate grouping**: Groups identical episodes from different sources
3. **Quality comparison**: Ranks based on resolution, source, codec, etc.

### Quality Scoring System

The quality scoring system evaluates files based on:

**Resolution** (Higher is better)
- 4K/2160p: 8 points
- 1440p/2K: 6 points  
- 1080p: 5 points
- 720p: 4 points
- 480p: 3 points

**Source Quality**
- Remux: 10 points
- BluRay: 8 points
- WEB-DL: 7 points
- WEBRip: 5 points
- HDTV: 4 points

**Video Codec**
- AV1: 5 points
- x265/HEVC: 3 points
- x264/AVC: 2 points

**Audio Quality**
- Atmos/TrueHD: 3 points
- DTS: 2 points
- AC3/AAC: 1 point

**Special Features**
- HDR/Dolby Vision: 2-3 points
- Director's Cut: 1 point
- IMAX: 2 points

## 📁 File Organization Examples

### Movies
```
Movies/
├── The Matrix (1999)/
│   ├── The Matrix (1999) [1080p BluRay].mkv    ← KEEP
│   └── The Matrix (1999) [720p WEB-DL].mkv     ← DELETE
├── Inception (2010)/
│   └── Inception.2010.2160p.UHD.BluRay.mkv
└── Inception 2010/
    └── Inception 2010 1080p WEBRip.mkv          ← DELETE (duplicate)
```

### TV Shows
```
TV Shows/
├── Breaking Bad/
│   ├── Season 1/
│   │   ├── Breaking.Bad.S01E01.1080p.BluRay.mkv    ← KEEP
│   │   └── Breaking.Bad.S01E01.720p.HDTV.mkv       ← DELETE
│   └── Season 2/
│       └── Breaking.Bad.S02E01.1080p.WEB-DL.mkv
```

## 🛡️ Safety Features

- **Dry-run mode**: Always test before actual deletion
- **Interactive confirmation**: Review each decision
- **Backup logging**: JSON file tracks all deletions
- **Sidecar file handling**: Automatically removes associated .srt, .nfo files
- **Extras protection**: Skips bonus content folders
- **Size validation**: Filters out samples and trailers

## 🔄 Recovery

If you need to recover deleted files, check the backup JSON file:

```bash
# Default backup location
cat janitorr_backup.json

# Custom backup location  
cat my_backup.json
```

The backup contains file paths, scores, and metadata for all deleted files.

## ⚠️ Important Notes

- **Always run `--dry-run` first** to preview changes
- **Backup your media** before running without dry-run mode
- The script deletes files permanently - use with caution
- Test with a small subset of your library first
- Review the quality scoring system to ensure it matches your preferences

## 🤝 Contributing

Contributions are welcome! Please feel free to submit issues, feature requests, or pull requests.

### Development Setup

```bash
git clone https://github.com/yourusername/janitorr.git
cd janitorr

# Run tests (if you add them)
python -m pytest tests/

# Format code
black janitorr.py
```

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Inspired by the need for clean, organized media libraries
- Built for the home media server community
- Designed with safety and user control in mind

---

**⚡ Pro Tip**: Start with `--interactive --dry-run` to get familiar with what Janitorr finds in your library!

## 📊 Examples Output

### Movie Duplicates Found
```
🔍 Found 3 movie duplicate groups (6 total files)
  - Folder-based: 1 groups  
  - Title-based: 1 groups
  - Fuzzy-matched: 1 groups

▶️  FOLDER: The Matrix (1999) (Keeping HIGHEST quality)
  ✅ KEEPING : (score: 15.0) The Matrix (1999) [1080p BluRay x264].mkv
  🗑️  DELETING: (score: 12.0) The Matrix (1999) [720p WEB-DL].mkv

📈 Summary: 3 files marked for deletion across 3 duplicate groups
```

### TV Episodes Found
```
🔍 Found 2 TV episodes with duplicates

▶️  Foundation S02E05 (Keeping HIGHEST quality)
  ✅ KEEPING : (score: 20.0) Foundation - S02E05 - The Sighted and the Seen - [1080p webdl h264].mkv
  🗑️  DELETING: (score: 17.0) Foundation.2021.S02E05.720p.ATVP.WEBRip.x264-GalaxyTV.mkv

📈 Summary: 1 files marked for deletion across 2 duplicate groups
```
