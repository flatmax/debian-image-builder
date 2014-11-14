# Debian bootstrapping script for Amazon machine images and Google Compute Engine images #

This script bootstraps a vanilla Debian installation to create either
an Amazon machine image or a Google Compute Engine image.  The image
contains no latent logfiles no .bash\_history or even the apt package
cache.  The machine configuration this script creates has been
thoroughly tested.

* This script has been tested on Debian squeeze and wheezy. *

* To create an AMI this bootstrapper needs to be run on an Amazon EC2
  instance. *

## Usage ##

The script is started with ``./debian-image-builder``.  You can choose
to either bootstrap a Debian AMI (``./debian-image-builder ec2``) or a
Google Compute Engine image (``./debian-image-builder gce``).  Both
modes have sensible defaults and can be configured with options and
plugins.  To see a list of options use ``--help``.  When creating an
AMI the script at least needs to know your AWS credentials.

There are no interactive prompts, the bootstrapping can run entirely
unattended from start till finish.

Some plugins are included in the plugins directory. A list of external
plugins is also provided there. If none of those scratch your itch,
you can of course write your own plugin (see HOWTO.md in the plugins
directory).

## Features ##

### AMI features ###

* EBS booted
* Base installation uses only 289MB
* Base installation bootup time ~45s* (from AMI launch to SSH connectivity)
* Support for both ext* and xfs
* Uses standard Debian Xen kernel from apt
* update-grub creates an actual menu.lst which pvGrub can read
* ec2 system log is not cluttered by grub menu
* ec2 startup scripts:
  * `ec2-get-credentials`: Copies the ec2 keypair to `~/.ssh/authorized_keys`
  * `ec2-run-user-data`: If the userdata starts with `#!` it will be executed
  * `generate-ssh-hostkeys`: Generates hostkeys for sshd on first boot
  * `expand-volume`: Expands the root partition to the volume size

*\*The bootup time was measured with [this script](https://gist.github.com/3813743)*

### Bootstrapper (AMI) features ###

* EBS volume is automatically created, mounted, formatted, unmounted, "snapshotted" and deleted
* AMI is automatically registered with the right kernels for the current region of the host machine
* Supports Debian squeeze and wheezy
* Can create both 32-bit and 64-bit AMIs
* Plugin system to keep the bootstrapping process automated
* The process is divided into simple task based scripts
* Uses only free software in accordance with the [Debian Social Contract](http://www.debian.org/social_contract).
  (In particular: The [EC2 API Tools](http://aws.amazon.com/developertools/351)
  have been replaced with [euca2ools](http://www.eucalyptus.com/download/euca2ools))
