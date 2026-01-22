# SpongeBob SquarePants MKV Auto-Splitter (Lossless)

This script automatically splits **merged SpongeBob SquarePants episode files**
(e.g. two ~11-minute episodes combined into one ~22-minute MKV)
into individual episode files without re-encoding.

It uses FFmpeg’s `blackdetect` filter to find the black pause between episodes
and splits the file losslessly at that point.

Built and tested on **macOS (zsh, default Bash 3.2)**. It should behave the same on Linux.

---

## What this script does

For each `.mkv` file in a folder:

1. Scans the video for black screen segments
2. Finds the best black segment near the midpoint (~11 minutes)
3. Splits the file into **two MKVs** using `-c copy` (no quality loss)
4. Renames the outputs to **Plex-friendly episode files**, preserving quality tags

---

## Expected input filename format

The script is tuned for SpongeBob releases named like:

SpongeBob SquarePants (1999) - S02E01-E02 - Something Smells & Bossy Boots (1080p AMZN WEB-DL x265 RCVR).mkv

---

## Output filename format (what you get)

From the example above, the script outputs:

SpongeBob SquarePants (1999) - S02E01 - Something Smells (1080p AMZN WEB-DL x265 RCVR).mkv  
SpongeBob SquarePants (1999) - S02E02 - Bossy Boots (1080p AMZN WEB-DL x265 RCVR).mkv

- Episode numbers are correct  
- Titles are preserved  
- Quality tag is preserved  
- Plex-friendly naming

If a filename doesn’t match the expected pattern, the script safely falls back to:

Original Filename - part1.mkv  
Original Filename - part2.mkv

---

## Requirements (macOS)

### 1) Homebrew
If `brew` is not installed:

/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

Follow the “Next steps” Homebrew prints (usually adds brew to PATH).

Verify:
brew --version

---

### 2) FFmpeg
Install via Homebrew:

brew install ffmpeg

Verify:
ffmpeg -version

---

## Files created by the script

- Output folder:
  <input folder>/Split/

- Log file:
  <output folder>/split_log.txt

The original MKVs are never modified.

---

## Making the script executable (first time only)

From the folder containing the script:

chmod +x spongebob_mkv_splitter.sh

---

## How to run the script

IMPORTANT: You must be in the folder that contains the script, or use the full path.

If the script lives here:
~/Desktop/spongebob_mkv_splitter.sh

Either:
cd ~/Desktop

OR run using the full path.

---

### Dry run (recommended first)

Scans files and prints chosen split times **without writing files**:

./spongebob_mkv_splitter.sh "PATH TO SEASON FOLDER" --season 01 --dry-run

---

### Run for real

./spongebob_mkv_splitter.sh "PATH TO SEASON FOLDER" --season 01

Outputs will be written to:

/Season 01/Split/

---

## Cancelling / stopping safely

You can stop the script at any time with:

Ctrl + C

- In --dry-run, nothing is written
- In real mode, completed files remain valid

---

## Performance expectations

This script decodes the entire video to detect black screens.

Typical runtime on macOS:
- 1080p x265: ~30–90 seconds per file

---

## Notes

- This script is intentionally SpongeBob-specific
- It assumes:
  - ~22-minute merged files
  - a hard black pause between episodes
  - filenames with SxxEyy-Ezz and “Title A & Title B”
