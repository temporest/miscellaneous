1. Install OpenSSH in Termux

First, make sure you have OpenSSH installed in Termux:
```sh
pkg update && pkg upgrade
pkg install openssh
```

Then set a password for the Termux user account to login using SSH.
```sh
passwd
```

2. Start the SSH Service

You can start the SSH server manually with the following command:
```sh
sshd
```

If you encounter an error like "no hostkeys available", then run 
```sh
ssh-keygen -A
```
Then try sshd again.

3.5. Find Your Username and IP Address
Get your username and the device's local IP address using these commands:
```sh
whoami
ifconfig # Look for the 'inet' value under the 'wlan0' interface
```

3. Set Up a Termux Boot Script
Termux doesn't have a built-in feature to run services on boot, but you can use the Termux:Boot app to achieve this.

a. Install Termux:Boot

b. Create the Boot Script
Open Termux and create a directory for the boot scripts if it doesn’t exist:
```sh
mkdir -p ~/.termux/boot
```
Create a new script file for starting the SSH server:
```sh
nano ~/.termux/boot/start_sshd.sh
```
Add the following content to start the SSH server when Termux starts:
```sh
#!/data/data/com.termux/files/usr/bin/bash
sshd
```
Save and exit the file by pressing Ctrl + X, then Y, and Enter.

c. Make the Script Executable
Now, make the script executable:
```sh
chmod +x ~/.termux/boot/start_sshd.sh
```
4. Reboot Your Device
After completing these steps, the SSH server should start automatically every time your device boots up.
You can test it by rebooting your device and then checking if you can SSH into it. If you set up SSH correctly, you should be able to connect to your Termux session via SSH using your device’s IP address and the port you configured (default is port 22).

5. Connect from Another Device
From your computer or another device on the same local network, connect using an SSH client (like OpenSSH, PuTTY, etc.). Remember to specify port 8022 with the -p flag:
```sh
ssh <username>@<android_device_ip_address> -p 8022
```
For example: ssh usertemp@192.168.1.100 -p 8022
