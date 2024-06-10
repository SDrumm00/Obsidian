# [[Unraid]] GPU Pass-Thru

#Linux #virtualization #GPU #NVidia

## How to make the GPU go puurrrr

**NOTE:** The below info is tailored for a Windows guest but I think it can be expanded to be relevant for Windows and [[Linux 1]]

Cite:
- https://forums.unraid.net/topic/133563-gpu-passthrough-is-easy-heres-how/


## GPU passthough is much easier than made out.

You don't need to do any of the stuff in videos..

### Step 1:
1. Set up the VM with software VNC.
2. Load the VM.
3. Click to open VNC in web browser.
4. Install windows etc.
5. Set up windows user.
6. Close VM.

### Step 2:
Then Go to top menu choose unraid -> tool system -> devices scroll down and tick the NVIDIA GPU or AMD GPU its soundcard.

1. Reboot unraid.
2. Edit VM.
3. Add 2nd graphics card choose your AMD or Nvidia device.
4. Add sound card choose your AMD or Nvidia device.
5. Click Apply.
6. Restart VM.

### Step 3:
It should automatically install basic drivers for the card.
1. Now go to amd or nvidia websites and download the drivers for your card.
2. Run the installer for your card.
3. Reboot VM.

Now you have a gaming VM.
Enjoy. 
