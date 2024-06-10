
[[Debian]] setup is weird
Ensure that the modules are loaded

`pactl load-module module-bluetooth-discover`

cite ==> https://askubuntu.com/questions/1115671/blueman-protocol-not-available

after some additional reading, this appears to be related to Pulse Audio

https://wiki.debian.org/BluetoothUser

You may also want to have Pulseaudio automatically connect to the newly discovered output, by adding this to ~/.config/pulse/default.pa:

**automatically switch to newly-connected devices**
load-module module-switch-on-connect


