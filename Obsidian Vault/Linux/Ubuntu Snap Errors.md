When using Ubuntu, the sanp packages sometimes cause an error when updating and are currently running in the examples of Spotify and Firefox which are always running on my workstation.

Snap Documentation
https://snapcraft.io/docs/keeping-snaps-up-to-date

How to close the Snap Store itself and refresh it
`killall snap-store`

Then tell snapd to update all snaps:

`sudo snap refresh`

#Ubuntu 