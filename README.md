# LinuxOpenMediaVault
LinuxOpenMediaVault


## GENERICS
```
diskutil list
diskutil umountDisk /dev/disk2
sudo dd bs=1m if=2018-11-13-raspbian-stretch-lite.img of=/dev/disk2
```
```
apt-get update
apt-get install -y ntfs-3g exfat-fuse
```

## OPEN MEDIA VAULT
```
apt-get update
apt-get install --yes apt-transport-https
```
```
cat <<EOF >> /etc/apt/sources.list.d/openmediavault.list
deb https://packages.openmediavault.org/public arrakis main
# deb https://downloads.sourceforge.net/project/openmediavault/packages arrakis main
## Uncomment the following line to add software from the proposed repository.
# deb https://packages.openmediavault.org/public arrakis-proposed main
# deb https://downloads.sourceforge.net/project/openmediavault/packages arrakis-proposed main
## This software is not part of OpenMediaVault, but is offered by third-party
## developers as a service to OpenMediaVault users.
# deb https://packages.openmediavault.org/public arrakis partner
# deb https://downloads.sourceforge.net/project/openmediavault/packages arrakis partner
EOF
```
```
export LANG=C
export DEBIAN_FRONTEND=noninteractive
export APT_LISTCHANGES_FRONTEND=none
apt-get update
apt-get --allow-unauthenticated install openmediavault-keyring
apt-get update
apt-get --yes --auto-remove --show-upgraded \
    --allow-downgrades --allow-change-held-packages \
    --no-install-recommends \
    --option Dpkg::Options::="--force-confdef" \
    --option DPkg::Options::="--force-confold" \
    install postfix openmediavault
```
```
# Initialize the system and database.
omv-initsystem
```

### Extras
```
wget http://omv-extras.org/openmediavault-omvextrasorg_latest_all4.deb
dpkg -i openmediavault-omvextrasorg_latest_all4.deb 
apt-get install -f
apt-get update
```

### Helper
```
omv-firstaid
```

## RAID MIRROR USB (2 disk mirror RAID)
```
apt install btrfs-tools
parted -s /dev/sda mklabel gpt 
parted -s /dev/sdb mklabel gpt 
partprobe 
mkfs.btrfs -f -d raid1 -m raid1 -L BTRFS_LINEAR /dev/sd[ab]
```

## CUPS
```
sudo apt-get install -y cups libcups2-dev libcupsimage2 libcupsimage2-dev
```
```
sudo usermod -aG lpadmin administrator
sudo cupsctl --remote-admin
```
```
/etc/cups/cupsd.conf

<Location />
  # Allow remote administration...
  Order allow,deny
  Allow @LOCAL
  Allow all
</Location>
```
```
sudo /etc/init.d/cups restart
```
```
CUPS >> http://xxx.xxx.xxx.xxx:631
```

## DRIVERS Brother DCP-7065DN
```
sudo apt-get install -y build-essential
sudo apt-get install -y make automake cmake git subversion checkinstall unzip
```
```
git clone https://github.com/pdewacht/brlaser
```
```
cmake .
make
sudo make install
```

## AIRPRINT
```
apt-get install avahi-discover cups cups-pdf python-cups
apt-get install hplip
```
```
/etc/init.d/avahi-daemon start
mkdir /opt/airprint
cd /opt/airprint
wget -O airprint-generate.py --no-check-certificate https://raw.github.com/tjfontaine/airprint-generate/master/airprint-generate.py
chmod +x airprint-generate.py
./airprint-generate.py -d /etc/avahi/services
/etc/init.d/avahi-daemon restart
ls /etc/avahi/services/
```
