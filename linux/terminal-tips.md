# Show full year in the result of `ls` command
Acording to this [answer](https://unix.stackexchange.com/a/415262/30988), `ls -l` will display month, day and **year** - *since*, according to the BSD man page: 

If the modification time of the file is more than 6 months in the past or future, then the year of the last modification is displayed in place of the hour and minute fields.

So, to make sure that year will always be shown, use:

`ls -l --time-style=long-iso` (GNU/Linux)

`ls -lT` will display complete time information in BSD (MacOS)

# Select and copy text in Tmux
In some settings, selecting text in tmux is disable. However, we can press `CMD + R` on MacOS to select any piece of text and `CMD + C` to copy it.

In particular om MacOS, press `Fn` key with left mouse if using `Terminal.app`. Otherwise, press `Opt` key with left mouse if using `iTerms.app`. Note: not press `Shift` key. 

# Delete the content or empty a file
The [simple and fast ways](https://stackoverflow.com/questions/22402054/what-is-the-simplest-way-to-delete-the-contents-of-a-file-in-bash) are 
```bash
> file.txt
true > file.txt # a slight variation

truncate -s 0 file.txt
truncate --size=0 file.txt

cat /dev/null > file.txt
```
# Sort `du -h` output by size
When reading [this question](https://serverfault.com/questions/62411/how-can-i-sort-du-h-output-by-size), I summarise all the commands below.
```bash
du -hs * | sort -h

# for Mac OS X, if the command above doesn't work
brew install coreutils
du -hs * | gsort -h
# sort files by size in MB on Unix-based systems
du --block-size=MiB --max-depth=1 path | sort -n
```
From the `sort` [manual](https://linux.die.net/man/1/sort):
```text
-h, --human-numeric-sort    compare human readable numbers (e.g., 2K 1G)
```

# Check the size of disk free
Using `fdisk -l`, noticing that the `-l` required option is crucial.
```bash
sudo fdisk -l 
Disk /dev/nvme0n1: 600 GiB, 644245094400 bytes, 1258291200 sectors
Disk model: Amazon Elastic Block Store              
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: D209C89E-EA5E-4FBD-B161-B461CCE297E0

Device          Start        End    Sectors   Size Type
/dev/nvme0n1p1   2048     411647     409600   200M EFI System
/dev/nvme0n1p2 411648 1258291166 1257879519 599.8G Linux filesystem
```
Using `df -h` (Disk Free - Human-readable) - shows the capacity and usage of *mounted* filesystems, which is useful if you want to know the size of a specific partition.
```bash
df -h 
Filesystem      Size  Used Avail Use% Mounted on
/dev/nvme0n1p2  600G   18G  583G   3% /
devtmpfs        4.0M     0  4.0M   0% /dev
tmpfs            16G     0   16G   0% /dev/shm
efivarfs        128K  3.3K  125K   3% /sys/firmware/efi/efivars
tmpfs           6.2G   25M  6.2G   1% /run
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-journald.service
/dev/nvme0n1p1  200M  8.7M  192M   5% /boot/efi
tmpfs           1.0M     0  1.0M   0% /run/credentials/serial-getty@ttyS0.service
tmpfs           1.0M     0  1.0M   0% /run/credentials/getty@tty1.service
tmpfs           3.1G   16K  3.1G   1% /run/user/1000
```
Using `lsblk` (List Block Devices) - shows all block devices (disks) and their sizes in a nice tree format.
```bash
lsblk
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
nvme0n1     259:0    0   600G  0 disk 
├─nvme0n1p1 259:1    0   200M  0 part /boot/efi
└─nvme0n1p2 259:2    0 599.8G  0 part /
```
Voilà!

# Exclude `.DS_store files from tar.gz
When archiving a directory on MacOS, these files are often included in the result archive file. To exclude them out the archive file, adding the following option.
```bash
tar -zcv --exclude='.DS_Store' -f file.tar.gz folder/
# or
tar --disable-copyfile --exclude='.DS_Store' -cvzf file.tar.gz folder/
```
More generally, using `--exclude='./upload/folder1 --exclude='./docs/largefile.img` to exclude any directories.
```bash
tar --exclude='./folder' --exclude='./upload/folder2' -zcvf /backup/filename.tgz .
```