+++
title = 'How I Almost Deleted Docker'
slug = 'almost-deleted-docker'
date = 2024-01-15T15:00:00+01:00
draft = true
[cover]
image = 'img/00-almost-deleted-docker.webp'
alt = 'How I Almost Deleted Docker'
caption = 'How I Almost Deleted Docker'
+++

Workaround
1. Diagnose your Docker Desktop installation
To determine if you need to re-download and re-install Docker Desktop, please run the following steps.

2. Download the attached check.sh.txt file, rename it to check.sh and set the executable flag with chmod +x check.sh. (Caution: it is never a good idea to just run shell scripts downloaded from the internet. Please take a moment to review the script before executing it.)
Execute the following command to verify the binary in your Docker.app application bundle. The script will exit with 0 if the certificate was correctly verified:

```code
$ ./check.sh /Applications/Docker.app/Contents/Library/LaunchServices/com.docker.vmnetd
```

3. You can also verify files in the /Library/PrivilegedHelperTools folder with:
./check.sh /Library/PrivilegedHelperTools/com.docker.vmnetd and
sudo ./check.sh /Library/PrivilegedHelperTools/com.docker.socket

4. If any of the above commands fail to verify the certificate, you have to re-download and re-install Docker Desktop


## Privileged users
If you face this issue, try the following procedure (requires root user access):

1. Quit Docker Desktop and check that no remaining docker processes are running using the Activity Monitor
2. Run the following commands:

```code
#!/bin/bash

# Stop the docker services
echo "Stopping Docker..."
sudo pkill '[dD]ocker'

# Stop the vmnetd service
echo "Stopping com.docker.vmnetd service..."
sudo launchctl bootout system /Library/LaunchDaemons/com.docker.vmnetd.plist

# Stop the socket service
echo "Stopping com.docker.socket service..."
sudo launchctl bootout system /Library/LaunchDaemons/com.docker.socket.plist

# Remove vmnetd binary
echo "Removing com.docker.vmnetd binary..."
sudo rm -f /Library/PrivilegedHelperTools/com.docker.vmnetd

# Remove socket binary
echo "Removing com.docker.socket binary..."
sudo rm -f /Library/PrivilegedHelperTools/com.docker.socket

# Install new binaries
echo "Install new binaries..."
sudo cp /Applications/Docker.app/Contents/Library/LaunchServices/com.docker.vmnetd /Library/PrivilegedHelperTools/
sudo cp /Applications/Docker.app/Contents/MacOS/com.docker.socket /Library/PrivilegedHelperTools/
```

3. Restart Docker Desktop
4. If that still doesn't work, download one of the currently supported release from the Release notes, re-install the application and then finally re-apply step 2.

<br/>

{{< bmc-button slug="emmysteven" >}}

<br/>

