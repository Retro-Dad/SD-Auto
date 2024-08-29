# SD-Auto
Automounting script for Valve Steam Deck
Script to Auto-Mount NTFS, BTRFS & exFat SDCards, External USB Drives (or SSD Docks) & Internal Partitions (If you Dual-Boot) on the Steam Deck

NTFS & BTRFS Partitions containing a SteamLibrary at root level or in a folder named SteamLibrary will automatically be added to Steam, exFAT isn't supported as a SteamLibrary but will be Mounted for use with other Launchers or for Media/ROMs etc.

This script is a mirror of SteamOS' automounting script found at /usr/lib/hwsupport/steamos-automount.sh with support for additional file formats

Additional RegEx has been added to the rules to allow he mounting of "Full Disk" Formatted drives (eg ones that don't have a partitions table) so even drives that are eg sda or mmcblk0 as well as sda1 or mmcblk0p1 can be mounted.

SteamOS's rule for this lives at /usr/lib/udev/rules.d/99-steamos-automount.rules and because SteamOS has a Read-Only File System, files in /usr/ cannot be changed without removing the Read-Onlyness, however systemd rules can be overwritten due to how systemd prioritieses directories, so by adding a rule with the same name in /etc/udev/rules.d/ we can override the rule without making changes to SteamOS.

a udev rule is added to /etc/udev/rules.d/99-steamos-automount.rules which takes priority over /usr/lib/udev/rules.d/99-steamos-automount.rules this then calls systemd /etc/systemd/system/external-drive-mount@[sda|sda1|sda2|sdd1|etc].service that then runs /home/deck/.local/share/scawp/SDMED/automount.sh to Auto Mount any supported SD/External USB/Internal Partitions.

/etc/fstab is not required for mounting in this way, (however if a Device has an fstab entry this script will still work)
