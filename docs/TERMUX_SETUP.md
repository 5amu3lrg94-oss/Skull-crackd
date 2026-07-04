# Termux setup for Skull-crackd

This document explains how to set up an unrooted Android device to run Skull-crackd tooling for educational, authorized network testing using Termux and a remote capture appliance.

IMPORTANT: You must have explicit authorization to capture or test the target network or devices. This guide does NOT enable monitor mode, packet injection, or any bypass of device protections on the Android device.

Prerequisites
- Android device (unrooted)
- Termux installed (prefer F‑Droid: https://f-droid.org/packages/com.termux/)
- A separate remote capture appliance you control (recommended: Raspberry Pi or Linux VM) with SSH access

Quick setup (Termux)
1. Open Termux and update package lists:

   pkg update -y && pkg upgrade -y

2. Install common packages used by Skull-crackd (adjust per project needs):

   pkg install -y git openssh python nodejs proot-distro tsu curl wget

Note: `tsu` is not rooting; it’s a convenience in some Termux environments. Do NOT use it to escalate privileges.

3. Clone this repository and run the setup script:

   git clone https://github.com/5amu3lrg94-oss/Skull-crackd.git
   cd Skull-crackd
   bash scripts/setup-termux.sh

What the Termux setup script does
- Detects Termux environment and installs recommended packages.
- Optionally installs `proot-distro` and creates a minimal Debian/Ubuntu userland for packages not available in Termux.
- Generates an SSH key pair in ~/.ssh/skull_id if none exists and gives instructions to add the public key to your remote capture appliance.
- Writes a helper script `skull-remote-capture` that streams a remote tcpdump over SSH to a local pcap file.

Limitations on unrooted Android
- You cannot enable Wi‑Fi monitor mode or perform raw packet injection from an unrooted Android device.
- Tools that require monitor mode (airodump-ng, aireplay-ng) will not work on unrooted Android hardware.
- Use a remote capture appliance you control for low-level capture (see docs/NETWORK_SETUP.md).

Recommended workflow
1. Use a Raspberry Pi (or Linux VM) as the capture host. Attach it to the network or to a USB Wi‑Fi adapter that supports monitor mode.
2. From Termux on your device, use SSH to trigger captures on the remote appliance or fetch pcaps.
3. Analyze captures locally in Termux (tshark) or transfer to a desktop for Wireshark analysis.

If you need binaries for Android (arm/arm64), provide the build scripts or tell me which components need cross‑compiling and I can add a build guide.
