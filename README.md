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
