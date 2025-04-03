
[Arch Linux - Downloads](https://archlinux.org/download/)
[Rufus - Создать загрузочный USB-диск? Это просто](https://rufus.ie/ru/)

Download `.iso` file and install on USB flash drive with `Rufus`

If Bios ver is frash with UEFI craete `GPT` Partition, `MBR` another

SCRENNSHOT

Reboot into USB flash drive and choose `Arch Install`

`lsblk` - list of partitions in hard drive

`sda` - hard drive with **SCSI** (Small Computer System Interface) (old devices)
`nvme` - device with `NVME` support (new ssd\`s)

Choose correct device to install system on it

`cfdisk` /dev/\_\_name__ 

You need to create 3 partitions for EFI (boot), SWAP, Filesystem

For EFI allocate 600M and choose Type - Efi Boot
For SWAP allocate 2G  and choose Type - Swap filesystem
For Filesystem all what you need

With `lsblk` check all memory sizes for device and then format partitions

```bash
mkfs.vfat -F 32 /dev/__name__p_Number # write EFI Number part

mkswap /dev/__name__p_NUMBER # write SWAP Number part

mkfs.ext4 /dev/__number__p_NUMBER # write filesystem Number part of your device
```

For fillesystem you can use another disk format for Filesystem if you want
- ext4 (default)
- ZFS
- XFS
- Btrfs
- Reiser4

Next you need to mount 2 points for install and turn on SWAP
Hint: You can usually use TAB to autofill location

```bash
mkdir /mnt/archisntall
mount /dev/__name__p_NUMBER /mnt/archinstall # write filesystem Number part of your device

mkdir /mnt/archinstall/boot
mount /dev/__name__p_NUMBER /mnt/archinstall/boot # write EFI Number part

swapon /dev/__name__p_NUMBER # write SWAP Number part
```

Then run `archinstall`

!For `Disk configuration` use `Pre-Mounted Configuration` and insert `/mnt/archinstall`

For dual boot for `GRUB`

```bash
sudo pacman -S os-prober

sudo nano /etc/defualt/grub
```

Make `#GRUB_DISABLE_OS_PROBER=false` -> `GRUB_DISABLE_OS_PROBER=false` (uncomment)

```bash
os-prober # you will see all .efi files include Windows 

sudo grub-mkconfig -o /boot/grub/grub.cfg
```


If you see this error 

`Timed out waiting for device /dev/tpmrm0`

Run `systemctl mask dev-tpmrm0.device`