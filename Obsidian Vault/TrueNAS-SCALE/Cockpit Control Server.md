#linux #cockpit #coding 
Doesn't play nice with [[Ubuntu]]... and  sometimes [[Debian]]

[Here](https://cockpit-project.org/faq#error-message-about-being-offline) is the work around.
TLDR;

#### Error message about being offline

The software update page shows “packagekit cannot refresh cache whilst offline” on a Debian or Ubuntu system.

##### Solution

Create a placeholder file and network interface.

1. Create `/etc/NetworkManager/conf.d/10-globally-managed-devices.conf` with the contents:
    
    ```
     [keyfile]
     unmanaged-devices=none
    ```
    
2. Set up a “dummy” network interface:
    
    ```
     nmcli con add type dummy con-name fake ifname fake0 ip4 1.2.3.4/24 gw4 1.2.3.1
    ```
    
3. Reboot
    

##### Explanation

Ubuntu uses two different network stacks which don’t play so well together.

Cockpit’s software update page uses PackageKit, which checks NetworkManager. However, as the network interfaces are primarily managed by netplan and systemd-networkd in Ubuntu, NetworkManager reports back to PackageKit that the network is offline as a critical error message and stops further actions.

Unfortunately, this means in many Ubuntu configurations, Cockpit’s software update page might fail with a message about not being able to “refresh cache whilst offline”. Other software that relies on PackageKit, such as KDE’s Discover, can also hit this bug.

Additionally, there’s nothing Cockpit itself can do to fix this problem. It’s an “emergent bug” that happens much lower in the software stack. It’s up to Ubuntu to patch the way they’re using netplan and networkd… or, alternatively, PackageKit could provide a workaround. Unfortunately, none of these fixes have been implemented yet.

Meanwhile, to be able to update your server using Cockpit when you get this error, you have to do a workaround similar to the one suggested above.