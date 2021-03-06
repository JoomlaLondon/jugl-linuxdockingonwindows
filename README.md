# JUGL Workshop on how to setup Docker on Windows, to run native linux containers.

*NOTE:* Before you start, it is important to prepare the hardware you're working on.  To enable the Windows Subsystem Linux , you need to enable Virtualisation in the bios of your laptop or desktop.  Each bios is different especially between Amd  and Intel systems but you should look for something relatiing to Virtualisation.  The setting is usually found in  Advanced/CPU settings page.  In one AMD system we found it was labelled as "SVM Mode".  Maybe check the documentation of your motherboard/laptop if you have it.  You won't be able to get things running without it.

Once you have it enabled, you should be able to confirm this in the Performance Tab of the Task manager in Windows.  Look for confirmation of Virtualisation enabled.
We will be using Powershell for most of the command line commands.  You may not have used this before but it should come pre-installed with windows.

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

Use these details in the command below...


Set WSL 2 as your default version

##### Open PowerShell as Administrator and run:

```
wsl --set-default-version 2
```


#### Install your Linux distribution of choice

With our Linux kernel now availabl we can grab our distrubution to run on it.  We'll get this from the Microsoft Store.  Search for the Store and find and select your favorite Linux distribution.

Some of the popular one’s are below:

&nbsp;&nbsp;&nbsp;Debian

&nbsp;&nbsp;&nbsp;UbuntuSUSE

&nbsp;&nbsp;&nbsp;Linux Enterprise Server

&nbsp;&nbsp;&nbsp;KaliLinux


The first time you launch a newly installed Linux distribution, a console window will open and you’ll be asked to wait for a minute or two, for files to de-compress and be stored on your PC. All future launches should be much faster.

During this part of the process, you will be asked to create a user account and password for your new Linux distribution.
Store this somewhere other than a postit note.

##### Check for the list of linux distributions:

In Powershell(As Administrator):
```
wsl -l -v
```
![PowerShell Output 3](https://github.com/JoomlaLondon/jugl-linuxdockingonwindows/raw/main/images/snip5.png "PowerShell Output 3")

##### Set the distribution to use WSL 2:

In Powershell(As Administrator):
```
wsl --set-version <distribution name> <versionNumber>
```
![PowerShell Output 4](https://github.com/JoomlaLondon/jugl-linuxdockingonwindows/raw/main/images/snip6.png "PowerShell Output 4")

##### Installing Docker Desktop

At this point we recommend a reboot just to help everything settle in.

#### Download Docker Desktop Stable 2.3.0.2 or a later release:

https://hub.docker.com/editions/community/docker-ce-desktop-windows

Accept the default settings during the install.

Once complete, run Docker Desktop.

Set Docker on WSL to also allow the Distro you installed (see below).
![PowerShell Output](https://github.com/JoomlaLondon/jugl-linuxdockingonwindows/raw/main/images/dockerWSLsetting.png "PowerShell Output")

That should be it.

You have now installed Docker on WSL 2.

In PowerShell(As Administrator), verify docker is running:

```
docker ps
docker run hello-world
```
If you don't see this but didn't see any errors as you went through the process, try restarting the machine, double check for Virtualisation and then reinstall the Linux dist and reboot and then try to reinstall Docker...  If that doesn't work create an issue at github.com/joomlaLondon/jugl-linuxdockingonwindows and I'll do my best to help.


### Advanced Tip

#### Optional: Changing the default image location of Docker using WSL

I wanted to store the images of containers somewhere other than my system drive(SSD).  I tried several things, like using the "root-data" param in the Docker daemon config, none worked.  
The only thing that did work for me was this:

In Powershell(as Administrator)
```
wsl --list -v
```
You should see something like:
```
  NAME                   STATE           VERSION
* docker-desktop         Stopped         2
  docker-desktop-data    Stopped         2
```
Make sure the STATE for both is Stopped, if not, try this:

In Powershell(as Administrator)
```
wsl -t <DistributionName>
```
*do for both distributions*

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

