Py-KMS for FreeBSD
=====

Python based KMS emulator for FreeBSD.
https://github.com/Py-KMS-Organization/py-kms

## Using with Portshaker

Add this Github repo `github:tuaris:freebsd-py-kms` to your **_merge_from** line 
in `/usr/local/etc/portshaker.conf`.  For example.

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

## Upgrading

### g20211211
The default filename for the database file has changed from `/var/db/kms/client.db` 
to `/var/db/kms/pykms_database.db`.  If you want to continue using your old database
rename it using:

```
mv /var/db/kms/client.db /var/db/kms/pykms_database.db
```

