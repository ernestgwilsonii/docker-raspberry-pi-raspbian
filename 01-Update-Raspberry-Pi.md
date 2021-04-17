### Determine current versions
```
# ssh to your Pi
ssh pi@192.168.1.75

# Which Raspberry Pi model do you have?
cat /proc/device-tree/model
cat /proc/cpuinfo
# See table and lookup your Revision
# REF: https://www.raspberrypi.org/documentation/hardware/raspberrypi/revision-codes/README.md
cat /proc/cpuinfo | grep 'Revision' | awk '{print $3}'

# Determine current OS version info:
cat /etc/os-release
cat /etc/debian_version
uname -m
uname -a
#sudo apt-get install -y lsb_release
lsb_release -a
lsb_release -d
hostnamectl

# Determine current bootloader version
vcgencmd bootloader_version
cat /boot/.firmware_revision

# Review current firmware?
# REF: https://github.com/raspberrypi/rpi-eeprom/tree/master/firmware
vcgencmd version
cat /etc/rpi-issue
cat /boot/issue.txt
```

### Apply standard updates
```
# Typical updates:
sudo apt update
sudo apt list --upgradable
sudo apt upgrade -y
sudo reboot

# Full upgrade:
sudo apt dist-upgrade -y
sudo reboot
```

### Upgrade the Raspberry Pi 4's firmware / bootloader for better thermals
```
# REF: https://www.jeffgeerling.com/blog/2019/upgrade-raspberry-pi-4s-firmware-bootloader-better-thermals
sudo apt update
sudo apt -y full-upgrade
sudo apt autoremove -y
# Check for eeprom update:
sudo apt install -y rpi-eeprom
sudo rpi-eeprom-update
# Perform eeprom update:
#raspi-config
#sudo rpi-eeprom-update -a
#sudo reboot

# CAUTION: Update your Raspberry Pi OS kernel and VideoCore firmware to the latest pre-release versions
# REF: https://www.raspberrypi.org/documentation/raspbian/applications/rpi-update.md
# REF: https://github.com/raspberrypi/firmware
# Backup first! https://www.raspberrypi.org/documentation/linux/filesystem/backup.md
# READ: https://www.raspberrypi.org/forums/viewtopic.php?f=29&t=288234
#sudo rpi-update
#sudo reboot
```
