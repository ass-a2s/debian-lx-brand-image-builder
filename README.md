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
╰─± ./install -r stretch -d /data/chroot -m http://httpredir.debian.org/debian/ -i lx-debian-9 -p "Debian 9 LX Brand" -D "Debian 9 64-bit lx-brand image." -u https://docs.joyent.com/images/container-native-linux

==> Installation complete!
==> lx-debian-9-20170826.tar.gz
╭─root at it-daniel in /github-ass-a2s/debian-lx-brand-image-builder on master✘✘✘ using
╰─±
```

* SmartOS System
```
[root@edv-labor-smartos /zones/ass.de/test]# ./create-lx-image -t /zones/ass.de/test/lx-debian-9-20170826.tar.gz -k 4.9.0 -m 20170803T064301Z -i lx-debian-9 -d "Debian 9 64-bit lx-brand image." -u https://docs.joyent.com/images/container-native-linux

*** Image creation complete ***
==> Image files:
lx-debian-9-20170826.zfs.gz
lx-debian-9-20170826.json

[root@edv-labor-smartos /zones/ass.de/test]#
[root@edv-labor-smartos /zones/ass.de/test]# ./create-manifest -f lx-debian-9-20170826.zfs.gz -k 4.9.0 -m 20170803T064301Z -n lx-debian-9 -v 20170826
[root@edv-labor-smartos /zones/ass.de/test]#
[root@edv-labor-smartos /zones/ass.de/test]# imgadm install -m lx-debian-9-20170826.json -f lx-debian-9-20170826.zfs.gz
Installing image ea2d7866-8a95-11e7-a114-8f4a50190eef (lx-debian-9@20170826)
zones/ea2d7866-8a95-11e7-a114-8f4a50190eef              [=================================================================================================================================>] 100% 139.72MB  21.47MB/s     6s
Installed image ea2d7866-8a95-11e7-a114-8f4a50190eef (lx-debian-9@20170826)
[root@edv-labor-smartos /zones/ass.de/test]#
```

