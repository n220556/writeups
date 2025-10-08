---
title: Data Compression
description: 
date: 2025-01-01
categories: [Data]
tag: [archive, data, compress, decompress, zip, tar]
---

# Data Compression

## Introduction

### 📌 What is Data Compression?

**Data compression** is the process of **reducing the size of data** so it takes up **less storage space** or **less bandwidth** during transmission.
<br>
Instead of storing/transmitting data in its raw form, compression encodes it in a more efficient way.

👉 Example
: 
- Text: `"aaaaaaabbbcc"`
- Without compression: 12 characters
- With compression (Run-Length Encoding): "a7b3c2" (6 characters)

### 📌 Why Use Data Compression?

- Save **disk/storage space**.
- Reduce **network transfer time**.
- Speed up **file backups**.
- Optimize **performance** (e.g., compressed web pages load faster).

### 📌 Types of Data Compression

#### 🔹 1. Lossless Compression

- Original data can be **perfectly restored** after decompression.
- Used when **accuracy is critical** (text, executables, databases, legal/medical files).
- Techniques:
  - **Run-Length Encoding** (RLE)
  - **Huffman Coding**
  - **Arithmetic Coding**
  - **Lempel–Ziv** (LZ77, LZ78, LZW, DEFLATE)
  - **Burrows–Wheeler Transform** (BWT)

✅ Examples of Lossless Tools/Formats
: 
- Gzip (`.gz`)
- Bzip2 (`.bz2`)
- XZ / LZMA (`.xz`)
- ZIP (`.zip`)
- 7z (`.7z`)
- PNG (images)
- FLAC (audio)

#### 🔹 2. Lossy Compression

- Some data is **discarded** to achieve higher compression.
- The decompressed data is **not identical** to the original but looks/sounds close enough.
- Used when **exact reproduction isn’t required** (images, audio, video).
- Techniques:
  - **Transform Coding** (DCT in JPEG, MP3, MPEG)
  - **Quantization** (reducing precision of values)
  - **Perceptual Coding** (remove data not noticeable to human senses)

✅ Examples of Lossy Formats
: 
- JPEG (images)
- MP3, AAC (audio)
- MP4, H.264, H.265 (video)
- WebP, HEIC (modern image formats)



✅ In short
: 
- Lossless = exact recovery, used for text/data.
- Lossy = smaller files, used for multimedia.


## Tools 🧰

- In OTW we mainly used bzip2, gzip and tar


### gzip tool

#### 📌 What is gzip?

- `gzip` (**GNU zip**) is a **lossless compression tool**.
- Uses **DEFLATE (LZ77 + Huffman coding)**.
- **Replaces** the input file with a compressed version (`file.txt → file.txt.gz`).
- Companion tools
: 
  - `gunzip` = decompression (`gzip -d`)
  - `zcat` = **view compressed contents**

#### 📌 Syntax
```bash
gzip [options] file...
```

#### 📌 Options with Explanations + Examples

1. 🔹 `-a`, `--ascii`
: 
- Convert text file line endings between **MS-DOS CRLF** and *Unix LF*.
- 👉 Rarely used today.
```bash
gzip -a dosfile.txt
```

1. 🔹 `-c`, `--stdout`, `--to-stdout`
: 
- Write output to **stdout** instead of replacing file.
- Useful for redirecting/piping.
```bash
gzip -c file.txt > file.txt.gz   # compress but keep original
gzip -dc file.txt.gz             # decompress to screen
```

1. 🔹 `-d`, `--decompress`, `--uncompress`
: 
- Decompress (same as `gunzip`).
```bash
gzip -d file.txt.gz
```

1. 🔹 `-f`, `--force`
: 
- Force compression/decompression, even if file already compressed or exists.
```bash
gzip -f file.txt   # overwrite if file.txt.gz already exists
```

1. 🔹 `-h`, `--help`
: 
- Show help.
```bash
gzip -h
```

1. 🔹 `-k`, `--keep`
: 
- Keep original files (do not delete).
```bash
gzip -k file.txt   # keeps file.txt and creates file.txt.gz
```

1. 🔹 `-l`, `--list`
: 
- Show compression statistics.
```bash
gzip -l file.txt.gz
```
```markdown
Output:

         compressed        uncompressed  ratio uncompressed_name
                 34                   64  46.9% file.txt
```

1. 🔹 -L, `--license`
: 
- Show GNU license.
```bash
gzip -L
```

1. 🔹 `-n`, `--no-name`
: 
- Do not save original filename and timestamp in .`gz` header.
```bash
gzip -n file.txt
```

1. 🔹 `-N`, `--name`
: 
- Save original filename + timestamp in `.gz` header (default).
```bash
gzip -N file.txt
```

1. 🔹 `-q`, `--quiet`
: 
- Suppress warnings.
```bash
gzip -q file.txt
```

1. 🔹 `-r`, `--recursive`
: 
- Compress directories recursively.
```bash
gzip -r myfolder/
```

1. 🔹 `-S .suf`, `--suffix .suf`
: 
- Use custom suffix instead of `.gz`.
```bash
gzip -S .zz file.txt   # produces file.txt.zz
```

1. 🔹 `-t`, `--test`
: 
- Test integrity of compressed file.
```bash
gzip -t file.txt.gz
echo $?   # 0 = OK, nonzero = corrupted
```

1. 🔹 `-v`, `--verbose`
: 
- Show file names and compression ratio.
```bash
gzip -v file.txt
```
Output:
```text
file.txt:  35.2% -- replaced with file.txt.gz
```

1. 🔹 `-V`, `--version`
: 
- Show version.
```bash
gzip -V
```

1. 🔹 `-1` … `-9`
: 
- Set compression level:
  - `-1` = fastest, least compression
  - `-9` = slowest, best compression (default = -6)
```bash
gzip -1 file.txt   # fast compression
gzip -9 file.txt   # best compression
```

#### 📌 Practical Workflows

✅ Compress a file
: 
```bash
gzip file.txt
```

✅ Decompress a file
: 
```bash
gzip -d file.txt.gz
gunzip file.txt.gz
```

✅ Keep original + compress
: 
```bash
gzip -k file.txt
```

✅ Compress recursively
: 
```bash
gzip -rv myfolder/
```

✅ Test integrity
: 
```bash
gzip -t file.txt.gz
```

✅ Show stats
: 
```bash
gzip -l file.txt.gz
```

#### 📌 Summary

- gzip is for single-file compression.
- Use tar with gzip for directories (.tar.gz).
- Most useful options in real life:
  - `-d` (decompress)
  - `-k` (keep original)
  - `-c` (stdout)
  - `-v` (verbose)
  - `-1` … `-9` (compression level)




### bzip2 tool

#### 📌 What is bzip2?

- `bzip2` is a **lossless compression utility** for single files.
- Uses the **Burrows–Wheeler Transform (BWT) + Huffman coding**.
- Produces **smaller files than** `gzip`, but slower.
- Default output extension: `.bz2`
- Companion tools:
  - `bunzip2` → decompress (same as `bzip2 -d`)
  - `bzcat` → view compressed file

#### 📌 Syntax
```bash
bzip2 [options] file...
```

#### 📌 Options (with Explanation + Examples)

1. 🔹 `-c`, `--stdout`
: 
- Write output to stdout instead of replacing file.
- Useful for redirection or piping.
```bash
bzip2 -c file.txt > file.txt.bz2
bunzip2 -c file.txt.bz2 > file.txt
```

1. 🔹 `-d`, `--decompress`
: 
- Decompress a `.bz2` file (same as `bunzip2`).
```bash
bzip2 -d file.txt.bz2
bunzip2 file.txt.bz2
```

1. 🔹 `-z`, `--compress`
: 
- Force compression mode (default, usually not needed).
```bash
bzip2 -z file.txt
```

1. 🔹 `-t`, `--test`
: 
- Test integrity of a compressed file.
```bash
bzip2 -t file.txt.bz2
echo $?   # 0 means OK
```

1. 🔹 `-k`, `--keep`
: 
- Keep original file (don’t delete after compression).
```bash
bzip2 -k file.txt   # keeps file.txt + creates file.txt.bz2
```

1. 🔹 `-f`, `--force`
: 
- Force overwrite if file exists, compress/decompress even if not needed.
```bash
bzip2 -f file.txt
```

1. 🔹 `-q`, `--quiet`
: 
- Suppress warnings.
```bash
bzip2 -q file.txt
```

1. 🔹 `-v`, `--verbose`
: 
- Show compression ratio for each file.
```bash
bzip2 -v file.txt
```
Output : 
```text
  file.txt:  37.2% -- replaced with file.txt.bz2
```

1. 🔹 `-s`, `--small`
: 
- Use less memory, but slower (good for very large files on low-RAM systems).
```bash
bzip2 -s bigfile.log
```

1. 🔹 `-1` … `-9`
: 
- Set compression level:
  - `-1` = fastest, least compression
  - `-9` = slowest, best compression (default)
```bash
bzip2 -1 file.txt   # faster
bzip2 -9 file.txt   # maximum compression (default)
```

#### 📌 Companion Tools

- `bunzip2` → same as `bzip2 -d`
```bash
bunzip2 file.txt.bz2
```
- bzcat → view contents without extracting
```bash
bzcat file.txt.bz2
```

#### 📌 Practical Examples

✅ Compress a file
: 
```bash
bzip2 file.txt
```
→ Creates file.txt.bz2, deletes file.txt.

✅ Decompress a file
: 
```bash
bzip2 -d file.txt.bz2
```

✅ Keep original
: 
```bash
bzip2 -k file.txt
```

✅ Test integrity
: 
```bash
bzip2 -t file.txt.bz2
```

✅ Show compression stats
: 
```bash
bzip2 -v file.txt
```

#### 📌 Summary

- `bzip2` = smaller but slower than `gzip`.
- Best when you want **higher compression** and speed isn’t critical.
- Most useful options in practice:
  - `-d` (decompress)
  - `-k` (keep original)
  - `-v` (verbose stats)
  - `-1` … `-9` (control compression level)
  - `-s` (low memory mode)



### tar tool

#### 📌 What is tar?

- `tar` = **Tape Archive**
- Used to **collect multiple files/directories into a single archive file**.
- By itself, `tar` does **not compress**, it only bundles.
- Compression is done with:
  - `gzip` → `.tar.gz` or `.tgz`
  - `bzip2` → `.tar.bz2`
  - `xz` → `.tar.xz`

#### 📌 Syntax
```bash
tar [options] [archive-file] [file...]
```

#### 📌 Main Options

Tar options can be combined (like `-xvf`) or written separately (`-x` `-v` `-f`).

1. 🔹 `-c`, `--create`
: 
- Create a new archive.
```bash
tar -cf archive.tar file1 file2 dir/
```

1. 🔹 `-x`, `--extract`, `--get`
: 
- Extract files from an archive.
```bash
tar -xf archive.tar
```

1. 🔹 `-t`, `--list`
: 
- List contents of an archive.
```bash
tar -tf archive.tar
```

1. 🔹 `-f`, `--file`
: 
- Specify the archive file name (must always follow immediately).
```bash
tar -cf backup.tar mydir/
tar -xf backup.tar
```
⚠️ Without `-f`, tar expects input/output from tape devices (old UNIX behavior).

1. 🔹 `-v`, `--verbose`
: 
- Show progress — lists files being archived or extracted.
```bash
tar -cvf archive.tar mydir/
tar -xvf archive.tar
```

1. 🔹 `-z`, `--gzip`, `--gunzip`, `--ungzip`
: 
- Compress/decompress archive with gzip.
```bash
tar -czf archive.tar.gz mydir/
tar -xzf archive.tar.gz
```

1. 🔹 `-j`, `--bzip2`
: 
- Compress/decompress using bzip2.
```bash
tar -cjf archive.tar.bz2 mydir/
tar -xjf archive.tar.bz2
```

1. 🔹 `-J`, `--xz`
: 
- Compress/decompress using xz.
```bash
tar -cJf archive.tar.xz mydir/
tar -xJf archive.tar.xz
```

1. 🔹 -A, --concatenate
: 
- Append a tar archive to another tar archive.
```bash
tar -Af main.tar extra.tar
```

1. 🔹 `-r`, `--append`
: 
- Append files to an existing uncompressed archive.
```bash
tar -rf archive.tar newfile.txt
```

1. 🔹 `-u`, `--update`
: 
- Append files only if they are newer than existing ones.
```bash
tar -uf archive.tar updated.txt
```

1. 🔹 -d, --diff, --compare
: 
- Compare archive contents with filesystem.
```bash
tar -df archive.tar
```

1. 🔹 -C dir
: 
- Change to directory before processing.
```bash
tar -cf archive.tar -C /home/user documents/
```
(Archives `/home/user/documents` as `documents/`)

1. 🔹 `-p`, `--preserve-permissions`
: 
- Preserve file permissions.
```bash
tar -cpf archive.tar mydir/
```

1. 🔹 `--same-owner`
: 
- Extract files with original ownership. Useful with root.
```bash
tar --same-owner -xvf archive.tar
```

1. 🔹 `--remove-files`
: 
- Remove files after adding them to archive.
```bash
tar -cf backup.tar *.log --remove-files
```

1. 🔹 `--exclude=PATTERN`
: 
- Exclude files matching pattern.
```bash
tar -czf archive.tar.gz mydir/ --exclude="*.tmp"
```

1. 🔹 `--strip-components=N`
: 
- Remove first N directory levels when extracting.
```bash
tar -xzf archive.tar.gz --strip-components=1
```

1. 🔹 `--wildcards`
: 
- Enable shell-style wildcards in extraction.
```bash
tar -xvf archive.tar "*.txt"
```

1. 🔹 `--checkpoint`
: 
- Show progress checkpoints during large operations.
```bash
tar -cvf archive.tar bigdir/ --checkpoint
```

1. 🔹 `--totals`
: 
- Show total bytes written after archive.
```bash
tar -cvf archive.tar mydir/ --totals
```

#### 📌 Common Workflows

✅ Create archive
: 
```bash
tar -cvf files.tar file1 file2 dir/
```

✅ Extract archive
: 
```bash
tar -xvf files.tar
```

✅ Create compressed archive
: 
```bash
tar -czvf files.tar.gz dir/
tar -cjf files.tar.bz2 dir/
tar -cJvf files.tar.xz dir/
```

✅ Extract compressed archive
: 
```bash
tar -xzvf files.tar.gz
tar -xjvf files.tar.bz2
tar -xJvf files.tar.xz
```

✅ List archive contents
: 
```
tar -tvf files.tar
```

#### 📌 Why `-xvf` instead of just `-x`?

- `-x` → extract (but tar doesn’t know which file)
- `-f` → specify archive file
- `-v` → just shows progress
<br>
So: 
```bash
tar -x file.tar    # works if shell interprets `file.tar` as filename
tar -xvf file.tar  # safer, explicit, standard practice
```

#### 📌 Summary

- Essential options:
  - `-c` (create)
  - `-x` (extract)
  - `-t` (list)
  - `-f` (archive file)
  - `-v` (verbose)
  - `-z`/`-j`/`-J` (gzip/bzip2/xz compression)
- **Advanced options**: `-r`, `-u`, `-d`, `--exclude`, `--strip-components`

## 🎨 Summary
- we can compress files as `bzip2` or `gzip` by using `tar`