# JUGL Workshop on how to setup Docker on Windows to run native linux containers

*NOTE:* Before you start, it is important to prepare the hardware you're working on.  To enable the Windows Subsystem Linux , you need to enable Virtualisation in the bios of your laptop or desktop.  Each bios is different especially between Amd  and Intel systems but you should look for something relatiing to Virtualisation.  The setting is usually found in  Advanced/CPU settings page.  In one system we found it was disguised as "SVM Mode".  Maybe check the documentation of your motherboard/laptop if you have it.  You won't be able to get things running without it.

Once you have it enabled, you should be able to confirm this in the Performance Tab of the Task manager in Windows.  Look for confirmation of Virtualisation

## Enable WSL 2 feature on Windows.

##### Open PowerShell as Administrator and run:

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```
![PowerShell Output](https://github.com/JoomlaLondon/jugl-linuxdockingonwindows/raw/main/images/snip1.png "PowerShell Output")

Enable the ‘Virtual Machine Platform’ optional component

##### Open PowerShell as Administrator and run:

```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
![PowerShell Output 2](https://github.com/JoomlaLondon/jugl-linuxdockingonwindows/raw/main/images/snip2.png "PowerShell Output 2")

#### Restart your machine at this point to complete the WSL install and update to WSL 2.

Install the linux kernel package required to update the WSL version to WSL 2.

##### Details of kernel are in below link:

https://docs.microsoft.com/en-us/windows/wsl/wsl2-kernel

##### Installer is at:

https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi


Set WSL 2 as your default version

##### Open PowerShell as Administrator and run:

```
wsl --set-default-version 2
```


#### Install your Linux distribution of choice

Open the Microsoft Store and select your favorite Linux distribution.

Some of the popular one’s are below:

   Debian

   UbuntuSUSE

   Linux Enterprise Server

   KaliLinux


The first time you launch a newly installed Linux distribution, a console window will open and you’ll be asked to wait for a minute or two for files to de-compress and be stored on your PC. All future launches should take less than a second.

You will then need to create a user account and password for your new Linux distribution.
Store this somewhere other than a postit note.

##### Check for the list of linux distributions:

```
wsl -l -v
```
![PowerShell Output 3](https://github.com/JoomlaLondon/jugl-linuxdockingonwindows/raw/main/images/snip5.png "PowerShell Output 3")

##### Set the distribution to use WSL 2:

```
wsl --set-version <distribution name> <versionNumber>
```
![PowerShell Output 4](https://github.com/JoomlaLondon/jugl-linuxdockingonwindows/raw/main/images/snip6.png "PowerShell Output 4")

##### Installing Docker Desktop

At this point we recommend a reboot just to get everything in check.

#### Download Docker Desktop Stable 2.3.0.2 or a later release:

https://hub.docker.com/editions/community/docker-ce-desktop-windows

Accept the default settings during install
![PowerShell Output](https://github.com/JoomlaLondon/jugl-linuxdockingonwindows/raw/main/images/dockerWSLsetting.png "PowerShell Output")

Run Docker Desktop.
Set Docker on WSL to also allow the Distro you installed.

That’s it.

You have now installed docker on WSL 2.

In a PowerShell window, verify docker is running:

```
docker ps
docker run hello-world
```

### Advanced Tip

#### Changing the default image location of Docker using WSL

I tried several things at first, like using "root-data" param in daemon config, none worked.  
The only thing that did work for me was this:
Open your command prompt:
```
wsl --list -v
```
You should be able to see, make sure the STATE for both is Stopped.
```
  NAME                   STATE           VERSION
* docker-desktop         Stopped         2
  docker-desktop-data    Stopped         2
```

Export docker-desktop-data into a file:
```
wsl --export docker-desktop-data "D:\Docker\wsl\data\docker-desktop-data.tar"
```
Unregister docker-desktop-data from wsl, note that after this, your ext4.vhdx file would automatically be removed:
```
wsl --unregister docker-desktop-data
```
Import the docker-desktop-data back to wsl, but now the ext4.vhdx would reside in different drive/directory:
```
wsl --import docker-desktop-data "D:\Docker\wsl\data" "D:\Docker\wsl\data\docker-desktop-data.tar" --version 2
```
Re-start docker and should show any images you download but from the new location.



##### CREDITs:

[@avinashsingh post on Hackernoon.com](https://hackernoon.com/how-to-run-docker-linux-containers-natively-on-windows-ti1i3uxr)
[StackOverflow](https://stackoverflow.com/questions/62441307/how-can-i-change-the-location-of-docker-images-when-using-wsl2-with-windows-10-h)

