
# Getting Started

## Building
Configure and build the project
```bash
lb config
sudo lb build
```

This produces a image `live-image-amd64.iso`
## Prepare the Boot Device
Use Ventoy to make the USB with this command:
```bash
sudo sh Ventoy2Disk.sh -i -r 4096 /dev/sda
```
- `-i` to install
- `-r 4096` to preserve 4GiB at the bottom of the disk (for the persistence partition)
- `/dev/sda` the boot device

Then you need to make a partition in the botoom of the disk. The disk is partitioned with MBR. Partition have to be primary, number default, first sector default, last sector default. You can use:
```bash
printf 'n\np\n3\n\n\nw\n' | sudo fdisk /dev/sda
```

Then you have to format that partition with `ext4` and label "persistence":
```bash
sudo mkfs.ext4 -L persistence /dev/sda3
```

Mount `/dev/sda3` and copy the file `persistence.conf` 

Mount `/dev/sda1` (its `fat32`) and copy the built image into it