# Debian lx-brand Image Builder

[![Build Status](https://travis-ci.org/joyent/debian-lx-brand-image-builder.svg?branch=master)](https://travis-ci.org/joyent/debian-lx-brand-image-builder) (shecllcheck)

This is a collection of scripts used for creating an lx-brand Debian image.

## Requirements

In order to use these scripts you'll need:

- Debian running in a VM or bare metal (required for the `install` script)
- debootstrap: `apt-get install -y debootstrap`
- git: `apt-get install -y git`
- A SmartOS (or SDC headnode) install (required for the `create-lx-image` script)

## Usage

1. Run `./install -d <chroot> -m <mirror> -i <image name> -p <proper name> -u <image docs` under Debian to install Debian 7 in a given directory. This will create a tarball of the installation in your working directory (named `<image name>-<YYMMDD>.tar.gz`). See `./install -h` for detailed usage.
2. Copy the tarball to a SmartOS machine or SDC headnode and run `./create-lx-image -t /full/path/to/<image name>-<YYMMDD>.tar.gz` (substituting the name of your tar file). This will create the image file and manifest.

Example
=======

* Debian System
```
╭─root at it-daniel in /github-ass-a2s/debian-lx-brand-image-builder on master✔ using
╰─± ./install -r stretch -d /data/chroot -m http://httpredir.debian.org/debian/ -i ass-debian-9 -p "Debian 9 LX Brand" -D "Debian 9 64-bit lx-brand image." -u https://docs.joyent.com/images/container-native-linux

==> Installation complete!
==> ass-debian-9-20170827.tar.gz
╭─root at it-daniel in /github-ass-a2s/debian-lx-brand-image-builder on master✘✘✘ using
╰─±
```

* SmartOS System
```
[root@edv-labor-smartos /zones/ass.de/test]# ./create-lx-image -t /zones/ass.de/test/ass-debian-9-20170827.tar.gz -k 4.9.0 -m 20170803T064301Z -i ass-debian-9 -d "Debian 9 64-bit lx-brand image." -u https://docs.joyent.com/images/container-native-linux

*** Image creation complete ***
==> Image files:
ass-debian-9-20170827.zfs.gz
ass-debian-9-20170827.json

[root@edv-labor-smartos /zones/ass.de/test]#
[root@edv-labor-smartos /zones/ass.de/test]# ./create-manifest -f ass-debian-9-20170827.zfs.gz -k 4.9.0 -m 20170803T064301Z -n ass-debian-9 -v 20170827
[root@edv-labor-smartos /zones/ass.de/test]#
[root@edv-labor-smartos /zones/ass.de/test]# imgadm install -m ass-debian-9-20170827.json -f ass-debian-9-20170827.zfs.gz
Installing image 1c88885e-8b46-11e7-af26-8b92b4effc78 (ass-debian-9@20170827)
zones/1c88885e-8b46-11e7-af26-8b92b4effc78              [=================================================================================================================================>] 100% 139.72MB  21.47MB/s     6s
Installed image 1c88885e-8b46-11e7-af26-8b92b4effc78 (ass-debian-9@20170827)
[root@edv-labor-smartos /zones/ass.de/test]#
```

