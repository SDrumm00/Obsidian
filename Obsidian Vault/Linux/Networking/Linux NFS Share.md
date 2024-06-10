This applies to my workstation as I use NFS for accessing my shared drives on my NAS as well as SAMBA protocols.


First locate the fstab file located in the
`/etc/fstab`

Then enter the following
`{NAS-MOUNT}:/mnt/tank/X /home/{user}/X				  nfs	  defaults        0 0`
Where NAS-MOUNT = the IP Address of the host and {user}= scott

Note that some systems require the nfs-common package installed in order to mount an NFS Share. This depends on the specific distribution being used such as [[Debian]]


