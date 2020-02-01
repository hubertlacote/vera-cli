# vera-cli

**A set of command-line tools to make it easier to use VeraCrypt for password-protected volumes.**

## Why ([![start with why](https://img.shields.io/badge/start%20with-why%3F-brightgreen.svg?style=flat)](http://www.ted.com/talks/simon_sinek_how_great_leaders_inspire_action))

If you decided to use password-protected volumes, this set of tools:

- avoids you from having to find where your encrypted disk is attached (e.g. is it '/dev/sdb1'?) - just pass the disk vendor/model name,
- allows you to quickly mount / unmount volumes without typing passwords - just create small wrapper scripts in your PATH (assuming your hard drive is encrypted...),
- allows you to execute scripts (e.g. backup scripts) on your password-protected volumes,
- avoids unnecessary backups by mounting by default volumes as read-only - pass `-w` to mount as read-write.

## Requirements

The following tools are used:

- [VeraCrypt](https://www.veracrypt.fr/en/Downloads.html),
- `lsblk` to lookup disks by vendor name / disk model (installed by default on Ubuntu).

## Usage

To mount password-protected volumes or encrypted file-containers, use `v-mount`:

```bash
# Mount a disk as read-write using the disk vendor name and use a default veracrypt mount point
v-mount -p "password" -V "Kingston" -w

# Mount a disk as read-write using the disk model and use a default veracrypt mount point
v-mount -p "password" -M "Samsung T5" -w

# Mount a disk as read-write by specifying the block device and the mount point
v-mount -p "password" -v /dev/sdb1 -m /mnt/data -w

# Mount an encrypted file-container as read-write and specify the mount point
v-mount -p "password" -v volume.tc -m /mnt/data -w

# See more options
v-mount -h
```

To mount password-protected volumes or encrypted file-containers and browse them, use `v-imount`.
Press Ctrl+D when you are done and the volume will be unmounted automatically.
For instance, you can create a wrapper script in your path to quickly mount your
encrypted USB drive:

```bash
#!/bin/bash

v-imount -M "Samsung T5" -p "password" $@
```

To automatically backup to your encrypted volumes / file-containers, create a wrapper
script in your path that you can execute easily whenever you want to perform a backup. e.g. with the script
[examples/backup](examples/backup) that backs up the home folder:

```bash
#! /bin/bash

v-mount-and-exec -V Kingston -p "password" -w -- "backup $@"
```
