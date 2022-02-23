# lacie-d2

from [lacie-nas](http://lacie-nas.org/doku.php?id=debian_install)

```sh
wget http://ftp.debian.org/debian/dists/jessie/main/installer-armel/current/images/kirkwood/network-console/lacie/uInitrd
wget http://ftp.debian.org/debian/dists/jessie/main/installer-armel/current/images/kirkwood/network-console/lacie/d2net/uImage
dd if=uInitrd of=initrd.gz bs=64 skip=1
mkdir initrd; cd initrd
gzip -d < ../initrd.gz | cpio --extract --verbose --make-directories --no-absolute-filenames
wget https://raw.githubusercontent.com/cjuniorfox/lacie-d2/main/preseed.cfg
find . | cpio -H newc --create --verbose | gzip -9 > ../initrd.gz
cd ..
mkimage -A arm -O linux -T ramdisk -C gzip -a "0x0" -e "0x0" -n "ramdisk with preseed.cfg" -d initrd.gz uInitrd
rm -rf initrd.gz initrd
```
To install Jessie, it's needed retrieving from the host archive.debian.org

## Troubleshotting
At moment asked for kernel installation, choose "none"
Then, open the shell and do as follows:
```sh
mount -t proc proc /target/proc/
mount -t sysfs sysfs /target/sys/
chroot /target /bin/bash

cat >> /etc/apt/sources.list << EOF
deb ftp://lacie-nas.org/debian/ sid main
deb-src ftp://lacie-nas.org/debian/ sid main
EOF

gpg --keyserver pgpkeys.mit.edu --recv-key 0x0E3D4C9F7C71B58C
gpg -a --export 0E3D4C9F7C71B58C | apt-key add -

apt-get update
apt-get install flash-kernel
apt-get install u-boot-tools
apt-get install --reinstall linux-image-3.2.0-2-kirkwood
```
