#linux #home-automation #network #nmap #arp-scan 

So today I am working on a means to scan my home network for all IP address.

It shouldn't be hard?

**Tools I intend to use:**
1) [NMAP](https://nmap.org/)
2) [ARP-Scan](https://github.com/royhills/arp-scan)
	- Documentation can be found ==> https://github.com/royhills/arp-scan/wiki/arp-scan-User-Guide
These tools should be enough for an initial test. Let's see what we can see.

**Test scenario:**
Can I see my thermostat? It's a smart device on my WiFi.

**Starting with ARP-Scan:**
I used `arp-scan --localnet` which revealed the following within my home network.

Interface: enp4s0, type: EN10MB, MAC: 70:85:c2:bd:bf:e1, IPv4: 192.168.1.224
Starting arp-scan 1.9.8 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.1.1	68:05:ca:1e:87:d2	Intel Corporate
192.168.1.2	84:8f:69:e3:a1:c8	Dell Inc.
192.168.1.3	00:a0:98:49:de:56	NetApp
192.168.1.4	00:a0:98:10:55:8b	NetApp
192.168.1.8	90:2b:34:06:0e:af	GIGA-BYTE TECHNOLOGY CO.,LTD.
192.168.1.14	00:a0:98:69:76:fb	NetApp
192.168.1.15	5c:ad:76:ce:1b:e1	Shenzhen TCL New Technology Co., Ltd
192.168.1.50	00:1e:67:ce:71:d8	Intel Corporate
192.168.1.66	20:ef:bd:51:78:16	Roku, Inc
192.168.1.104	78:a0:3f:08:4f:d6	Amazon Technologies Inc.
192.168.1.130	38:8c:50:bf:e1:5b	LG Electronics
192.168.1.74	00:31:92:44:05:e1	TP-Link Corporation Limited
192.168.1.143	60:22:32:fa:a3:08	Ubiquiti Networks Inc.
192.168.1.160	00:a0:98:63:32:d1	NetApp
192.168.1.162	00:a0:98:00:e3:1c	NetApp
192.168.1.90	f8:33:31:bc:f3:f3	Texas Instruments
192.168.1.168	e0:63:da:73:1e:a1	Ubiquiti Networks Inc.
192.168.1.192	d0:21:f9:ea:b3:70	Ubiquiti Networks Inc.
192.168.1.194	5c:60:ba:7c:06:32	HP Inc.
192.168.1.202	74:ac:b9:d6:77:0c	Ubiquiti Networks Inc.
192.168.1.137	98:22:6e:84:ae:a3	Amazon Technologies Inc.
192.168.1.220	f4:92:bf:50:ff:18	Ubiquiti Networks Inc.
192.168.1.249	00:a0:98:72:9c:8c	NetApp
192.168.1.145	34:85:18:6a:7f:84	Espressif Inc.
192.168.1.204	00:31:92:d7:e2:a0	TP-Link Corporation Limited
192.168.1.166	c0:91:b9:c8:02:ed	Amazon Technologies Inc.
192.168.1.252	9c:8e:cd:36:8b:9f	Amcrest Technologies
192.168.1.75	00:31:92:43:fe:9a	TP-Link Corporation Limited
192.168.1.152	e6:87:37:63:2b:1f	(Unknown: locally administered)

29 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.9.8: 256 hosts scanned in 1.926 seconds (132.92 hosts/sec). 29 responded

29 IP Addresses and the machine IP Address from which I executed the command.
NICE! I never knew there were nearly 30 machines on my network.

OK! Next test.

**NMAP:**
I ran the following script `sudo nmap -v -sn 192.168.1.0/24` against my internal network and it produced the following output

Starting Nmap 7.93 ( https://nmap.org ) at 2023-07-19 10:09 EDT
Initiating ARP Ping Scan at 10:09
Scanning 255 hosts [1 port/host]
Completed ARP Ping Scan at 10:09, 5.72s elapsed (255 total hosts)
Initiating Parallel DNS resolution of 27 hosts. at 10:09
Completed Parallel DNS resolution of 27 hosts. at 10:09, 0.00s elapsed
Nmap scan report for 192.168.1.0 [host down]
Nmap scan report for Bartender.Drumm.Home (192.168.1.1)
Host is up (0.00024s latency).
MAC Address: 68:05:CA:1E:87:D2 (Intel Corporate)
Nmap scan report for Unifi.drumm.home (192.168.1.2)
Host is up (0.00023s latency).
MAC Address: 84:8F:69:E3:A1:C8 (Dell)
Nmap scan report for 192.168.1.3
Host is up (0.00064s latency).
MAC Address: 00:A0:98:49:DE:56 (NetApp)
Nmap scan report for 192.168.1.4
Host is up (0.00060s latency).
MAC Address: 00:A0:98:10:55:8B (NetApp)
Nmap scan report for 192.168.1.5 [host down]
Nmap scan report for 192.168.1.6 [host down]
Nmap scan report for 192.168.1.7 [host down]
Nmap scan report for 192.168.1.8
Host is up (0.00022s latency).
MAC Address: 90:2B:34:06:0E:AF (Giga-byte Technology)
Nmap scan report for 192.168.1.9 [host down]
Nmap scan report for 192.168.1.10 [host down]
Nmap scan report for 192.168.1.11 [host down]
Nmap scan report for 192.168.1.12 [host down]
Nmap scan report for 192.168.1.13 [host down]
Nmap scan report for 192.168.1.14
Host is up (0.00055s latency).
MAC Address: 00:A0:98:69:76:FB (NetApp)
Nmap scan report for 192.168.1.15
Host is up (0.00013s latency).
MAC Address: 5C:AD:76:CE:1B:E1 (Shenzhen TCL New Technology)
Nmap scan report for 192.168.1.16 [host down]
Nmap scan report for 192.168.1.17 [host down]
Nmap scan report for 192.168.1.18 [host down]
Nmap scan report for 192.168.1.19 [host down]
Nmap scan report for 192.168.1.20 [host down]
Nmap scan report for 192.168.1.21 [host down]
Nmap scan report for 192.168.1.22 [host down]
Nmap scan report for 192.168.1.23 [host down]
Nmap scan report for 192.168.1.24 [host down]
Nmap scan report for 192.168.1.25 [host down]
Nmap scan report for 192.168.1.26 [host down]
Nmap scan report for 192.168.1.27 [host down]
Nmap scan report for 192.168.1.28 [host down]
Nmap scan report for 192.168.1.29 [host down]
Nmap scan report for 192.168.1.30 [host down]
Nmap scan report for 192.168.1.31 [host down]
Nmap scan report for 192.168.1.32 [host down]
Nmap scan report for 192.168.1.33 [host down]
Nmap scan report for 192.168.1.34 [host down]
Nmap scan report for 192.168.1.35 [host down]
Nmap scan report for 192.168.1.36 [host down]
Nmap scan report for 192.168.1.37 [host down]
Nmap scan report for 192.168.1.38 [host down]
Nmap scan report for 192.168.1.39 [host down]
Nmap scan report for 192.168.1.40 [host down]
Nmap scan report for 192.168.1.41 [host down]
Nmap scan report for 192.168.1.42 [host down]
Nmap scan report for 192.168.1.43 [host down]
Nmap scan report for 192.168.1.44 [host down]
Nmap scan report for 192.168.1.45 [host down]
Nmap scan report for 192.168.1.46 [host down]
Nmap scan report for 192.168.1.47 [host down]
Nmap scan report for 192.168.1.48 [host down]
Nmap scan report for 192.168.1.49 [host down]
Nmap scan report for 192.168.1.50
Host is up (0.00029s latency).
MAC Address: 00:1E:67:CE:71:D8 (Intel Corporate)
Nmap scan report for 192.168.1.51 [host down]
Nmap scan report for 192.168.1.52 [host down]
Nmap scan report for 192.168.1.53 [host down]
Nmap scan report for 192.168.1.54 [host down]
Nmap scan report for 192.168.1.55 [host down]
Nmap scan report for 192.168.1.56 [host down]
Nmap scan report for 192.168.1.57 [host down]
Nmap scan report for 192.168.1.58 [host down]
Nmap scan report for 192.168.1.59 [host down]
Nmap scan report for 192.168.1.60 [host down]
Nmap scan report for 192.168.1.61 [host down]
Nmap scan report for 192.168.1.62 [host down]
Nmap scan report for 192.168.1.63 [host down]
Nmap scan report for 192.168.1.64 [host down]
Nmap scan report for 192.168.1.65 [host down]
Nmap scan report for 192.168.1.66
Host is up (0.0051s latency).
MAC Address: 20:EF:BD:51:78:16 (Roku)
Nmap scan report for 192.168.1.67 [host down]
Nmap scan report for 192.168.1.68 [host down]
Nmap scan report for 192.168.1.69 [host down]
Nmap scan report for 192.168.1.70 [host down]
Nmap scan report for 192.168.1.71 [host down]
Nmap scan report for 192.168.1.72 [host down]
Nmap scan report for 192.168.1.73 [host down]
Nmap scan report for 192.168.1.74
Host is up (0.030s latency).
MAC Address: 00:31:92:44:05:E1 (TP-Link Limited)
Nmap scan report for 192.168.1.75
Host is up (0.52s latency).
MAC Address: 00:31:92:43:FE:9A (TP-Link Limited)
Nmap scan report for 192.168.1.76 [host down]
Nmap scan report for 192.168.1.77 [host down]
Nmap scan report for 192.168.1.78 [host down]
Nmap scan report for 192.168.1.79 [host down]
Nmap scan report for 192.168.1.80 [host down]
Nmap scan report for 192.168.1.81 [host down]
Nmap scan report for 192.168.1.82 [host down]
Nmap scan report for 192.168.1.83 [host down]
Nmap scan report for 192.168.1.84 [host down]
Nmap scan report for 192.168.1.85 [host down]
Nmap scan report for 192.168.1.86 [host down]
Nmap scan report for 192.168.1.87 [host down]
Nmap scan report for 192.168.1.88 [host down]
Nmap scan report for 192.168.1.89 [host down]
Nmap scan report for 192.168.1.90
Host is up (0.095s latency).
MAC Address: F8:33:31:BC:F3:F3 (Texas Instruments)
Nmap scan report for 192.168.1.91 [host down]
Nmap scan report for 192.168.1.92 [host down]
Nmap scan report for 192.168.1.93 [host down]
Nmap scan report for 192.168.1.94 [host down]
Nmap scan report for 192.168.1.95 [host down]
Nmap scan report for 192.168.1.96 [host down]
Nmap scan report for 192.168.1.97 [host down]
Nmap scan report for 192.168.1.98 [host down]
Nmap scan report for 192.168.1.99 [host down]
Nmap scan report for 192.168.1.100 [host down]
Nmap scan report for 192.168.1.101 [host down]
Nmap scan report for 192.168.1.102 [host down]
Nmap scan report for 192.168.1.103 [host down]
Nmap scan report for 192.168.1.104
Host is up (0.0019s latency).
MAC Address: 78:A0:3F:08:4F:D6 (Amazon Technologies)
Nmap scan report for 192.168.1.105 [host down]
Nmap scan report for 192.168.1.106 [host down]
Nmap scan report for 192.168.1.107 [host down]
Nmap scan report for 192.168.1.108 [host down]
Nmap scan report for 192.168.1.109 [host down]
Nmap scan report for 192.168.1.110 [host down]
Nmap scan report for 192.168.1.111 [host down]
Nmap scan report for 192.168.1.112 [host down]
Nmap scan report for 192.168.1.113 [host down]
Nmap scan report for 192.168.1.114 [host down]
Nmap scan report for 192.168.1.115 [host down]
Nmap scan report for 192.168.1.116 [host down]
Nmap scan report for 192.168.1.117 [host down]
Nmap scan report for 192.168.1.118 [host down]
Nmap scan report for 192.168.1.119 [host down]
Nmap scan report for 192.168.1.120 [host down]
Nmap scan report for 192.168.1.121 [host down]
Nmap scan report for 192.168.1.122 [host down]
Nmap scan report for 192.168.1.123 [host down]
Nmap scan report for 192.168.1.124 [host down]
Nmap scan report for 192.168.1.125 [host down]
Nmap scan report for 192.168.1.126 [host down]
Nmap scan report for 192.168.1.127 [host down]
Nmap scan report for 192.168.1.128 [host down]
Nmap scan report for 192.168.1.129 [host down]
Nmap scan report for 192.168.1.130 [host down]
Nmap scan report for 192.168.1.131 [host down]
Nmap scan report for 192.168.1.132 [host down]
Nmap scan report for 192.168.1.133 [host down]
Nmap scan report for 192.168.1.134 [host down]
Nmap scan report for 192.168.1.135 [host down]
Nmap scan report for 192.168.1.136 [host down]
Nmap scan report for 192.168.1.137
Host is up (0.35s latency).
MAC Address: 98:22:6E:84:AE:A3 (Amazon Technologies)
Nmap scan report for 192.168.1.138 [host down]
Nmap scan report for 192.168.1.139 [host down]
Nmap scan report for 192.168.1.140 [host down]
Nmap scan report for 192.168.1.141 [host down]
Nmap scan report for 192.168.1.142 [host down]
Nmap scan report for 192.168.1.143
Host is up (0.0039s latency).
MAC Address: 60:22:32:FA:A3:08 (Ubiquiti Networks)
Nmap scan report for 192.168.1.144 [host down]
Nmap scan report for 192.168.1.145
Host is up (0.33s latency).
MAC Address: 34:85:18:6A:7F:84 (Espressif)
Nmap scan report for 192.168.1.146 [host down]
Nmap scan report for 192.168.1.147 [host down]
Nmap scan report for 192.168.1.148 [host down]
Nmap scan report for 192.168.1.149 [host down]
Nmap scan report for 192.168.1.150 [host down]
Nmap scan report for 192.168.1.151 [host down]
Nmap scan report for 192.168.1.152 [host down]
Nmap scan report for 192.168.1.153 [host down]
Nmap scan report for 192.168.1.154 [host down]
Nmap scan report for 192.168.1.155 [host down]
Nmap scan report for 192.168.1.156 [host down]
Nmap scan report for 192.168.1.157 [host down]
Nmap scan report for 192.168.1.158 [host down]
Nmap scan report for 192.168.1.159 [host down]
Nmap scan report for 192.168.1.160
Host is up (0.00066s latency).
MAC Address: 00:A0:98:63:32:D1 (NetApp)
Nmap scan report for 192.168.1.161 [host down]
Nmap scan report for 192.168.1.162
Host is up (0.00061s latency).
MAC Address: 00:A0:98:00:E3:1C (NetApp)
Nmap scan report for 192.168.1.163 [host down]
Nmap scan report for 192.168.1.164 [host down]
Nmap scan report for 192.168.1.165 [host down]
Nmap scan report for 192.168.1.166
Host is up (0.23s latency).
MAC Address: C0:91:B9:C8:02:ED (Amazon Technologies)
Nmap scan report for 192.168.1.167 [host down]
Nmap scan report for 192.168.1.168
Host is up (0.00060s latency).
MAC Address: E0:63:DA:73:1E:A1 (Ubiquiti Networks)
Nmap scan report for 192.168.1.169 [host down]
Nmap scan report for 192.168.1.170 [host down]
Nmap scan report for 192.168.1.171 [host down]
Nmap scan report for 192.168.1.172 [host down]
Nmap scan report for 192.168.1.173 [host down]
Nmap scan report for 192.168.1.174 [host down]
Nmap scan report for 192.168.1.175 [host down]
Nmap scan report for 192.168.1.176 [host down]
Nmap scan report for 192.168.1.177 [host down]
Nmap scan report for 192.168.1.178 [host down]
Nmap scan report for 192.168.1.179 [host down]
Nmap scan report for 192.168.1.180 [host down]
Nmap scan report for 192.168.1.181 [host down]
Nmap scan report for 192.168.1.182 [host down]
Nmap scan report for 192.168.1.183 [host down]
Nmap scan report for 192.168.1.184 [host down]
Nmap scan report for 192.168.1.185 [host down]
Nmap scan report for 192.168.1.186 [host down]
Nmap scan report for 192.168.1.187 [host down]
Nmap scan report for 192.168.1.188 [host down]
Nmap scan report for 192.168.1.189 [host down]
Nmap scan report for 192.168.1.190 [host down]
Nmap scan report for 192.168.1.191 [host down]
Nmap scan report for 192.168.1.192
Host is up (0.00047s latency).
MAC Address: D0:21:F9:EA:B3:70 (Ubiquiti Networks)
Nmap scan report for 192.168.1.193 [host down]
Nmap scan report for 192.168.1.194
Host is up (0.036s latency).
MAC Address: 5C:60:BA:7C:06:32 (HP)
Nmap scan report for 192.168.1.195 [host down]
Nmap scan report for 192.168.1.196 [host down]
Nmap scan report for 192.168.1.197 [host down]
Nmap scan report for 192.168.1.198 [host down]
Nmap scan report for 192.168.1.199 [host down]
Nmap scan report for 192.168.1.200 [host down]
Nmap scan report for 192.168.1.201 [host down]
Nmap scan report for 192.168.1.202
Host is up (0.042s latency).
MAC Address: 74:AC:B9:D6:77:0C (Ubiquiti Networks)
Nmap scan report for 192.168.1.203 [host down]
Nmap scan report for 192.168.1.204
Host is up (0.40s latency).
MAC Address: 00:31:92:D7:E2:A0 (TP-Link Limited)
Nmap scan report for 192.168.1.205 [host down]
Nmap scan report for 192.168.1.206 [host down]
Nmap scan report for 192.168.1.207 [host down]
Nmap scan report for 192.168.1.208 [host down]
Nmap scan report for 192.168.1.209 [host down]
Nmap scan report for 192.168.1.210 [host down]
Nmap scan report for 192.168.1.211 [host down]
Nmap scan report for 192.168.1.212 [host down]
Nmap scan report for 192.168.1.213 [host down]
Nmap scan report for 192.168.1.214 [host down]
Nmap scan report for 192.168.1.215 [host down]
Nmap scan report for 192.168.1.216 [host down]
Nmap scan report for 192.168.1.217 [host down]
Nmap scan report for 192.168.1.218 [host down]
Nmap scan report for 192.168.1.219 [host down]
Nmap scan report for 192.168.1.220
Host is up (0.00063s latency).
MAC Address: F4:92:BF:50:FF:18 (Ubiquiti Networks)
Nmap scan report for 192.168.1.221 [host down]
Nmap scan report for 192.168.1.222 [host down]
Nmap scan report for 192.168.1.223 [host down]
Nmap scan report for 192.168.1.225 [host down]
Nmap scan report for 192.168.1.226 [host down]
Nmap scan report for 192.168.1.227 [host down]
Nmap scan report for 192.168.1.228 [host down]
Nmap scan report for 192.168.1.229 [host down]
Nmap scan report for 192.168.1.230 [host down]
Nmap scan report for 192.168.1.231 [host down]
Nmap scan report for 192.168.1.232 [host down]
Nmap scan report for 192.168.1.233 [host down]
Nmap scan report for 192.168.1.234 [host down]
Nmap scan report for 192.168.1.235 [host down]
Nmap scan report for 192.168.1.236 [host down]
Nmap scan report for 192.168.1.237 [host down]
Nmap scan report for 192.168.1.238 [host down]
Nmap scan report for 192.168.1.239 [host down]
Nmap scan report for 192.168.1.240 [host down]
Nmap scan report for 192.168.1.241 [host down]
Nmap scan report for 192.168.1.242 [host down]
Nmap scan report for 192.168.1.243 [host down]
Nmap scan report for 192.168.1.244 [host down]
Nmap scan report for 192.168.1.245 [host down]
Nmap scan report for 192.168.1.246 [host down]
Nmap scan report for 192.168.1.247 [host down]
Nmap scan report for 192.168.1.248 [host down]
Nmap scan report for 192.168.1.249
Host is up (0.00056s latency).
MAC Address: 00:A0:98:72:9C:8C (NetApp)
Nmap scan report for 192.168.1.250 [host down]
Nmap scan report for 192.168.1.251 [host down]
Nmap scan report for 192.168.1.252
Host is up (0.19s latency).
MAC Address: 9C:8E:CD:36:8B:9F (Amcrest Technologies)
Nmap scan report for 192.168.1.253 [host down]
Nmap scan report for 192.168.1.254 [host down]
Nmap scan report for 192.168.1.255 [host down]
Initiating Parallel DNS resolution of 1 host. at 10:09
Completed Parallel DNS resolution of 1 host. at 10:09, 0.00s elapsed
Nmap scan report for 192.168.1.224
Host is up.
Read data files from: /nix/store/3via75dxlq6chndgafz1r151hwy3yzwp-nmap-7.93/bin/../share/nmap
Nmap done: 256 IP addresses (28 hosts up) scanned in 5.75 seconds
           Raw packets sent: 488 (13.664KB) | Rcvd: 32 (896B)

This is good! It contains the entire 256 address range that is possible within this network and then differentiated the hosts that are UP vs DOWN.

I noticed an issue with the counts of hosts that are UP. The totals do not match.
Re-running both to confirm. 

So... upon a new  scan, there is a discrepancy from each tools. One showing 27 hosts up and the other showing 28.

More research is required to understand why but for now this is enough I am pleased with these 2 tools and the network map they have just provided me.