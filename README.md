Py-KMS for FreeBSD
=====

Python based KMS emulator for FreeBSD.
https://github.com/SystemRage/py-kms

## Using with Portshaker

Add this Github repo `github:tuaris:freebsd-py-kms` to your **_merge_from** line in `/usr/local/etc/portshaker.conf`.  For example.

```
#---[ Base directory for mirrored Ports Trees ]---
mirror_base_dir="/var/cache/portshaker"

#---[ Directories where to merge ports ]---
ports_trees="default"

use_zfs="yes"
poudriere_dataset="poudriere/poudriere"
poudriere_ports_mountpoint="/usr/local/poudriere/ports"
default_poudriere_tree="default"
default_merge_from="ports github:tuaris:freebsd-py-kms"
```


