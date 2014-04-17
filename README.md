mkinitcpio-ykfde
================

**Full disk encryption with Yubikey (Yubico key)**

This allows to automatically unlock a LUKS encrypted hard disk from `systemd`-
enabled initramfs.

Requirements
------------

To compile and use yubico full disk encryption you need:

* [yubikey-personalization](https://github.com/Yubico/yubikey-personalization)
* [iniparser](http://ndevilla.free.fr/iniparser/)
* [systemd](http://www.freedesktop.org/wiki/Software/systemd/)
* [cryptsetup](http://code.google.com/p/cryptsetup/)
* [mkinitcpio](https://projects.archlinux.org/mkinitcpio.git/) (Though
  it may be easy to port this to any initramfs that uses systemd)
* [markdown](http://daringfireball.net/projects/markdown/) (HTML documentation)

Additionally it is expected to have `make` and `pkg-config` around to
successfully compile.

Build and install
-----------------

Building and installing is very easy. Just run:

> make

followed by:

> make install

This will place files to their desired places in filesystem.

Usage
-----

First prepare the key. Plug it in, make sure it is configured for
`HMAC-SHA1`, then run:

> ykfde -d /dev/`LUKS-device`

This will store a challenge in `/etc/ykfde.d/` and add a new slot to
your LUKS device. Make sure systemd knows about your encrypted device by
adding a line to `/etc/crypttab.initramfs`. It should read like:

> `mapping-name` /dev/`LUKS-device` -

Add `ykfde` to your hook list in `/etc/mkinitcpio.conf` and rebuild your
initramfs with:

> mkinitcpio -p linux

Reboot and have fun!

Limitation / TODO
-----------------

* At the moment this is specific to Arch Linux. Though everything should
  run with upstream `systemd` just fine anybody has to hook things up with
  [dracut](https://dracut.wiki.kernel.org/) or whatever.
* The challenge is not updated. The file is accessible read only in
  initramfs, but we have no easy way to write it to persistant storage.
  So probably this is a design limitation...
