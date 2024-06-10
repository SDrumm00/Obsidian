https://btrfs.readthedocs.io/en/latest/

Series of reads
1) https://fedoramagazine.org/working-with-btrfs-general-concepts/
2) https://fedoramagazine.org/working-with-btrfs-subvolumes/
3) https://fedoramagazine.org/working-with-btrfs-snapshots/


#### BTRFS Backup Tool
https://digint.ch/btrbk/
BTRBK

**btrbk** is a backup tool for btrfs subvolumes, taking advantage of btrfs specific capabilities to create atomic snapshots and transfer them incrementally to your backup locations.

The source and target locations are specified in a config file, which allows to easily configure simple scenarios like "laptop with locally attached backup disks", as well as more complex ones, e.g. "server receiving backups from several hosts via ssh, with different retention policy".

### Key Features:

-   Atomic snapshots
-   Incremental backups
-   Flexible retention policy
-   Backups to multiple destinations
-   Transfer via ssh
-   Robust recovery from interrupted backups (for removable and mobile devices)
-   Archive to offline storage
-   Encrypted backups to non-btrfs storage
-   Wildcard subvolumes (useful for docker and lxc containers)
-   Transaction log
-   Comprehensive list and statistics output
-   Resolve and trace btrfs parent-child and received-from relationships
-   List file changes between backups
-   Calculate accurate disk space usage based on block regions

