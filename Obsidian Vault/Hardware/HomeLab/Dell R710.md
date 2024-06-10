
Add a DAS
1) Lenovo SA120
2) Dell Powervault MD1200

Worth looking into as the main system will host
2 disks for the host OS in a mirrored configuration
1 L2ARC disk
1 Cache disk
1-2 hot swap disks

The chassis can hold only 6 drives which by the count above is all 6 drive bays consumed

The rationale for a DAS is to have the JBOD for just storage on ZFS

PROS:
Replaces FreeNAS/TrueNAS box - drives are reusable
Ensures greater expandability as multiple DAS's can be attached

CONS:
May need to get HBA card

Updated 4/26/2023
I decided to move away from a DAS and just get a new workstation chassis (Fractal Design 7 XL) which can hold all the disks I need while allowing me to retire my Dell server entirely.