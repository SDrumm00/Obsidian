
Today I learned about RSync.
It is a great [[Linux 1]] cli tool that allows me to backup my data to my [[ZFS 1]] storage array.

I should create a scheduled job to do this daily or so but for now... here is the syntax for that.

`rsync -av --exclude X --exclude tank /home/scott/ /home/scott/tank/
`

Essentially, the hardest part was to get RSync to ignore certain directories during the creation of the backup. In this case, "tank" and "X" need excluded because they are storage pools themselves and I don't need to backup a backup.

#linux #ZFS 
