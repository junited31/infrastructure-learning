# Bandit Learning: Linux Permissions, Files, and Text Inspection

## Concept Overview

Bandit reinforced how Linux permissions, ownership, hidden files, unusual filenames, file metadata, and text-processing tools affect access to data. The focus was not memorizing challenge answers, but learning how to inspect a system carefully and explain why a command works.

This topic is directly connected to infrastructure operations because administrators frequently need to find configuration files, inspect logs, handle unusual file names, validate permissions, and extract a useful line from a large amount of output.

## Commands Used

```bash
ls -la
file ./filename
cat ./filename
cat "./filename with spaces"
cat -- "--filename-starting-with-dash"
cat ./filename\ with\ spaces
find . -type f
find . -readable
find . -size 1033c
find / -type f -size 33c -user target-user -group target-group 2>/dev/null
stat ./filename
chmod
grep "^pattern" data.txt
sort data.txt | uniq -u
strings data.bin | grep "pattern"
base64 -d data.txt
tr "N-ZA-M" "A-Z"
xxd -r input.hex > output.bin
gzip -d file.gz
bzip2 -d file.bz2
tar -xvf archive.tar
```

## What I Learned

- Hidden files are normal filesystem entries with names beginning with `.`.
- Filenames can contain spaces, dashes, and special characters, so quoting and `./` prefixes matter.
- `--` tells many commands to stop parsing options, which is useful when a filename begins with `-`.
- Escaping spaces with `\` and quoting paths both prevent the shell from splitting one filename into multiple arguments.
- `find` is useful when file names are unknown but metadata is known.
- `-type f`, `-size`, `-user`, `-group`, `-readable`, and `! -executable` allow targeted file discovery.
- File type should be confirmed before assuming content format.
- `file` helps identify whether content is text, compressed data, an archive, or executable data.
- `strings` extracts readable text from binary-looking data.
- `sort | uniq -u` works because `uniq` only compares adjacent lines.
- `grep "^pattern"` anchors a search to the beginning of a line.
- `base64 -d`, `tr`, `xxd -r`, and decompression tools are useful for decoding or transforming stored data.
- Shell globbing such as `./*` can miss hidden files, so `ls -la` and `find` are safer for complete inspection.
- Permission errors should be inspected through ownership, mode bits, and parent directories.

## Operational Notes

### Handling Special Filenames

Files with spaces or leading dashes should be handled deliberately:

```bash
cat "./file with spaces"
cat -- "--file-starting-with-dash"
cat ./file\ with\ spaces
```

This prevents accidental option parsing or argument splitting.

### Finding Files by Metadata

When the filename is unknown, search by file attributes:

```bash
find . -type f -size 1033c ! -executable -readable -exec file {} +
```

For system-wide searches, redirect noisy permission errors:

```bash
find / -type f -user target-user -group target-group -size 33c 2>/dev/null
```

`2>/dev/null` redirects standard error, which keeps expected permission-denied noise out of the result.

### Inspecting Encoded or Compressed Data

Infrastructure files are not always plain text. Before changing extensions or decompressing, identify the file type:

```bash
file data
xxd -r data.hex > data.bin
gzip -d data.gz
bzip2 -d data.bz2
tar -xvf data.tar
```

This workflow is useful when investigating backups, log bundles, support archives, and files copied from another system.

## Real-world Infrastructure Relevance

These skills apply to configuration discovery, log inspection, permission troubleshooting, backup review, incident response, and safe handling of files with unusual names on production systems. They also support careful root-cause analysis because the same tools help answer practical questions:

- Can the service account read this file?
- Is this file really text, or is it compressed or binary?
- Did a hidden file or unusual filename get missed?
- Is the useful line inside a large file or command output?
- Are errors coming from expected permission boundaries?

## Interview Questions

- How do you list hidden files in a directory?
- Why might `cat -file` behave differently from `cat ./-file`?
- How would you find readable files owned by a specific user?
- What command shows detailed file metadata?
- How can parent directory permissions block access to a readable file?
- Why does `uniq` usually need sorted input when looking for duplicate or unique lines?
- How would you identify whether a file is compressed, text, or binary?
- What does `2>/dev/null` do, and when should it be used carefully?
